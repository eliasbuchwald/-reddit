Use `http://www.reddit.com/reddits/mine.json` to grab information about the subreddits the currently logged in user subscribes to.

**Note:** For the same data in XML, please see [[API: mine.xml]].

## Example Response

If the `reddit_session` cookie is not present in the request, the page will serve a `302 Moved Temporarily` status code, and redirect to `http://www.reddit.com/reddits/login.json?dest=%2Freddits%2Fmine.json`. That page will return the following:

```javascript
""
```

Otherwise, the API will return something like the following about the subreddits the currently logged in user subscribes to.

```javascript
{
    "kind": "Listing",
    "data": {
        "modhash": "f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0",
        "children": [
            {
                "kind": "t5",
                "data": {
                    "display_name": "Ubuntu",
                    "name": "t5_2qh62",
                    "title": "Ubuntu: Linux for Human Beings",
                    "url": "/r/Ubuntu/",
                    "created": 1201269320.0,
                    "created_utc": 1201269320.0,
                    "over18": false,
                    "subscribers": 16259,
                    "id": "2qh62",
                    "description": "**News for the Ubuntu Linux distribution**\n\nNote that this subreddit is intended primarily for news and information, **not tech support**. If you are in need of support, try one of the following sites:\n\n* [Official Ubuntu Documentation](https://help.ubuntu.com/)\n\n* [Official Ubuntu Forums](http://ubuntuforums.org/)\n\n* [Ubuntu Manual](http://ubuntu-manual.org/)\n\n* [/r/LinuxQuestions](http://www.reddit.com/r/linuxquestions)\n\n* [Ask Ubuntu](http://askubuntu.com/)\n\n* [#ubuntu on irc.freenode.net](http://webchat.freenode.net/?channels=ubuntu)\n\n\nAdditionally, feel free to message us if your (non-spam!) link/post is accidentally trapped in our **spam filter**, and we'll sort it out."
                }
            },
            {
                "kind": "t5",
                "data": {
                    "display_name": "UniversityofReddit",
                    "name": "t5_2rqj9",
                    "title": "University of Reddit",
                    "url": "/r/UniversityofReddit/",
                    "created": 1273206076.0,
                    "created_utc": 1273206076.0,
                    "over18": false,
                    "subscribers": 12092,
                    "id": "2rqj9",
                    "description": "##**When we say you can teach anything, we mean anything!**\n\n* [Read our FAQ](http://universityofreddit.com/help)\n\n* [View course offerings](http://universityofreddit.com/)\n\n* [Create a class](http://universityofreddit.com/register)\n\n* [Follow us on Twitter](http://twitter.com/uofreddit)\n\n* [Like us on Facebook](http://www.facebook.com/pages/University-of-Reddit/141410715882099)\n\n* Join us on IRC - irc.freenode.org #universityofreddit\n\nCheck out some of our friends at [/r/DepthHub](http://reddit.com/r/DepthHub), [/r/MethodHub](http://reddit.com/r/MethodHub), [/r/LanguageLearning](http://reddit.com/r/LanguageLearning), and [/r/food2](http://reddit.com/r/food2)."
                }
            },
            {
                "kind": "t5",
                "data": {
                    "display_name": "chrome",
                    "name": "t5_2qlz9",
                    "title": "Chrome",
                    "url": "/r/chrome/",
                    "created": 1220310943.0,
                    "created_utc": 1220310943.0,
                    "over18": false,
                    "subscribers": 4452,
                    "id": "2qlz9",
                    "description": "All about developments relating to Google's web browser. Post links, ask questions &amp; discuss Chrome-related subjects.\n\nDo not post to blogs that are simply re-hosting information ripped off from another site. Please post directly to original source where possible.\n\n[**Essential Chrome Extensions**](http://goo.gl/qurcI)\n\nFor other Google related posts, try [**/r/Google**](http://goo.gl/f3liW), [**/r/Chromium**](http://goo.gl/vFp9b) and [**/r/Android**](http://goo.gl/qdCkG)\n\n####Questions? Problems? Submission not showing up? Spot a spammer or a troll? Message the mods and we will look into it ASAP."
                }
            },
            {
                "kind": "t5",
                "data": {
                    "display_name": "redditdev",
                    "name": "t5_2qizd",
                    "title": "Reddit Development",
                    "url": "/r/redditdev/",
                    "created": 1213802177.0,
                    "created_utc": 1213802177.0,
                    "over18": false,
                    "subscribers": 2152,
                    "id": "2qizd",
                    "description": "A reddit for discussion of reddit API clients and the reddit source code.\n\n* [Get the code on github!](http://github.com/reddit)\n* [Join the mailing list](http://groups.google.com/group/reddit-dev)\n* [Harass us on IRC: `#reddit-dev` on irc.freenode.net](http://webchat.freenode.net/?channels=reddit-dev) (you can PM `spladug` if we're not in the channel: we're usually at least signed on)\n\n\nPlease please please confine discussion to reddit code instead of using this as a soapbox to talk to the admins. In particular, use [/r/ideasfortheadmins](/r/ideasfortheadmins) for feature ideas and [/r/bugs](/r/bugs) for bugs unless you're planning to write a patch. If you have general reddit questions, try [/r/help](/r/help)."
                }
            }
        ],
        "after": null,
        "before": null
    }
}
```

## API Reference: mine.json

- ###kind  *(string)*

    The type of thing (see **glossary** on the [[API]] page) this is, which translates to *Listing*.

- ### data *(object)*

    Holds relevant subreddit data.

    - ### modhash *(string)*

        The user's modhash (see **glossary** on the [[API]] page)

    - ### children *(array)*

        An array that holds an object for each subreddit that the currently logged in user is subscribed to, if any.

        - ### kind *(string)*

            The type of thing (see **glossary** on the [[API]] page) this is, which translates to *Subreddit*.

        - ### data *(object)*

            Holds relevant subreddit data

            - ### display_name *(string)*

                The name of the subreddit as found in the subreddit's URL, header, and sidebar.

            - ### name *(string)*

                The FULLNAME (see **glossary** on the [[API]] page) of the subreddit.

            - ### title *(string)*

                The title of the subreddit as found in the subreddit's `<title>` tag.

            - ### url *(string)*

                The [relative URL](http://en.wikipedia.org/wiki/Uniform_Resource_Locator#Absolute_vs_relative_URLs) to the subreddit.

            - ### created *(number)*

                This is the [unix time](http://en.wikipedia.org/wiki/Unix_time) that the subreddit was created in **[the currently logged in user's time zone?](https://github.com/reddit/reddit/wiki/API%3A-mine.json/_edit)**.

            - ### created_utc *(number)*

                This is the [unix time](http://en.wikipedia.org/wiki/Unix_time) that the subreddit was created in [UTC](http://en.wikipedia.org/wiki/Coordinated_Universal_Time).

            - ### over18 *(boolean)*

                Indicates whether the subreddit has been marked as NSFW.

            - ### subscribers *(number)*

                The number of subscribers the subreddit currently has.

            - ### id *(string)*

                The subreddit's id36 (see **glossary** on the [[API]] page). This is only used internally**[, right?](https://github.com/reddit/reddit/wiki/API%3A-mine.json/_edit)**.

            - ### description *(string)*

                The description of the subreddit, as written by its moderators, formatted in [reddit flavored markdown](http://www.reddit.com/help/commenting). Newlines are escaped as `\n`.

    - ### after *([null, string])*

        If not `null`, it is the FULLNAME (see **glossary** on the [[API]] page) of the subreddit previous to the first one in `children` if what the API returns is paginated. However, `mine.json` does not seem to paginate things, so the API will always return `null`.

    - ###before *([null, string])*

        If not `null`, it is the FULLNAME (see **glossary** on the [[API]] page) of the subreddit after the last one in `children` if what the API returns is paginated. However, `mine.json` does not seem to paginate things, so the API will always return `null`.