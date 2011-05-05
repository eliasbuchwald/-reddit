This is a description of the periodic tasks a reddit clone needs to stay healthy. See the [[Install guide]] for details on setting these up.

## ~/reddit/scripts/rising.sh

Calculates the [rising](http://www.reddit.com/new/?sort=rising) pages. Whenever a link is rendered by an app, that link's ID is incremented in memcache. `rising.sh` uses this information to determine which links have been upvoted the most relative to the number of times they've been seen. It should be run frequently to keep the rising page up to date.

## ~/reddit/scripts/send_mail.sh

Processes queued emails and sends them. This includes email address verification and password reset messages.

## ~/reddit/scripts/broken_things.sh

Scans through recently created things to make sure they have all of their essential attributes. Sometimes, when an app crashes mid-request, things will be left in an inconsistent state. This script will find them and mark them deleted so they don't break other pieces of the code.

## ~/reddit/scripts/update_promos.sh

Processes pending actions for self-serve promotions. 

## ~/reddit/scripts/update_reddits.sh

Updates the list of subreddits. In the open source code, subreddits are ordered by number of subscribers. This script must be run to have new subreddits show up in the `/reddits` listing or the top bar. 