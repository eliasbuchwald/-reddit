Use `http://www.reddit.com/api/flair` to set (or clear) a user's flair in a subreddit.


**Note:** Even though there is no extension, the API still returns JSON.

## POST Data

| **field** | **sample value** | **explanation** |
|:----------|:-----------------|:----------------|
| `r`       | `motorcycles`    | The name of the subreddit. |
| `name`    | `intortus`       | The name of the user. |
| `text`    | `ZZR600`         | The flair text to assign. |
| `css_class` | `kawasaki`       | The CSS class to assign to the flair text. |
| `uh`      | `f0f0f0f0`...     | The currently logged in user's modhash (see **glossary** on the [[API]] page). |

**Note:** If the empty string is assigned to both `text` and `css_class`, then flair for the user will be *removed*.

## Example Response

```javascript
{"jquery": [
  [0, 1, "call", ["#flair-1"]],
  [1, 2, "attr", "find"],
  [2, 3, "call", [".status"]],
  [3, 4, "attr", "hide"],
  [4, 5, "call", []],
  [5, 6, "attr", "html"],
  [6, 7, "call", [""]],
  [7, 8, "attr", "end"],
  [8, 9, "call", []],
  [1, 10, "attr", "find"],
  [10, 11, "call", [".status"]],
  [11, 12, "attr", "show"],
  [12, 13, "call", []],
  [13, 14, "attr", "html"],
  [14, 15, "call", ["saved"]],
  [15, 16, "attr", "end"],
  [16, 17, "call", []],
  [1, 18, "attr", "find"],
  [18, 19, "call", [".user"]],
  [19, 20, "attr", "show"],
  [20, 21, "call", []],
  [21, 22, "attr", "html"],
  [22, 23, "call", ["\n\n\n\n\n    \n  \n\n  \n&lt;a \n    href=\"http://www.reddit.com/user/intortus\" class=\"author id-t2_39qab\" &gt;intortus&lt;/a&gt;\n\n\n      \n    &lt;span class=\"flair flair-kawasaki\"&gt;ZZR600&lt;/span&gt;\n\n    &lt;span class=\"userattrs\"&gt;\n    &lt;/span&gt;\n\n"]],
  [23, 24, "attr", "end"],
  [24, 25, "call", []]
]}
```