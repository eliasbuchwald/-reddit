reddit supports [oEmbed](http://www.oembed.com) on individual comments, and likely link posts in the future.

The oEmbed endpoint is:

https://www.reddit.com/oembed?url=https://www.reddit.com/r/Showerthoughts/comments/2safxv/we_should_start_keeping_giraffes_a_secret_from/cno7zic

And provides JSON like:

```json
{
    "provider_url": "https://www.reddit.com/",
    "version": "1.0",
    "title": "h5f6jdh7b9vj0d3fhtnd's comment from discussion \"We should start keeping giraffes a secret from young children. Imagine discovering giraffes exist when you were like 15. \"Woah! Check out that long necked horse!\"\"",
    "provider_name": "reddit",
    "type": "rich",
    "html": "<div class=\"reddit-embed\" data-embed-media=\"www.redditmedia.com\" data-embed-parent=\"false\" data-embed-live=\"false\" data-embed-created=\"2015-04-29T22:33:02.658202+00:00\"><a href=\"http://www.reddit.com/r/Showerthoughts/comments/2safxv/we_should_start_keeping_giraffes_a_secret_from/cno7zic\">Comment</a> from discussion <a href=\"http://www.reddit.com/r/Showerthoughts/comments/2safxv/we_should_start_keeping_giraffes_a_secret_from/\">h5f6jdh7b9vj0d3fhtnd's comment from discussion &quot;We should start keeping giraffes a secret from young children. Imagine discovering giraffes exist when you were like 15. &quot;Woah! Check out that long necked horse!&quot;&quot;</a>.</div><script async src=\"https://www.redditstatic.com/comment-embed.js\"></script>",
    "author_name": "h5f6jdh7b9vj0d3fhtnd"
}
```

When embedded, the embed will look like:

![geraffes](http://i.imgur.com/QaUs3s0.png)

The oEmbed endpoint also accepts two non-standard parameters to help make better embeds:

1. **parent** - boolean - include the parent in the embed.

2. **live** - boolean - allow edits made to this comment to be showed immediately.

More on embeds here: https://www.reddit.com/wiki/embeds

Presently, non-permalinks to comments will 404. When we add support for post embeds, we'll also support them in oEmbed format.

