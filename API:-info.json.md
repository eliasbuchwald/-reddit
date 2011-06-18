Use `http://www.reddit.com/api/info.json` to grab information about a URL's submissions on reddit.

**Note:** For the same data in XML, please see [[API: info.xml]].

## GET Data

- `url` - The URL of the page you are requesting information about.

## Example Response

If the `reddit_session` cookie is not present in the request, the API will return `false` for `saved` and `hidden`, and `null` for `likes`.

Otherwise, the API will return something like the following about the currently logged in user:

```javascript
{
    "kind": "Listing",
    "data": {
        "modhash": "a4isjdlbe719c86ee9747ac18ac076e9c9de2e6683d32ad564",
        "children": [{
            "kind": "t3",
            "data": {
                "domain": "blog.reddit.com",
                "media_embed": {},
                "levenshtein": null,
                "subreddit": "blog",
                "selftext_html": null,
                "selftext": "",
                "likes": true,
                "saved": true,
                "id": "i0jf9",
                "clicked": false,
                "author": "hueypriest",
                "media": null,
                "score": 1520,
                "over_18": false,
                "hidden": false,
                "thumbnail": "http://thumbs.reddit.com/t3_i0jf9.png",
                "subreddit_id": "t5_2qh49",
                "downs": 2381,
                "is_self": false,
                "permalink": "/r/blog/comments/i0jf9/reddit_levels_up_with_three_new_programmers/",
                "name": "t3_i0jf9",
                "created": 1308164715.0,
                "url": "http://blog.reddit.com/2011/06/reddit-levels-up-with-three-new.html",
                "title": "reddit Levels Up with Three New Programmers",
                "created_utc": 1308164715.0,
                "num_comments": 533,
                "ups": 3901
            }
        }, {
            "kind": "t3",
            "data": {
                "domain": "blog.reddit.com",
                "media_embed": {},
                "levenshtein": null,
                "subreddit": "ifyoulikeblank",
                "selftext_html": null,
                "selftext": "",
                "likes": false,
                "saved": false,
                "id": "i0o2k",
                "clicked": false,
                "author": "warrrennnnn",
                "media": null,
                "score": 80,
                "over_18": false,
                "hidden": false,
                "thumbnail": "http://thumbs.reddit.com/t3_i0o2k.png",
                "subreddit_id": "t5_2sekf",
                "downs": 5,
                "is_self": false,
                "permalink": "/r/ifyoulikeblank/comments/i0o2k/guess_which_awesome_new_subreddit_was_mentioned/",
                "name": "t3_i0o2k",
                "created": 1308174144.0,
                "url": "http://blog.reddit.com/2011/06/reddit-levels-up-with-three-new.html",
                "title": "Guess which \"awesome new subreddit\" was mentioned in the latest Reddit blogpost? r/ifyoulikeblank, you rock :D",
                "created_utc": 1308174144.0,
                "num_comments": 3,
                "ups": 85
            }
        }],
        "after": null,
        "before": null
    }
}
```

## API Reference: info.json

- ###kind  *(string)*

    The type of thing (see **glossary** on the [[API]] page) this is, which translates to *Listing*.

- ### data *(object)*

    Holds relevant data about the URL

    - ### modhash *(string)*

        The page's modhash (see **glossary** on the [[API]] page) needed for voting and other API calls.

    - ### children *(array)*

        [Unix time](http://en.wikipedia.org/wiki/Unix_time) that the user's account was created in [UTC](http://en.wikipedia.org/wiki/Coordinated_Universal_Time)

        - ### domain *(string)*

            The domain of the page that was submitted, *not* including the protocol.

    - ### after *([null, string])*

        If not `null`, it is the FULLNAME (see **glossary** on the [[API]] page) of the post previous to the first one in `children` if what the API returns is paginated.

    - ###before *([null, string])*

        If not `null`, it is the FULLNAME (see **glossary** on the [[API]] page) of the post after the last one in `children` if what the API returns is paginated.