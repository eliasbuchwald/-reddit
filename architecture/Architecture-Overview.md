In describing the architecture of reddit, it is useful to make a distinction
between "reddit" the software and "reddit.com" the main user and developer of
that software. reddit is able to run on a single machine up through data center
with hundreds of machines. reddit.com is at the latter end of the spectrum and
employs a few tricks that are outside of the reddit software itself as well.
This architectural overview starts by describing what is intrinsic to reddit,
and then goes on to explain what is currently set up for reddit.com.

# reddit (the software)

Unlike many other high-traffic sites, reddit is a very monolithic piece of
software, for better or worse. The reddit application takes HTTP requests and
does all the necessary work itself to fetch data from databases and build a
proper response.

There are two core permanent data stores that reddit uses, PostgreSQL and
Cassandra. Increasingly, ZooKeeper is becoming important for some forms of data
storage, but its use remains optional at the code level right now. Memcached is
used heavily to speed up many forms of lookup.

A fair (and increasing) amount of reddit's write workload is done outside of
user requests to reduce the effect of processing slowdowns. Changes to be
executed are queued up by a RabbitMQ messaging server and distributed out to
backend jobs for processing.

## PostgreSQL

reddit uses [PostgreSQL](http://en.wikipedia.org/wiki/Postgres) in two distinct
ways: through the *ThingDB* model, which is akin to a key/value store, and
through more traditional relational models.

### ThingDB

The ThingDB model is the core Postgres data persistence mechanism for most of
the objects that people would associate with reddit e.g. Links, Comments,
Accounts, and Subreddits. In short, this data model is a mostly-schemaless
key/value store that uses two tables for each thing type, its "thing" table and
its "data" table.

The first table is the "thing" table. It has a fixed set of columns common to
all things such as ID, whether or not the thing is deleted or marked spam, and
the thing's upvote / downvote counts. Some of these columns are repurposed when
they don't have a useful meaning for the thing type in question (e.g. a
Subreddit's "ups" is its number of subscribers). Having these frequently used
attributes in a fixed schema makes it easy to do relatively fast sorts on the
things.

ThingDB's flexibility comes in through the "data" table. For every attribute
on a thing that isn't represented by a column in the thing table, a row is
created in the data table. This allows zero-effort flexibility in the schema of
things, but does mean higher overhead via joins and the like to fetch the data.

Lookups done through the ThingDB model can fetch individual items or groups of
items in batch. Writes are always done one item at a time.

### Relational data

There is some data that's stored in a more traditional relational form,
including traffic statistics and transaction information for selling ads and
gold subscriptions. This isn't terribly unique to reddit and not worth much
more description.

## Cassandra

[Apache Cassandra](http://en.wikipedia.org/wiki/Apache_cassandra) is a
distributed key/value datastore. It offers transparent
[sharding](http://en.wikipedia.org/wiki/Shard_(database_architecture\)) and is
resistant to hardware failures.

## Memcached

[Memcached](http://en.wikipedia.org/wiki/Memcached) is used very heavily
throughout reddit to add fast-access caching. There are several distinct ways

### sgm

### pagecache

### rendercache

### memoize

## Asynchronous messages via RabbitMQ

Many tasks take a large amount of time to execute and would be ill suited to happen
in-request while the user is waiting.

## ZooKeeper

# reddit.com (the site)

TODO
