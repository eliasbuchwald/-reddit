Use `http://www.reddit.com/api/flaircsv.json` to post a CSV file of flair settings to a subreddit.

**Note:** If you post without the `.json` extension, you'll receive a 404 response with no useful data, but your changes will still take effect.

## POST Data

| **field** | **sample value** | **explanation** |
|:------------|:-----------------|:----------------|
| `r`         | `motorcycles`    | The name of the subreddit.
| `flair_csv` | `intortus,ZZR600,kawasaki\n`... | CSV file contents, up to 100 lines (see below). |
| `uh`        | `f0f0f0f0`...     | The currently logged in user's modhash (see **glossary** on the [[API]] page). |

The CSV "file" should be a newline-separated list of flair settings to apply. Each line should give the username, flair text, and flair CSS class in this order, separated by commas. A line like `intortus,,` (where the flair text and CSS class are empty strings) will *remove* flair for the given user.

## JSON Response

The response will be an array with a result object for each line in the CSV file. Each result object will indicate whether a flair setting was successfully applied, or the reason why not.

## Example Response

```javascript
[
  {
    'ok': True,
    'status': 'added flair for user intortus',
    'errors': {},
    'warnings': {}
  }
]
```
