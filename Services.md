There are several runscripts in the [/srv/](https://github.com/reddit/reddit/tree/master/srv) directory of the repository. This page aims to document what they do. 

## Queue Processors

### vote_link_q and vote_comment_q

Voting on a link or comment inserts an item on one of these queues. The queue processors read the votes and process them to detect cheating then update the scores accordingly.

### scraper_q

When a link is submitted, it is added to the scraper_q for deferred processing. This queue processor works through the submitted links and will scrape the submitted URL for media embed information and thumbnails. 

### commentstree_q

After a comment is created, this queue processor does the work of updating the comment tree data structures. 

### comments_q

This processor inserts items onto the `/comments` listing. It exists because the lock contention of prepending to the list from every app was too high.