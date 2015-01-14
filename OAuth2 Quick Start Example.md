*This is a stripped down, quick start guide to getting a script to make OAuth2 API requests. If you want more details, see the [full OAuth2 login docs](OAuth2).*

First Steps
----------

Go to your [app preferences](https://www.reddit.com/prefs/apps). Click the "Create app" or "Create another app" button. Fill out the form like so:

* **name**: My Example App
* **App type**: Choose the **script** option
* **description**: You can leave this blank
* **about url**: You can leave this blank
* **redirect url**: http://www.example.com/unused/redirect/uri (We won't be using this as a redirect)

Note: These examples **will only work for `script` type apps,** which will ONLY have access to accounts registered as "developers" of the app and require the application to know the user's password. Read more about [app types](OAuth2-App-Types).

Hit the "create app" button. Make note of the client ID and client secret. For the rest of this page, it will be assumed that:

* Your username is: `reddit_bot`
* Your password is: `snoo`
* Your app's client ID is: `p-jcoLKBynTLew`
* Your app's client secret is: `gko_LXELoV07ZBNUXrvWZfzE3aI`

*Note: You should NEVER post your client secret (or your reddit password) in public.* If you create a bot, you should take steps to ensure that the bot's password and the app's client secret are secured against digital theft. The client IDs, secrets, tokens and passwords used here are, obviously, fake and invalid.

Curl Example
-----------

**Request a token.** Notice that for *acquiring* a token, requests are made to `https://www.reddit.com`

    reddit@reddit-VirtualBox:~$ curl -X POST -d 'grant_type=password&username=reddit_bot&password=snoo' --user 'p-jcoLKBynTLew:gko_LXELoV07ZBNUXrvWZfzE3aI' https://www.reddit.com/api/v1/access_token
    {
        "access_token": "J1qK1c18UUGJFAzz9xnH56584l4", 
        "expires_in": 3600, 
        "scope": "*", 
        "token_type": "bearer"
    }

**Use the token to get identity information.** Notice that for *using* the token, requests are made to `https://oauth.reddit.com`.

    reddit@reddit-VirtualBox:~$ curl -H "Authorization: bearer J1qK1c18UUGJFAzz9xnH56584l4" -A "ChangeMeClient/0.1 by YourUsername" https://oauth.reddit.com/api/v1/me
    {
        "comment_karma": 0, 
        "created": 1389649907.0, 
        "created_utc": 1389649907.0, 
        "has_mail": false, 
        "has_mod_mail": false, 
        "has_verified_email": null, 
        "id": "1", 
        "is_gold": false, 
        "is_mod": true, 
        "link_karma": 1, 
        "name": "reddit_bot", 
        "over_18": true
    }

That's all there is to it!

Python example
---------------

*This example uses the Python [requests library](http://docs.python-requests.org/en/latest/).* An [IPython](http://ipython.org) prompt is shown instead of the standard python prompt.

**Request a token.** Notice that for *acquiring* a token, requests are made to `https://www.reddit.com`.

    In [1]: import requests
    In [2]: import requests.auth
    In [3]: client_auth = requests.auth.HTTPBasicAuth('p-jcoLKBynTLew', 'gko_LXELoV07ZBNUXrvWZfzE3aI')
    In [4]: post_data = {"grant_type": "password", "username": "reddit_bot", "password": "snoo"}
    In [5]: response = requests.post("https://www.reddit.com/api/v1/access_token", auth=client_auth, data=post_data)
    In [6]: response.json()
    Out[6]: 
    {u'access_token': u'fhTdafZI-0ClEzzYORfBSCR7x3M',
     u'expires_in': 3600,
     u'scope': u'*',
     u'token_type': u'bearer'}

**Use the token.** Notice that for *using* the token, requests are made to `https://oauth.reddit.com`.

    In [7]: headers = {"Authorization": "bearer fhTdafZI-0ClEzzYORfBSCR7x3M", "User-Agent": "ChangeMeClient/0.1 by YourUsername"}
    In [8]: response = requests.get("https://oauth.reddit.com/api/v1/me", headers=headers)
    In [9]: response.json()
    Out[9]: 
    {u'comment_karma': 0,
     u'created': 1389649907.0,
     u'created_utc': 1389649907.0,
     u'has_mail': False,
     u'has_mod_mail': False,
     u'has_verified_email': None,
     u'id': u'1',
     u'is_gold': False,
     u'is_mod': True,
     u'link_karma': 1,
     u'name': u'reddit_bot',
     u'over_18': True}
