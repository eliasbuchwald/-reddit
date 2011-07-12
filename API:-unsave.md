Use `http://www.reddit.com/api/unsave` to save a post.

**Note:** Even though there is no extension, the API still returns JSON.

## POST Data

| **field** | **sample value** | **explanation** |
|:----------|:-----------------|:----------------|
| `id`      | `t3_6nw57`       | The FULLNAME (see **glossary** on the [[API]] page) of the post you are unsaving. |
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

Otherwise, the API will return the following, indicating the post was unsaved successfully:

```javascript
{}
```