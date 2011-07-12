Use `http://www.reddit.com/api/comment` to post a comment.

## POST Data

| **field** | **sample value** | **explanation** |
|:----------|:-----------------|:----------------|
| `thing_id` or `parent`      | `t3_hzko5`, `t1_c1znqz2`       | The FULLNAME (see **glossary** on the [[API]] page) of the thing or comment you are commenting on. |
| `text`     | `ಠ_ಠ`              | The markdown content of the comment you are posting.  |
| `uh`      | `f0f0f0f0`...     | The currently logged in user's modhash (see **glossary** on the [[API]] page). |

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

Otherwise, the API will return JSON data with a series of serialized jquery commands to append the new comment to the page.