## Basic Information

### Under what license is reddit released?

The CPAL. See <https://github.com/reddit/reddit/blob/master/LICENSE>

### Is this all of the code?

Our anti-cheating/spam code is not public. In the source itself, anything under the package r2admin falls under this, so it's easy to see where that begins and ends. Our private code is not required to run a successful clone. 

## Installation Troubleshooting

### I'm seeing an error. What should I do?

<table>
<thead>
<tr><th>Error</th><th>Resolution</th></tr>
</thead>
<tbody>
<tr>
    <td>ImportError: No module named wrapped</td>
    <td>
         You need to compile the Cython modules.

```bash
$ cd ~/reddit/r2
$ make
```
    </td>
</tr>
<tr>
    <td>ImportError: No module named rails.asset_tag</td>
    <td>setup.py installed the wrong versions of some dependencies. You must downgrade them.

```bash
$ sudo easy_install "Paste==1.7.2-reddit-0.2"
$ sudo easy_install "webhelpers==0.6.4"
```
</td>
</tr>
</tbody>
</table> 

### Do I need to use S3 to run reddit?

At the moment, S3 is required for thumbnails. We are willing to accept patches that would allow you to choose where to store them.

### What is the administrator account?

By default, there are no accounts. Create an account and add it to the `admins` line in `example.ini`. If you populate the database using `populatedb` it will generate an account called `reddit` with password `password` which is in the list of admins by default.

### Why can't I log in to my reddit?

Most browsers won't deal with cookies from a domain that doesn't contain dots. For example, `http://localhost` will not work, while `http://reddit.local` or `http://127.0.0.1` will. Make sure you update the .ini file accordingly!

### How do I make subreddits show up in the top bar?

`update_reddits.sh` needs to be run. See [[Cron Jobs]] for more information on what this does.

### What if the top bar isn't showing up at all?

Check that `domain` in the ini file matches the domain that you're accessing your reddit clone with.

### Why doesn't the "My Reddits" dropdown show up?

The dropdown is only rendered if the current user is subscribed to more than `sr_dropdown_threshold` subreddits (see `example.ini`. This defaults to 15.

### Why doesn't search work? 

reddit used to use [Solr](http://lucene.apache.org/solr/) for its search needs, but [switched](http://blog.reddit.com/2010/07/new-search.html) to using [IndexTank](http://indextank.com/) in July 2010. The code for searching with Solr still exists in the repository, but is inactive. A third party running a reddit clone could either create a patch that reactivates Solr, or get an IndexTank account.

If you don't have `INDEXTANK_API_URL` set to a valid URL, searching will fail with the following error.

```python
<type 'exceptions.TypeError'>: unsupported operand type(s) for +: 'NoneType' and 'str'
```

### Why doesn't subreddit search or the "related" tab work?

Solr is still used for subreddit search and the "related" tab. This is transitional until we convert completely to IndexTank. The install script does not currently install or configure Solr and so neither of these features will work until that is done. 