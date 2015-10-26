The reddit install script is a bash script designed for use on Ubuntu Linux. It is designed for 14.04 (Trusty Tahr). The script will install the majority of the reddit stack from scratch. ***Don't run this on a production system or anything with valuable data. We recommend using a dedicated environment (such as a VM) with at least 4GB of RAM***

## Usage
1. Get the [latest version of the install script](https://github.com/reddit/reddit/blob/master/install-reddit.sh).
2. Run the script as root.

```bash
$ wget https://raw.github.com/reddit/reddit/master/install-reddit.sh
$ chmod +x install-reddit.sh
$ less install-reddit.sh
```

Seriously, stop here and *read* the script. It's got some helpful information that can prevent common issues. Resolving to the appropriate domain name is beyond the scope of this document, but the easiest thing is probably editing `/etc/hosts` on the host machine. Then just run the script.

```bash
$ sudo ./install-reddit.sh
```

reddit is now available and listening on port 80. If you used the default domain and have name resolution working correctly, try going to http://reddit.local/

## Test data

To populate the database with test data, including a variety of Accounts, Subreddits, Links and Comments do the following.

```bash
$ cd $REDDIT_HOME/src/reddit
$ reddit-run scripts/inject_test_data.py -c 'inject_test_data()'
```

This will take a while and spew a lot of trace to the console, but the result is a reddit with plenty of test data.

## Troubleshooting

See [[FAQ]] for some common issues.