There are several runscripts in the [/srv/](https://github.com/reddit/reddit/tree/master/srv) directory of the repository. 

## Daemons

### cassandra and memcached

These runscripts are provided in case your platform does not provide runscripts for Cassandra and memcached. If they are provided, you can elect not to use these.

### haproxy and reddit-app0{1,2}

In an effort to better mimic the reddit.com production environment, these runscripts create two separate app processes which are load balanced by haproxy. haproxy will bind to port 80, while the two apps will be bound to 8001 and 8002 respectively. 

### servicemonitor

The service monitor system is designed to maintain information on the status and load of various servers. By default, these runscripts monitor the reddit apps and cassandra.  If a master/slave replication scheme is used for postgres, the service monitor can watch the loads on the slaves and help the apps balance their reads across the slaves.

## Queue Processors

### vote_link_q and vote_comment_q

Voting on a link or comment inserts an item on one of these queues. The queue processors read the votes and process them to detect cheating then update the scores accordingly.

### scraper_q

When a link is submitted, it is added to the scraper_q for deferred processing. This queue processor works through the submitted links and will scrape the submitted URL for media embed information and thumbnails. 

### commentstree_q

After a comment is created, this queue processor does the work of updating the comment tree data structures. 

### comments_q

This processor inserts items onto the `/comments` listing. It exists because the lock contention of prepending to the list from every app was too high.