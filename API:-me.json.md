Use `http://www.reddit.com/api/me.json` to grab information about the currently logged in user.

**Note:** Using the `.xml` extension still returns JSON.

## Example Response

If the `reddit_session` cookie is not present in the request, the API will return the following:

```javascript
{}
```

Otherwise, the API will return something like the following about the currently logged in user:

```javascript
{
    "kind": "t2",
    "data": {
        "has_mail": false,
        "name": "username",
        "created": 1213716360.0,
        "modhash": "f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0",
        "created_utc": 1213716360.0,
        "link_karma": 5000,
        "comment_karma": 10000,
        "is_gold": false,
        "is_mod": true,
        "id": "0ffff",
        "has_mod_mail": false
    }
}
```

## API Reference: me.json

- ###kind  *(string)*

     Description

- ###data *(object)*

    Holds relevant user data.

    - ###has_mail *(boolean)*

        Says whether the user has unread mail from [messaging](http://www.reddit.com/help/messaging) or [comment/post replies](http://www.reddit.com/help/commenting).

    - ###name *(string)*

        Logged in user's username

    - ###created *(number)*

        [Unix time](http://en.wikipedia.org/wiki/Unix_time) that the user's account was created in __the time zone the server is set to?__.

    - ###modhash *(string)*

        The user's modhash (see **glossary** on the [[API|]] page)

    - ###created_utc *(string)*

        [Unix time](http://en.wikipedia.org/wiki/Unix_time) that the user's account was created in [UTC](http://en.wikipedia.org/wiki/Coordinated_Universal_Time)

    - ###link_karma *(number)*

        The user's link karma.

    - ###comment_karma *(number)*

        The user's comment karma.

    - ###is_gold *(boolean)*

        Says whether the user is a [reddit gold](http://www.reddit.com/help/gold) member.

    - ###is_mod *(boolean)*

        Says whether the user is a [moderator](http://www.reddit.com/help/moderation).

    - ###id *(string)*

        The user's ID. This is only used internally__, right?__.

    - ###has_mod_mail *(boolean)*

        Says whether the user has unread [moderator mail](http://www.reddit.com/help/moderation#Whatismoderatormail).