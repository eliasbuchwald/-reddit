The reddit install script is a bash script designed for use on Ubuntu Linux. It is supported on Ubuntu 14.04 (Trusty Tahr). The script will install the majority of the reddit stack from scratch. ***Don't run this on a production system or anything with valuable data. We recommend using a dedicated environment (such as a VM) with at least 4GB of RAM***

## Usage
1. Get the [latest version of the install script](https://github.com/reddit/reddit/blob/master/install-reddit.sh).
2. Run the script. (It doesn't need to be invoked as root, but it'll ask for `sudo` as it runs)

```bash
$ wget https://raw.github.com/reddit/reddit/master/install-reddit.sh
$ chmod +x install-reddit.sh
$ ./install-reddit.sh
```

_Read the instructions that this script prints_, making sure that the pre-defined environmental variables are set properly.  

This script will download the installer and config from [the reddit github repository](https://github.com/reddit/reddit/tree/master/install).  The downloaded config (`install/install.cfg`) and the main installer script (`install/reddit.sh`) that come with running that command have got some helpful information that can prevent common issues. 

Reddit is now available and listening on port 80.  Resolving to the appropriate domain name is beyond the scope of this document, but the easiest thing is probably editing `/etc/hosts` on the host machine.  If you used the default domain and have name resolution working correctly, try going to http://reddit.local/

## Test data

To populate the database with test data, including a variety of Accounts, Subreddits, Links and Comments do the following.

```bash
$ cd $REDDIT_HOME/src/reddit
$ reddit-run scripts/inject_test_data.py -c 'inject_test_data()'
```

This will take a while and spew a lot of trace to the console, but the result is a reddit with plenty of test data.

## Troubleshooting

See [[FAQ]] for some common issues.