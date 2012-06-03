To reduce the amount of work done in-request, reddit defers to asynchronous message queues for many tasks. These jobs are described by the `reddit-consumer-` jobs in [the upstart directory](https://github.com/reddit/reddit/tree/master/upstart). Following are explanations of the various queue consumer jobs.

## Queue Processors

### vote_link_q and vote_comment_q

Voting on a link or comment inserts an item on one of these queues. The queue processors receive a message for each vote they need to process and will update scores, karma, and cached listings accordingly.

### scraper_q

When a link is submitted, it is added to the scraper_q for deferred processing. This queue processor works through the submitted links and will scrape the submitted URL for media embed information and thumbnails. 

### commentstree_q

After a comment is created, this queue processor does the work of updating the cached comment tree data structures. 

### newcomments_q

This processor inserts items onto the `/comments` listings. It exists because the lock contention of prepending to the list from every app was too high.

### cloudsearch_q

When new links, comments, or subreddits are created, or when existing ones are edited, the Amazon CloudSearch index needs to be updated for search to work. This queue processor handles batching up and sending updates to Amazon for processing.