Use `http://www.reddit.com/api/flairlist` to download the flair assignments of a subreddit.

This is a "paginated" API. If your subreddit has a lot of flair, you won't be able to fetch it all in a single request. In that case the result of the first query will provide the parameters you'll need to fetch the "next" page of results.

**Note:** Even though there is no extension, the API still returns JSON.

## GET Data

| **field** | **sample value** | **explanation** |
|:----------|:-----------------|:----------------|
| `r`       | `motorcycles`    | The name of the subreddit. |
| `limit`   | `100`            | The maximum number of items to return (up to 1000). |
| `after`   | `t2_39qab`       | Return entries starting *after* this user. |
| `before`  | `t2_39qab`       | Return entries that come just *before* this user. |
| `uh`      | `f0f0f0f0`...     | The currently logged in user's modhash (see **glossary** on the [[API]] page). |

Flair is sorted according to the id36 of users.

## Example Response

You might see something like this on a `limit=1` request where `after` is also given:

```javascript
{
  "next": "t2_39qab",
  "prev": "t2_39qab",
  "users": [
    {
      "user": "intortus",
      "flair_css_class": "kawasaki",
      "flair_text": "ZZR600"
    }
  ]
}
```
