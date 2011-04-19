The reddit install script is a bash script designed for use on Ubuntu Linux. It has been tested with [Ubuntu 10.04 LTS (Lucid Lynx)](http://releases.ubuntu.com/lucid/). The script will install the majority of the reddit stack from scratch.

## Usage
```bash
curl https://gist.github.com/raw/922144/install-reddit.sh | sudo sh
```

-- or --

1. Get the [latest version of the install script](https://gist.github.com/922144).
2. Run the script as root.

## Test data

To populate the database with test data, including a variety of Accounts, Subreddits, Links and Comments do the following as user `reddit`.

```bash
$ cd ~/reddit/r2
$ paster shell run.ini
```
```python
>>> from r2.models import populatedb
>>> populatedb.populate()
```

This will take a while and spew a lot of trace to the console, but the result is a reddit with plenty of test data.

## FAQ

### What is the administrator account?

By default, there are no accounts. Create an account and add it to the `admins` line in `example.ini`. If you populate the database using `populatedb` it will generate an account called `reddit` with password `password` which is in the list of admins by default.

### Why can't I log in to my reddit?

Most browsers won't deal with cookies from a domain that doesn't contain dots. For example, `http://localhost` will not work, while `http://reddit.local` or `http://127.0.0.1` will. Make sure you update the .ini file accordingly!

### How do I make subreddits show up in the top bar?

`update_reddits.sh` needs to be run. See [[Cron Jobs]] for more information on what this does.

### What if the top bar isn't showing up at all?

Check that `domain` in the ini file matches the domain that you're accessing your reddit clone with.

### Why doesn't search work? 

reddit used to use [Solr](http://lucene.apache.org/solr/) for its search needs, but [switched](http://blog.reddit.com/2010/07/new-search.html) to using [IndexTank](http://indextank.com/) in July 2010. The code for searching with Solr still exists in the repository, but is inactive. A third party running a reddit clone could either create a patch that reactivates Solr, or get an IndexTank account.

### Why doesn't subreddit search or the "related" tab work?

Solr is still used for subreddit search and the "related" tab. This is transitional until we convert completely to IndexTank. The install script does not currently install or configure Solr and so neither of these features will work until that is done. 