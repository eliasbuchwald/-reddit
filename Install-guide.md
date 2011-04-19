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
reddit:~$ git clone git://github.com/reddit/reddit.git
```

Once this is done, you'll need to install the python module dependencies.

```bash
$ cd reddit/r2
$ sudo python setup.py develop
$ make
```
    
## PostgreSQL

## Cassandra

## RabbitMQ 

## memcached

## Cron Jobs

## Test it out