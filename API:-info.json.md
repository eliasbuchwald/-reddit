Use `http://www.reddit.com/api/info.json?url=` to grab information about a URL's submissions on reddit.

**Note:** For the same data in XML, please see [[API: info.xml]].

## GET Data

- `url` - The URL of the page you are requesting information about.

## Example Response

If the `reddit_session` cookie is not present in the request, the API will return `false` for `saved` and `hidden`, and `null` for `likes`, but the rest of the information returned will be the same as if the user was logged in.

Otherwise, the API will return something like the following about the submissions with `saved` and `null` customized for the currently logged in user:

```javascript
{
    "kind": "Listing",
    "data": {
        "modhash": "f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0",
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

        An array that holds each post that was submitted for this URL.

        - ### kind *(string)*

            The type of thing (see **glossary** on the [[API]] page) this is, which translates to *Link*.

        - ### data *(object)*

            Holds relevant post data.

            - ### domain *(string)*

                The domain of the page that was submitted, *not* including the protocol.

            - ### meda_embed *(object)*

                **What is this for?**

            - ### levenshtein *(null)*

                This is always null. Apparently, it was an experiment that got removed from everything but `jsontemplates`.

            - ### subreddit *(string)*

                The name of the subreddit this post was submitted to.

            - ### selftext_html *(null)*

                This would hold the HTML-formatted text of the post if this was a self post, but since `info.json` always serves information about link posts, it is always null.

            - ### selftext *(null)*

                This would hold the plaintext of the post if this was a self post, but since `info.json` always serves information about link posts, it is always null.

            - ### likes *([null, boolean])*

                If the `reddit_session` cookie is not present in the request, the API will return null.

                Otherwise, this will indicate how the currently logged in user has voted the story: `true` for an up vote, `false` for a down vote, or `null` for no vote.

            - ### saved *(boolean)*

                If the `reddit_session` cookie is not present in the request, the API will return false.

                Otherwise, this will indicate whether the currently logged in user has saved the story.

            - ### id *(string)*

                The link's id36 (see **glossary** on the [[API]] page). This is also used for short URLs (e.g. <http://redd.it/6nw57>) and toolbar URLs (e.g. <http://www.reddit.com/tb/6nw57/>).

            - ### clicked *(boolean)*

                **Is this really always false?**

            - ### author *(string)*

                The username of the user who submitted the post.

            - ### media *(null)*

                **What is this for?**

            - ### score *(number)*

                The current score (based on up votes minus down votes)  of the post.

            - ### over_18 *(boolean)*

                Says whether the post has been marked NSFW.

            - ### hidden *(boolean)*

                If the `reddit_session` cookie is not present in the request, the API will return false.

                Otherwise, this will indicate whether the currently logged in user has hidden the story.

            - ### thumbnail *([ string, enumerated string [ `"/static/nsfw2.png"`, `""`, `"/static/noimage.png"` ] ])*

                If the `reddit_session` cookie *is* present in the request **and** the post *has* been marked NSFW **and** the user *does have* the "make safe(r) for work" preference checked, the API will return [`"/static/nsfw2.png"`](http://www.reddit.com/static/nsfw2.png), which is 70px square.

                Otherwise, if the `reddit_session` cookie *is not* present in the request **and** the post has been marked NSFW, the API will return `""`.

                Otherwise, if the post has no thumbnail, the API will return [`"/static/noimage.png"`](http://www.reddit.com/static/noimage.png), which is 70px wide by 50px tall.

                Otherwise, the API will return a full URL to a thumbnail that is 70px wide.

            - ### name *(type)*

                Description

            - ### name *(type)*

                Description

    - ### after *([null, string])*

        If not `null`, it is the FULLNAME (see **glossary** on the [[API]] page) of the post previous to the first one in `children` if what the API returns is paginated.

    - ###before *([null, string])*

        If not `null`, it is the FULLNAME (see **glossary** on the [[API]] page) of the post after the last one in `children` if what the API returns is paginated.