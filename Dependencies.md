## Servers

Dependency   | Version | Ubuntu Package(s)                      | Description
-------------|---------|----------------------------------------|----------------------------------------------------------
PostgreSQL   | 9.0+    | `postgresql`                           | Robust RDBMS. Used as primary data store.
Cassandra    | 1.0+  | `cassandra` in `ppa:reddit`              | Distributed database. Slowly becoming primary data store. 
memcached    | 1.4+    | `memcached`                            | Fast in-memory caching server. Used throughout reddit.
RabbitMQ     | 2.0+    | `rabbitmq-server`                      | AMQP server. Used for offline processing.
HAProxy      |         | `haproxy`                              | Load balancer for distributing requests to app servers.

## Libraries

Dependency   | Version | Ubuntu Package(s)                      | Description
-------------|---------|----------------------------------------|----------------------------------------------------------
Python       | 2.7.x   | `python-dev`                           | Headers for building python modules.
Setuptools   |         | `python-setuptools`                    | Provides `easy_install` and support for `setup.py`.
Profiler     |         | `python-profiler`                      | Enables reddit app to provide profile information.
PIL          |         | `python-imaging`                       | Used for image manipulation (thumbnails, CAPTCHAs, etc.)
libmemcached | 0.32+   | `libmemcached5` and `libmemcached-dev` | For communication with memcached.
libpqxx      |         | `libpqxx-dev`                          | C++ library for PostgreSQL.
gettext      |         | `gettext`                              | Used for i18n support.

## Misc. Utilities

Dependency   | Version | Ubuntu Package(s)                      | Description
-------------|---------|----------------------------------------|----------------------------------------------------------
Git          |         | `git-core`                             | reddit code is stored in Git.
GCC / G++    |         | `g++`                                  | GNU C++ Compiler. Used for compiling python modules etc.
Cython       |         | `cython`                               | Performance-intensive code is in Cython for speed.
Subversion   |         | `subversion`                           | Needed to install some python dependencies.
daemontools  |         | `daemontools` and `daemontools-run`    | Simple daemon management system.
optipng      |         | `optipng`                              | Used to compress auto-generated CSS spritemap.
psql         |         | `postgresql-client`                    | CLI client for managing PostgreSQL.
