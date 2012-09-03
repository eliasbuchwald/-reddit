## Basic Information

### Under what license is reddit released?

The CPAL. See <https://github.com/reddit/reddit/blob/master/LICENSE>

### Is this all of the code?

Our anti-cheating/spam code is not public. In the source itself, anything under the package r2admin falls under this, so it's easy to see where that begins and ends. Our private code is not required to run a successful clone. 

## Installation Troubleshooting

### How can I figure out which part of the system is broken?

```bash
$ sudo svstat /service/*
```

Look for services that have an uptime much smaller than the rest of the services. Once you've found one, check its logs for errors

```bash
$ tail -F /service/broken-service/log/main/current
```

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
    <td>setup.py installed the wrong version of webhelpers. You must downgrade it.

```bash
$ sudo easy_install "webhelpers==0.6.4"
```
</td>
</tr>
<tr>
    <td>(OperationalError) FATAL:  password authentication failed for user "reddit"</td>
    <td>
        The postgres user "reddit" has the wrong password. You should recreate the postgres user "reddit" with password "reddit".

```bash
reddit$ su postgres
postgres$ dropuser reddit
postgres$ createuser -P reddit
```
    </td>
</tr>
</tbody>
</table> 

## Other Questions

### Do I need to use S3 to run reddit?

At the moment, S3 is required for thumbnails. We are willing to accept patches that would allow you to choose where to store them.

### What is the administrator account?

By default, there are no accounts. Create an account and add it to the `admins` line in `example.ini`. If you populate the database using `populatedb` it will generate an account called `reddit` with password `password` which is in the list of admins by default.

### Why can't I log in to my reddit?

Most browsers won't deal with cookies from a domain that doesn't contain dots. For example, `http://localhost` will not work, while `http://reddit.local` or `http://127.0.0.1` will. Make sure you update the .ini file accordingly!

### How do I create a subreddit?

By default, the link to the form for subreddit creation is hidden for accounts newer than 30 days. This was done because a large number of new users were confused about what creating a reddit meant and would use the form to try and post links. The form can be accessed at any time by going to `/reddits/create`. It's also possible to configure the minimum age an account must have to be able to see the link by tweaking the [`min_membership_create_community`](https://github.com/reddit/reddit/blob/master/r2/example.ini#L326) property. Setting this value to 0 will allow all users to see the link.

### How do I make subreddits show up in the top bar?

`update_reddits` needs to be run. See [[Cron Jobs]] for more information on what this does. To run it manually:

    sudo start reddit-job-update_reddits 

### What if the top bar isn't showing up at all?

Check that `domain` in the ini file matches the domain that you're accessing your reddit clone with.

### Why doesn't the "My Reddits" dropdown show up?

The dropdown is only rendered if the current user is subscribed to more than `sr_dropdown_threshold` subreddits (see `example.ini`.) The default threshold is 15 subreddits.

### Why doesn't search work? 

reddit used to use [Solr](http://lucene.apache.org/solr/) for its search needs, but [switched](http://blog.reddit.com/2010/07/new-search.html) to using [IndexTank](http://indextank.com/) in July 2010. They currently use [Amazon CloudSearch](http://aws.amazon.com/cloudsearch/). Kemitche has provided [instructions](http://www.reddit.com/r/redditdev/comments/wqx7o/does_reddit_using_amazon_cloud_search/c5fut4i) on how to set up a clone to use your own instance.