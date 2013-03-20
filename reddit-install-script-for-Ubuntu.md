The reddit install script is a bash script designed for use on Ubuntu Linux. It is designed for 12.04 (Precise Pangolin). The script will install the majority of the reddit stack from scratch. ***Don't run this on a production system or anything with valuable data. We recommend using a dedicated environment (such as a VM) with at least 4GB of RAM***

## Usage
1. Get the [latest version of the install script](https://github.com/reddit/reddit/blob/master/install-reddit.sh).
2. Run the script as root.

```bash
$ wget https://raw.github.com/reddit/reddit/master/install-reddit.sh
$ chmod +x install-reddit.sh
$ sudo ./install-reddit.sh
```

reddit is now available and listening on http://0.0.0.0:8001/, although many of the links will be broken unless you change the domain key in `~/reddit/r2/run.ini` to accurately reflect your environment.

## Test data

To populate the database with test data, including a variety of Accounts, Subreddits, Links and Comments do the following.

```bash
$ cd ~reddit/reddit/r2
$ sudo -u reddit paster shell run.ini
```
```python
>>> from r2.models import populatedb
>>> populatedb.populate()
```

This will take a while and spew a lot of trace to the console, but the result is a reddit with plenty of test data.

## Troubleshooting

See [[FAQ]] for some common issues.