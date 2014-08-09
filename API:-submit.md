Use `http://www.reddit.com/api/submit` to submit a post, either a link post or a text post.

# Link Posts

## POST Data

| **field** | **sample value** | **explanation** |
|:----------|:-----------------|:----------------|
| `title`   | `Look at this!`  | The text that the user submits as the link's title. |
| `url`     | `http://github.com/reddit/reddit` | The URL of the page being submitted.  |
| `sr`      | `programming`    | The subreddit the post is being submitted to. |
| `kind`    | `link`           | This must always be `link` for link posts. For text posts, see the next section. |
| `uh`      | `f0f0f0f0`...    | The currently logged in user's modhash (see **glossary** on the [[API]] page). |

## Example Response

If the `reddit_session` cookie **and** `uh` POST Data are not present in the request, the API will return something like the following:

```javascript
{
    "jquery": [
        [0, 1, "attr", "refresh"],
        [1, 2, "call", []],
        [0, 3, "attr", "find"],
        [3, 4, "call", [".error.USER_REQUIRED"]],
        [4, 5, "attr", "show"],
        [5, 6, "call", []],
        [6, 7, "attr", "text"],
        [7, 8, "call", ["please login to do that"]],
        [8, 9, "attr", "end"],
        [9, 10, "call", []]
    ]
}
```

Otherwise, if the user is being rate-limited, the API will return something like the following:

```javascript
{
	"jquery":[
		[0, 1, "call", ["#newlink"]],
		[1, 2, "attr", "find"],
		[2, 3, "call", [".status"]],
		[3, 4, "attr", "hide"],
		[4, 5, "call", []],
		[5, 6, "attr", "html"],
		[6, 7, "call", [""]],
		[7, 8, "attr", "end"],
		[8, 9, "call", []],
		[1, 10, "attr", "captcha"],
		[10, 11, "call", ["f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0"]],
		[1, 12, "attr", "find"],
		[12, 13, "call", ["*[name=url]"]],
		[13, 14, "attr", "attr"],
		[14, 15, "call", ["value", "http://www.example.com/"]],
		[15, 16, "attr", "end"],
		[16, 17, "call",[]],
		[1, 18, "attr", "find"],
		[18, 19, "call", [".error.RATELIMIT.field-ratelimit"]],
		[19, 20, "attr", "show"],
		[20, 21, "call", []],
		[21, 22, "attr", "text"],
		[22, 23, "call", ["you are doing that too much. try again in 9 minutes."]],
		[23, 24, "attr", "end"],
		[24, 25, "call", []]
	]
}
```

Otherwise, the API will return something like the following:

```javascript
{
	"jquery": [
		[0, 1, "call", ["#newlink"]],
		[1, 2, "attr", "find"],
		[2, 3, "call", [".status"]],
		[3, 4, "attr", "hide"],
		[4, 5, "call", []],
		[5, 6, "attr", "html"],
		[6, 7, "call", [""]],
		[7, 8, "attr", "end"],
		[8, 9, "call", []],
		[1, 10, "attr", "captcha"],
		[10, 11, "call", ["f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0"]],
		[1, 12, "attr", "find"],
		[12, 13, "call", ["*[name=url]"]],
		[13, 14, "attr", "attr"],
		[14, 15, "call", ["value", "HTTP://URL.URL"]],
		[15, 16, "attr", "end"],
		[16, 17, "call", []],
		[1, 18, "attr", "redirect"],
		[18, 19, "call", ["http://www.reddit.com/r/test/comments/it3f3/test/"]]
	]
}
```

## API Reference: submit (Link Posts)

At `jquery[14][3][1]`, there is a string that represents the link that was just submitted.

At `jquery[18][3][0]`, there is a string that represents the newly created posts's comments page.

# Text Posts


## POST Data

| **field** | **sample value** | **explanation** |
|:----------|:-----------------|:----------------|
| `title`   | `Look at this!`  | The text that the user submits as the post's title. |
| `text`     | `Markdown **rocks**!\n\nಠ_ಠ` | The body of the text post being submitted, with support for markdown.  |
| `sr`      | `programming`    | The subreddit the post is being submitted to. |
| `kind`    | `self`           | This must always be `self` for text posts. For link posts, see the previous section. |
| `uh`      | `f0f0f0f0`...    | The currently logged in user's modhash (see **glossary** on the [[API]] page). |

## Example Response

If the `reddit_session` cookie **and** `uh` POST Data are not present in the request, the API will return something like the following:

```javascript
{
    "jquery": [
        [0, 1, "attr", "refresh"],
        [1, 2, "call", []],
        [0, 3, "attr", "find"],
        [3, 4, "call", [".error.USER_REQUIRED"]],
        [4, 5, "attr", "show"],
        [5, 6, "call", []],
        [6, 7, "attr", "text"],
        [7, 8, "call", ["please login to do that"]],
        [8, 9, "attr", "end"],
        [9, 10, "call", []]
    ]
}
```

Otherwise, if the user is being rate-limited, the API will return something like the following:

```javascript
{
	"jquery": [
		[0, 1, "call", ["#newlink"]],
		[1, 2, "attr", "find"],
		[2, 3, "call", [".status"]],
		[3, 4, "attr", "hide"],
		[4, 5, "call", []],
		[5, 6, "attr", "html"],
		[6, 7, "call", [""]],
		[7, 8, "attr", "end"],
		[8, 9, "call", []],
		[1, 10, "attr", "captcha"],
		[10, 11, "call", ["f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0"]],
		[1, 12, "attr", "find"],
		[12, 13, "call", [".error.RATELIMIT.field-ratelimit"]],
		[13, 14, "attr", "show"],
		[14, 15, "call", []],
		[15, 16, "attr", "text"],
		[16, 17, "call", ["you are doing that too much. try again in 9 minutes."]],
		[17, 18, "attr", "end"],
		[18, 19, "call", []]
	]
}
```

Otherwise, the API will return something like the following:

```javascript
{
	"jquery": [
		[0, 1, "call", ["#newlink"]],
		[1, 2, "attr", "find"],
		[2, 3, "call", [".status"]],
		[3, 4, "attr", "hide"],
		[4, 5, "call", []],
		[5, 6, "attr", "html"],
		[6, 7, "call", [""]],
		[7, 8, "attr", "end"],
		[8, 9, "call", []],
		[1, 10, "attr", "captcha"],
		[10, 11, "call", ["f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0"]],
		[1, 12, "attr", "redirect"],
		[12, 13, "call", ["http://www.reddit.com/r/pics/comments/92dd8/test_post_please_ignore/"]]
	]
}
```

## API Reference: submit (Text Posts)

At `jquery[12][3][0]`, there is a string that represents the newly created posts's comments page.