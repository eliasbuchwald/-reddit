## Rules ##
We're happy to have API clients, crawlers, scrapers, and Greasemonkey scripts, but they have to obey some rules:


* **Make no more than one request every two seconds.** There's some allowance for bursts of requests, but keep it sane. In general, keep it to no more than 30 requests in a minute.
* Change your client's User-Agent string to something unique and descriptive, preferably referencing your reddit username. 
    * Example: `User-Agent: super happy flair bot by /u/spladug`
    * Many default User-Agents (like "Python/urllib" or "Java") are drastically limited to encourage unique and descriptive user-agent strings.
    * **NEVER lie about your user-agent.** This includes spoofing popular browsers and spoofing other bots. We will ban liars with extreme prejudice.
* Most pages are cached for 30 seconds, so you won't get fresh data if you request the same page that often. So don't hit the same page more than once per 30 seconds
* Requests for multiple resources at a time are always better than requests for single-resources in a loop. Talk to us on the mailing list or in #reddit-dev if we don't have a batch API for what you're trying to do.
* If you need help testing your API client or assessing its impact on us, please ask on [the mailing list](http://groups.google.com/group/reddit-dev). If your API client could hurt reddit, and we catch it before you do, we'll have to ban it. It's nothing personal, but we have to keep the site up and 50% of the time when something goes wrong it's a badly written robot.

## See Also ##
* [reddit API Documentation](http://www.reddit.com/dev/api)

## API Console ##

You can try out the API with Apigee's API console: https://apigee.com/console/reddit

## Glossary ##

### Thing ###

An object, such as a Link, Comment, or Account.  See [[thing]] for more details.

### Modhash ###

A key which reddit requires for API functions modifying state to protect against XSRF attacks.  A modhash will be returned on login and whenever a `Listing` thing is sent.  See [[thing]] for more details.

### Fullname ###

A [base-36](http://en.wikipedia.org/wiki/Base_36) id of the form `t[0-9]+_[a-z0-9]+` (e.g. [t3_6nw57](http://www.reddit.com/r/programming/comments/6nw57/)) that reddit associates with every Thing. In the example, the type prefix `t3_` specifies that the fullname is for a Link, and the id `6nw57` specifies the Link's id36. The type IDs may vary among different reddit clones, but here are the possible values for reddit.com.

| **kind id** | **name** |
|:---|:---------|
|  1 | comment
|  2 | account
|  3 | link
|  4 | message
|  5 | subreddit

## API References ##

Below is an incomplete list of operations you can perform using the Reddit API. For reference examples of using such API operations, take a look at some of the [[third party clients | API: Third Party Clients]].

- [[API: me.json]]
- [[API: mine.json]]
- [[API: info.json]]
- [[API: vote]]
- [[API: comment]]
- [[API: login]]
- [[API: submit]]
- [[API: save]]
- [[API: unsave]]
- [[API: hide]]
- [[API: unhide]]
- [[API: flairlist]]
- [[API: flair]]
- [[API: flaircsv]]

## Fetching Information ##

To download raw data from reddit, simply find the page that contains the desired data, and append an extension to the URL:

* Adding `.json` to the URL will return raw data in JSON form. Example: <http://www.reddit.com/r/programming/comments/6nw57/.json>
* Adding `.rss` or `.xml` to the URL will return the data formatted as an RSS feed. Example: <http://www.reddit.com/.rss>

In many applications, it is useful to query reddit Links by URL or Fullname. This is possible using the following queries, followed by an extension of your choice:

#### Link by Fullname ####

`http://www.reddit.com/by_id/FULLNAME.EXTENSION`

Example: http://www.reddit.com/by_id/t3_6nw57.json

#### Link by Fullname (With Comments) ####

`http://www.reddit.com/comments/ID36.EXTENSION`

Example: http://www.reddit.com/comments/6nw57.json

#### Getting User "About" Page (Includes Karma Totals) ####

`http://www.reddit.com/user/USERNAME/about.json`

TODO: Clarify licensing of reddit.com information

### Fetching more ###

If you're fetching comments from a thread with more comments than the API will return in a single response, the last comment will look like this:

```javascript
{'data': {'id': 'abc1010', 'name': 't1_abc1010'}, 'kind': 'more'}
```

To get these comments, you can fetch the url `http://reddit.com/comments/ID/.../abc1010.json`, where ID is the base-36 ID of the story and `...` can be any non-breaking string (a-z, A-Z, _).  Example: `http://www.reddit.com/comments/quxda/_/c40oyqc` loads the comment who's id is `c40oyqc` from the story who's ID is`quxda`.  The filler string we used in place of `...` is an underscore character, but i can be any alpha-numeric that includes underscores.

### Fix Listing Size ###

You can specify how many `thing`s you want listed by adding the url query `limit`.  For example, a GET to `http://www.reddit.com/.json?limit=5` will return a listing of 5 `link`s instead of the default 25 or whatever the user has set as the default.  Note that many listings have a built in max for the limit parameter.  For example, you can only fetch 100 links and 500 comments (1500 for RedditGold members).

This also works with any extension as well as for regular browsing.  Eg. `http://www.reddit.com/?limit=5`.

## Logging In ##

To do a simple login, send an HTTP POST to `http://www.reddit.com/api/login` Must include two POST parameters - `user` and `passwd`

Login now supports SSL, send an HTTP POST to `https://ssl.reddit.com/api/login`

This will return a [SetCookie](http://en.wikipedia.org/wiki/HTTP_cookie#Setting_a_cookie) line. It is best to manage this cookie with some automated tool or framework

Here are some examples made using the [curl](http://curl.haxx.se/) command from a unix/linux terminal

### Correct Login ###

Making the request to <http://www.reddit.com/api/login>. We set two POST parameters: `user=some_user` and `passwd=correct_passwd`. curl automatically converts returned HTTP headers into cookies, so the [SetCookie](http://en.wikipedia.org/wiki/HTTP_cookie#Setting_a_cookie) is converted into a cookie. The `-c` option requests that curl print all cookies associated with this request into Cookie.txt

```
$> curl -d user=some_user -d passwd=correct_passwd -c Cookie.txt http://www.reddit.com/api/login
```

The server returns some JSON which curl prints to stdout for us.

```javascript
{"jquery": [[0, 1, "call", ["body"]], [1, 2, "attr", "find"], [2, 3, "call", [".status"]], [3, 4, "attr", "hide"], [4, 5, "call", []], [5, 6, "attr", "html"], [6, 7, "call", [""]], [7, 8, "attr", "end"], [8, 9, "call", []], [1, 10, "attr", "redirect"], [10, 11, "call", ["/"]]]}
```

Now let's look at what the returned cookie contains! Hopefully it is a session ID that we can now pass to other parts of the reddit API...

```
$> cat Cookie.txt
```

```
# Netscape HTTP Cookie File
# http://curl.haxx.se/rfc/cookie_spec.html
# This file was generated by libcurl! Edit at your own risk.

.reddit.com	TRUE	/	FALSE	0	reddit_session	4029916%2C2010-04-30T22%3A51%3A52%2C1243925043100000000000000000000000000000
```

As hoped, we see a reddit_session (Note that I changed the ending chars to zero's for this example).

Breaking down what you are looking at here - 

| **attribute**   | **value**              |
|:----------------|:-----------------------|
| `domain`        | `.reddit.com`          |
| `tailmatch`     | `TRUE`                 |
| `path`          | `/`                    |
| `secure`        | `FALSE`                |
| `expires`       | `0`                    |
| `name`          | `reddit_session`       |
| `value`         | long-alphanumeric :) |

If you are not sure what all this means, then either [read up](http://en.wikipedia.org/wiki/HTTP_cookie), or use a framework to manage the cookie.

### Incorrect Login ###

Similar to correct example above, so I am just going to show you the results

```
$> curl -d user=some_user -d passwd=wrong_password -c Cookie.txt http://www.reddit.com/api/login
```

```javascript
{"jquery": [[0, 1, "call", ["body"]], [1, 2, "attr", "find"], [2, 3, "call", [".status"]], [3, 4, "attr", "hide"], [4, 5, "call", []], [5, 6, "attr", "html"], [6, 7, "call", [""]], [7, 8, "attr", "end"], [8, 9, "call", []], [1, 10, "attr", "find"], [10, 11, "call", [".error.WRONG_PASSWORD.field-passwd"]], [11, 12, "attr", "show"], [12, 13, "call", []], [13, 14, "attr", "html"], [14, 15, "call", ["invalid password"]], [15, 16, "attr", "end"], [16, 17, "call", []]]}
```

```
$> cat Cookie
```

```
# Netscape HTTP Cookie File
# http://curl.haxx.se/rfc/cookie_spec.html
# This file was generated by libcurl! Edit at your own risk.
 
.reddit.com	TRUE	/	FALSE	2145916799	reddit_first	%7B%22firsttime%22%3A%20%22first%22%7D
$> 
```

As you can see, it lets you know the password was wrong, and the reddit_session is not set (reddit_first is, however).

Note that if you enter a username that does not exist, it still returns the "invalid password" message (in my experience). This behavior might be subject to change, so check it if you need it.

Now that you have successfully logged-in, pages served from reddit will contain a "user modhash". This hash is required in making other calls to the api. You can find it by looking for a variable named "uh".

TODO: Clean this section up and make more user friendly

## Submitting New Stories ##

See also [[API:-submit]].

For submitting to work, the `reddit_session` cookie needs to be present in the request.

It is possible to submit without a cookie, but that requires supplying the answer to the captcha.

Post the following to `http://www.reddit.com/api/submit`:

```
uh=f0f0f0f0&kind=link&url=yourlink.com&sr=funny&title=omg-look-at-this&id%23newlink&r=funny&renderstyle=html
```

| **field**   | **value**                                      |
|:------------|:-----------------------------------------------|
| `uh`        | the user modhash                               |
| `kind`      | either "link" or "self"                        |
| `sr`        | the subreddit                                  |
| `title`     | the text to appear as a link to your new story |
| `r`         | appears to be the subreddit again **(?)**      |

Setting "kind" to "self" means that the "url" field becomes a "text" field.

The JSON reply will be a somewhat larger one, and it can potentially contain an error message.

Here's one error message:

> You haven't verified your email address; until you do, your submitting privileges will be severely limited. Please try again in an hour or verify your email address. If you'd like an exemption from this rule, please write to the moderators of this reddit.

If the submission worked, the JSON reply will contain the link to the newly created story.

## Sharing Stories ##

The idea here is that we want to have reddit email a story to a friend on our behalf.

For sharing to work, the `reddit_session` cookie needs to be present in the request.

Post the following to `http://www.reddit.com/api/share`:

```
parent=t1_abc1010&share_from=Foo&replyto=foo@bar.org&share_to=afriend@example.com&message=i+share+this&id%23sharelink_t1_abc1010&uh=f0f0f0f0f0&renderstyle=html
```

| **field**    | **value**                                                   |
|:-------------|:------------------------------------------------------------|
| `parent`     | the thing you want to share                                 |
| `share_from` | the name of the person who is sending the message           |
| `replyto`    | the email address of the person who is sending the message  |
| `share_to`   | the email address of the recipient of the message           |
| `message`    | the text that precedes the link to the story in the message |
| `uh`         | the user modhash                                            |

The JSON reply is another short one:

```javascript
{}
```

## Changes to the API ##

Changes to this API can happen without warning. Use the Firefox add-on [LiveHeaders](https://addons.mozilla.org/en-us/firefox/addon/live-http-headers/) or the Webkit inspector's Network section to see what fields are being posted this week.

The JSON replies can also change without warning.

## Downtime ##

As of this writing, downtime will simply return the down page. Not sure if this comes with a non-200 HTTP response.

## See Also ##

* If you can read [Python](http://www.python.org/), the [API source](https://github.com/reddit/reddit/blob/master/r2/r2/controllers/api.py) is the definitive reference for API calls.

* [More API info on the talklittle project](https://github.com/talklittle/reddit-is-fun/wiki/API---all-functions)