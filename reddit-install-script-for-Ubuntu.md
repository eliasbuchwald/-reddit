The reddit install script is a bash script designed for use on Ubuntu Linux. It has been tested with [Ubuntu 10.04 LTS (Lucid Lynx)](http://releases.ubuntu.com/lucid/). The script will install the majority of the reddit stack from scratch.

## Usage
```bash
$ curl https://raw.github.com/gist/922144/install-reddit.sh | sudo sh
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

## Troubleshooting

See [[FAQ]] for some common issues.