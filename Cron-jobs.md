Some tasks need to be run on a regular basis for reddit to function
properly. These jobs are described by the `reddit-job-` jobs in [the
upstart directory](https://github.com/reddit/reddit/tree/master/upstart).
Following are explanations of the various periodic tasks.

## update_sr_names

    0    3 * * * root /sbin/start --quiet reddit-job-update_sr_names

Builds an index of subreddit names for use in the subreddit selection
box on the submission page.

## update_reddits

    30  16 * * * root /sbin/start --quiet reddit-job-update_reddits

Updates the list of subreddits. In the open source code, subreddits are
ordered by number of subscribers. This script must be run to have new
subreddits show up in the `/reddits` listing or the top bar.

## update_promos

    0    * * * * root /sbin/start --quiet reddit-job-update_promos

Processes pending actions for self-serve promotions.

## clean_up_hardcache

    */5  * * * * root /sbin/start --quiet reddit-job-clean_up_hardcache

Cleans expired data out of the hardcache (a table in Postgres).

## email

    *    * * * * root /sbin/start --quiet reddit-job-email

Processes queued emails and sends them. This includes email address
verification and password reset messages.

## broken_things

    */2  * * * * root /sbin/start --quiet reddit-job-broken_things

Scans through recently created things to make sure they have all of
their essential attributes. Sometimes, when an app crashes mid-request,
things will be left in an inconsistent state. This script will find them
and mark them deleted so they don't break other pieces of the code.

## rising

    */2  * * * * root /sbin/start --quiet reddit-job-rising

Calculates the [rising](http://www.reddit.com/new/?sort=rising)
pages. It should be run frequently to keep the rising page
up to date.
