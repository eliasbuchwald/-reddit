The following documentation describes the data structure of object returned when using the Reddit API.  Items attached with a `?` are not definite and are subject to change.  Validation and peer review are needed for those items.  However, they should not be taken as literal question marks.

### thing (superclass)
All `thing`s except listing have these attributes:

| **type**  | **name**                 | **description** |
|:----------|:-------------------------|:----------------|
| `String`  | `id`                     | this item's identification ex: "8xwlg" |
| `String`  | `name`                   | Fullname of comment.  Ex: "t1_c3v7f8u" |
| `String`  | `kind`                   | All `thing`s have a `kind`.  The kind is a String identifier that denotes the object's type.  Some examples: `Listing`, `more`, `t1`, `t2` |
| `Object`  | `data`                   | A custom data structure used to hold valuable information.  This object's format will follow the data structure respective of it's kind.  See below for specific structures.  Some `thing`s do not use data structures.  Ex: More|



### Listing
Used to [paginate](http://en.wikipedia.org/wiki/Pagination) content that is too long to display in one go.  Add the query argument `before` or `after` with the value given to get the previous or next page.  This is usually used in conjunction with a `count` argument.

**Exception**:  This `thing` does not seem to subclass `thing`.  Although it does have `data` and `kind` as parameters, it does not have `id` and `name`.  A listing's `kind` will always be `Listing` and it's data will be a List of `thing`s.

| **type**  | **name**                 | **description** |
|:----------|:-------------------------|:----------------|
| `String`  | `before`                 | The fullname of the listing that follows before this page. |
| `String`  | `after`                  | The fullname of the listing that follows after this page. |
| `String`  | `modhash`                | This modhash is not the same modhash provided upon login.  You do not need to update your user's modhash everytime you get a new modhash.  You can reuse the modhash given upon login. |

### votable (implementation)
All `thing`s that implement `votable` have these attributes:

| **type**  | **name**                 | **description** |
|:----------|:-------------------------|:----------------|
| `int`     | `ups`                    | the number of upvotes.  does this include one's own upvote? |
| `int`     | `downs`                  | the number of downvote.  does this include one's own downvote? |
| `Boolean` | `likes`                  | true if thing is liked by the user.  false if thing is disliked.  null if the user is neutral on the thing.  Certain languages such as Java may need to use a boolean wrapper that supports null assignment.|

### created (implementation)
All `thing`s that implement `created` have these attributes:

| **type**  | **name**                 | **description** |
|:----------|:-------------------------|:----------------|
| `long`?   | `created`                | the localized time of creation. ex: "1331042771.0" |
| `long`?   | `created_utc`            | the UTC time this item was created since the unix start time in seconds. "1331017571.0"|


### Data Structures

##### comment (implements votable | created)
| **type**  | **name**                 | **description** |
|:----------|:-------------------------|:----------------|
| `String`  | `author`                 | the account name of the poster |
| `String`  | `author_flair_css_class` | the css class of the author's flair.  subreddit specific |
| `String`  | `author_flair_text`      | the text of the author's flair.  subreddit specific |
| `String`  | `body`                   | the raw text.  this is the unformatted text which includes the raw markup characters such as `**` for bold. |
| `String`  | `body_html`              | the formatted html text.  this is the html formatted version of the marked up text.  Items that are boldened by `**` or `***` will now have `<em>` or `***` tags on them. Additionally, bullets and numbered lists will now be in html list format. ***NOTE:*** The html string will be escaped.  You must unescape to get the raw html.|
| `String`  | `link_id`                |  |
| `String`  | `parent_id`              |  |
| `String`  | `subreddit`              | subreddit of thing excluding the /r/ prefix. "pics" |
| `String`  | `subreddit_id`           |  |

##### link (implements votable | created)
| **type**  | **name**                 | **description** |
|:----------|:-------------------------|:----------------|
| `String`  | `author`                 | the account name of the poster |
| `String`  | `author_flair_css_class` | the css class of the author's flair.  subreddit specific |
| `String`  | `author_flair_text`      | the text of the author's flair.  subreddit specific |
| `boolean` | `clicked`                | probably always returns false |
| `String`  | `domain`                 | the domain of this link.  Self posts will be `self.reddit.com` while other examples include `en.wikipedia.org` and `s3.amazon.com` |
| `boolean` | `hidden`                 | true if the post is hidden by the logged in user.  false if not logged in or not hidden. |
| `boolean` | `is_self`                | true if this link is a selfpost |
| `Object`  | `media`                  | unknown.  I need someone else to document this as I haven't done much research into this |
| `Object`  | `media_embed`            | unknown.  I need someone else to document this as I haven't done much research into this |
| `int`     | `num_comments`           | the number of comments that belong to this link |
| `boolean` | `over_18`                | true if the post is tagged as NSFW.  False if otherwise |
| `String`  | `permalink`              | relative url of the permanent link for this link |
| `boolean` | `saved`                  | true if this post is saved by the logged in user |
| `int`     | `score`                  | the net-score of the link |
| `String`  | `selftext`               | the raw text.  this is the unformatted text which includes the raw markup characters such as `**` for bold. |
| `String`  | `selftext_html`          | the formatted escaped html text.  this is the html formatted version of the marked up text.  Items that are boldened by `**` or `***` will now have `<em>` or `***` tags on them. Additionally, bullets and numbered lists will now be in html list format. ***NOTE:*** The html string will be escaped.  You must unescape to get the raw html.|
| `String`  | `subreddit`              |  |
| `String`  | `subreddit_id`           |  |
| `String`  | `thumbnail`              | full url to the thumbnail for this link |
| `String`  | `title`                  |  |
| `String`  | `url`                    | the link of this post.  the permalink if this is a self-post |


##### subreddit
| **type**  | **name**                 | **description** |
|:----------|:-------------------------|:----------------|
| `String`  | `description`            |  |
| `String`  | `display_name`           |  |
| `boolean` | `over18`                 | is_nsfw? |
| `long`    | `subscribers`            | the number of redditors subscribed to this subreddit |
| `String`  | `title`                  |  |
| `String`  | `url`                    | The relative URL of the subreddit.  Ex: "/r/pics/" |

##### message (implements created)
| **type**  | **name**                 | **description** |
|:----------|:-------------------------|:----------------|
| `String`  | `author`                 |  |
| `String`  | `body`                   |  |
| `String`  | `body_html`              |  |
| `String`  | `context`                | does not seem to return null but an empty string instead. |
| `Message`?| `first_message`          |  |
| `String`  | `name`                   | ex: "t4_8xwlg" |
| `boolean` | `new`                    | unread?  not sure |
| `String`  | `parent_id`              | null if no parent is attached |
| `String`  | `replies`                | Again, an empty string if there are no replies. |
| `String`  | `subject`                | subject of message |
| `String`  | `subreddit`              | null if not a comment. |
| `boolean` | `was_comment`            |  |


##### account


##### more
