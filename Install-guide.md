These instructions will guide you through the process of setting up a reddit clone for the first time. We also have an [[automated install script for Ubuntu Linux|reddit install script for Ubuntu]].

## Prerequisites 

Before continuing with this guide, make sure you have all of reddit's many dependencies installed. See one of the following guides for details.

* [[Basic list of dependencies|Dependencies]]
* [[Debian Dependencies]]
* [[Fedora Dependencies]]
* [[Mac OS X Dependencies]]

**This guide will assume that you are installing reddit as user `reddit` in the directory `/home/reddit`.** If this is not the case, modify the examples accordingly.

## Get the code

Clone the git repository available here on github.

```bash
$ git clone git://github.com/reddit/reddit.git
```

Once this is done, you'll need to install the python module dependencies.

```bash
$ cd reddit/r2
$ sudo python setup.py develop
$ make
```

The `setup.py` script only needs to be run at installation time, but the Makefile will need to be rerun any time you modify a Cython file (`*.pyx`) so that the code can be compiled.     

## PostgreSQL

PostgreSQL is reddit's primary data store. It is used for storing data on Accounts, Subreddits, Links, Comments, Votes, etc. In production, we use many separate database clusters, but for sites with less traffic (or test instances) a single postgres database should suffice.

### Initialize postgres

*This section may be unnecessary on your system. Check if your installation of postgres created a default database and start scripts for you.*

If one doesn't already exist, create an account for postgres to run under. This guide will assume the name `postgres`.

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

Then add reddit's SQL functions to the schema.

```bash
$ cd ~/reddit/
$ psql -U reddit reddit < sql/functions.sql
```

### Populate with test data

For testing purposes, you can generate random test data for your reddit install.

```bash
$ cd ~/reddit/r2
$ paster shell example.ini
```
```python
>>> from r2.models import populatedb
>>> populatedb.populate()
```

## Cassandra

## RabbitMQ 

## memcached

## Cron Jobs

## Test it out