[reddit has a PPA on launchpad.net](https://launchpad.net/~reddit/+archive/ppa) where packages can be found for Ubuntu 12.04 Precise Pangolin. If a dependency below cannot be satisfied by the Ubuntu 12.04 archives, it will be marked `reddit ppa` and can be fetched from there. Alternative package managers may provide these packages in the right version natively.

## Servers

In a simple environment, these can all be run on one machine. For a larger site, they will almost certainly need to be split out to different hosts.

Dependency   | Version | Ubuntu Package(s)                      | Description
-------------|---------|----------------------------------------|----------------------------------------------------------
PostgreSQL   | 9.0+    | `postgresql`                           | Robust RDBMS. Used as primary data store.
Cassandra    | 1.0+    | `cassandra` in (reddit ppa)            | Distributed database. Slowly becoming primary data store. 
memcached    | 1.4+    | `memcached`                            | Fast in-memory caching server. Used throughout reddit.
RabbitMQ     | 2.0+    | `rabbitmq-server`                      | AMQP server. Used for offline processing.
HAProxy      |         | `haproxy`                              | Load balancer for distributing requests to app servers.
stunnel      | patched | `stunnel` (reddit ppa)                 | For HTTPS support. Needs to be a version with the X-Forwarded-For patches on top.

## Libraries

The following libraries are prerequisites before an install can begin. With these requirements satisfied, `setup.py` should be able to take care of the remaining python dependencies.

Dependency   | Version | Ubuntu Package(s)                      | Description
-------------|---------|----------------------------------------|----------------------------------------------------------
Setuptools   | 0.6.16+ | `python-setuptools`                    | Python package management.
Python       | 2.7.x   | `python-dev`                           | Headers for building python modules.
libmemcached | 0.32+   | `libmemcached-dev`                     | For communication with memcached.
libpq        |         | `libpq-dev`                            | C library for PostgreSQL.
libxml2      |         | `libxml2-dev`                          | XML Parsing Library (for python's `lxml`)
libxslt1     |         | `libxslt1-dev`                         | XSLT library (for python's `lxml`)
PIL          |         | `python-imaging`                       | This is not strictly necessary as `setup.py` will install it. However, a PyPI-installed copy of PIL will not work out of the box on a Debian/Ubuntu install because it will have trouble finding the C libraries for JPEG / PNG etc.

## Misc. Utilities

Dependency   | Version | Ubuntu Package(s)                      | Description
-------------|---------|----------------------------------------|----------------------------------------------------------
Git          |         | `git-core`                             | reddit code is stored in Git.
GCC          |         | `gcc`                                  | Used for compiling python modules etc.
optipng      |         | `optipng`                              | Used to compress auto-generated CSS spritemap and thumbnails.
jpegoptim    |         | `jpegoptim`                            | Used to compress thumbnails.
psql         |         | `postgresql-client`                    | CLI client for managing PostgreSQL. Used by some processing jobs.
make         |         | `make`                                 | Build tool.
gettext      |         | `gettext`                              | Tools for internationalization.
node.js      | 0.6.19+ | `nodejs` (reddit ppa)                  | Javascript environment.
less.js      | 1.4+    | `node-less` (reddit ppa)               | CSS Precompiler.
