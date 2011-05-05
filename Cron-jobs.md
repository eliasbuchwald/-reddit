## ~/reddit/scripts/rising.sh

The [rising](http://www.reddit.com/new/?sort=rising) pages are calculated by this script. Whenever a link is rendered by an app, that link's ID is incremented in memcache. `rising.sh` uses this information to determine which links have been upvoted the most relative to the number of times they've been seen. It should be run frequently to keep the rising page up to date.

## ~/reddit/scripts/send_mail.sh

This script will send emails from the queue, including email verification messages.

## ~/reddit/scripts/broken_things.sh

Scans through recently created `Thing`s to make sure they have all of their essential attributes. Sometimes, when an app crashes mid-request, things will be left in an inconsistent state. This script will find them and mark them deleted so they don't break other pieces of the code.

## ~/reddit/scripts/update_promos.sh
## ~/reddit/scripts/update_reddits.sh