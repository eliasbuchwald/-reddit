The reddit install script is a bash script designed for use on Ubuntu Linux. It is designed for 12.04 (Precise Pangolin). The script will install the majority of the reddit stack from scratch. ***Don't run this on a production system or anything with valuable data. We recommend using a dedicated environment (such as a VM) with at least 4GB of RAM***

## Usage
1. Get the [latest version of the install script](https://github.com/reddit/reddit/blob/master/install-reddit.sh).
2. Run the script as root.

```bash
$ wget https://raw.github.com/reddit/reddit/master/install-reddit.sh
$ chmod +x install-reddit.sh
$ editor install-reddit.sh
```

Seriously, stop here and *read* the script. It's got some helpful information that can prevent common issues. This is also where you can change the domain name for your local reddit install. Resolving to the appropriate domain name is beyond the scope of this document, but the easiest thing is probably editing `/etc/hosts`. Then just run the script.

```bash
$ sudo ./install-reddit.sh
```

reddit is now available and listening on port 80. If you used the default domain and have name resolution working correctly, try going to http://reddit.local/

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