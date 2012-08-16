These instructions will guide you through the process of setting up a
reddit clone for the first time. We also have an [[automated install
script for Ubuntu Linux 11.04|reddit install script for Ubuntu]].

## Prerequisites

Before continuing with this guide, make sure you have all of [[reddit's
many dependencies|Dependencies]] installed.

**This guide will assume that you are installing reddit as user `reddit`
in the directory `/home/reddit`.** If this is not the case, modify the
examples accordingly.

## Get the code

Clone the git repository available here on github.

```bash
$ git clone git://github.com/reddit/reddit.git
```

Once this is done, you'll need to install the python module dependencies.

```bash
$ cd reddit/r2
$ make pyx
$ python setup.py build
$ sudo python setup.py develop
$ make
```

The `setup.py develop` command only needs to be run at installation time,
but the Makefile will need to be rerun any time you modify a Cython file
(`*.pyx`) so that the code can be compiled.

## PostgreSQL

PostgreSQL is reddit's primary data store. It is used for storing data
on Accounts, Subreddits, Links, Comments, Votes, etc. In production,
we use many separate database clusters, but for sites with less traffic
(or test instances) a single postgres database should suffice.

### Initialize postgres

*This section may be unnecessary on your system. Check if your
installation of postgres created a default database and start scripts
for you.*

If one doesn't already exist, create an account for postgres to run
under. This guide will assume the name `postgres`.

```bash
$ adduser postgres
```

Create a directory for postgres to store its data in.

```bash
$ sudo mkdir -p /usr/local/pgsql/data
$ sudo chown postgres /usr/local/pgsql/data
```

Then, initialize the database.

```bash
$ sudo -u postgres initdb -D /usr/local/pgsql/data
```

Finally, start up the server. 

```bash
$ sudo -u postgres postgres -D /usr/local/pgsql/data
```

### Create the database

Create a database for reddit's data.

```bash
$ createdb -E utf8 reddit
```

And a user for the code to connect with.

```bash
$ sudo -u postgres psql reddit
> CREATE USER reddit WITH PASSWORD 'reddit';
> GRANT ALL PRIVILEGES ON DATABASE reddit TO reddit;
> \q
```

Then add reddit's SQL functions to the schema.

```bash
$ cd ~/reddit/
$ psql -U reddit reddit < sql/functions.sql
```

## Cassandra

Cassandra is a vital component the reddit architecture that stores many
pieces of data used throughout the site.

You must create the keyspace for reddit and the `permacache` column family.

```
$ cassandra-cli -h localhost
[default@unknown] create keyspace reddit with strategy_options = [{replication_factor:1}];
[default@unknown] use reddit;
[default@unknown] create column family permacache with column_type = 'Standard' and comparator = 'BytesType';
```

The reddit application will create the rest of the required column families automatically.

## RabbitMQ 

RabbitMQ is used for asynchronous job processing. Jobs are pushed onto
a set of queues by user actions (such as creating a post or comment)
for tasks that need not be done during the POST. As such, in addition
to getting rabbit running, there are a set of services responsible for
removing jobs from these queues covered under the services section.

Configuration of rabbit is relatively simple.

```bash
$ sudo rabbitmqctl add_vhost /
$ sudo rabbitmqctl add_user reddit reddit
$ sudo rabbitmqctl set_permissions -p / reddit ".*" ".*" ".*"
```

## memcached

Almost everything in reddit depends on memcached running, and you won't
be able to do much without it.

Most package managers will set up run scripts for memcached
automatically. To check if it is already running, use telnet.

```bash
$ telnet localhost 11211
```

If that is unable to connect, you must start the memcache daemon.

```bash
$ memcached 
```

## Test your installation

Before continuing, make sure that reddit is able to start up and connect
to the databases.

```bash
$ cd ~/reddit/r2
$ paster serve --reload example.ini http_port=8081
```

You should be able to access reddit at <http://127.0.0.1:8081/>.

### Troubleshooting

Following are some of the more commonly seen problems at this point in
the installation.

<table>
<thead>
<tr><th>Error</th><th>Resolution</th></tr>
</thead>
<tbody>
<tr>
    <td>ImportError: No module named wrapped</td>
    <td>
         You need to compile the Cython modules.

```bash
$ cd ~/reddit/r2
$ make
```
    </td>
</tr>
<tr>
    <td>ImportError: No module named rails.asset_tag</td>
    <td>setup.py installed the wrong versions of some dependencies. You must downgrade them.

```bash
$ sudo easy_install "Paste==1.7.2-reddit-0.2"
$ sudo easy_install "webhelpers==0.6.4"
```
</td>
</tr>
<tr>
    <td>(OperationalError) FATAL:  password authentication failed for user "reddit"</td>
    <td>
        The postgres user "reddit" has the wrong password. You should recreate the postgres user "reddit" with password "password".

```bash
reddit$ su postgres
postgres$ dropuser reddit
postgres$ createuser -P reddit
```
    </td>
</tr>
</tbody>
</table>

## Populate with test data

For testing purposes, you can generate random test data for your reddit install.

```bash
$ cd ~/reddit/r2
$ paster shell example.ini
```
```python
>>> from r2.models import populatedb
>>> populatedb.populate()
```

Note: this will also create a reddit account named `reddit` with password
`password`.

## Services and Cron Jobs

### Configuration

In `~/reddit/r2`, there is a script called `updateini.py` which reads ini
files and applies differences from a specified `.update` file. This means
that you can make changes to the provided `example.ini` by modifying a
`.update` file without worrying about merge issues down the line.

To take advantage of this infrastructure do the following.

```bash
$ cd ~/reddit/r2
$ touch run.update
$ make
```

### Services

reddit's various services are managed with Upstart. You need a little bit of
configuration to get started, change the settings according to your configuration:

```bash
# cat > /etc/default/reddit <<REDDIT
export REDDIT_ROOT=$REDDIT_HOME/reddit
export REDDIT_INI=$REDDIT_HOME/reddit/r2/run.ini
export REDDIT_USER=$REDDIT_USER
export REDDIT_CONSUMER_CONFIG=$REDDIT_HOME/consumer-counts
alias wrap-job=$REDDIT_HOME/reddit/scripts/wrap-job
alias manage-consumers=$REDDIT_HOME/reddit/scripts/manage-consumers
REDDIT
```

In the `consumer-counts` file mentioned above, put the number of queue processors
you'd like for each type:

```
scraper_q       1
commentstree_q  1
newcomments_q   1
vote_comment_q  1
vote_link_q     1
```

Then, copy the job configuration files from the upstart/ directory to
`/etc/init`.

```bash
$ sudo cp ~/reddit/upstart/* /etc/init/
```

You can then start up all the processors with:

```bash
$ sudo initctl emit reddit-start
```

### Crons

There are several jobs that need to be run periodically to update the
site. These jobs are also managed with upstart. Following is a recommended
cron setup. See [[Cron Jobs]] for information on what each of these does.

```cron
0    3 * * * root /sbin/start --quiet reddit-job-update_sr_names
30  16 * * * root /sbin/start --quiet reddit-job-update_reddits
0    * * * * root /sbin/start --quiet reddit-job-update_promos
*/5  * * * * root /sbin/start --quiet reddit-job-clean_up_hardcache
*    * * * * root /sbin/start --quiet reddit-job-email
*/2  * * * * root /sbin/start --quiet reddit-job-broken_things
*/2  * * * * root /sbin/start --quiet reddit-job-rising
```

## Troubleshooting

Check the [[FAQ]] for help with any issues you may be encountering at
this point.