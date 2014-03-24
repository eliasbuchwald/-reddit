*This guide shows example code for a web service that connects to a reddit account. For details on each step, see the [full OAuth2 login docs](/r/redditdev/wiki/oauth2). For a simpler use case, see the [script app quick start guide](/r/redditdev/wiki/oauth2/quickstart).*

OAuth2 Web App Sample code
======================

First Steps
----------

Go to your [app preferences](/prefs/apps). Click the "Create app" or "Create another app" button. Fill out the form like so:

* **name**: My Example App
* App type: Choose the **web app** option
* **description**: You can leave this blank
* **about url**: You can leave this blank
* **redirect url**: http://localhost:65010/reddit_callback

*Note*: For an actual app, you'd want to set this to you a URL that your user could access rather than `localhost`. It is also STRONGLY recommended to serve your app over `https` to protect the `authorization_code` from middlemen.

Hit the "create app" button. Make note of the client ID and client secret. For the rest of this page, it will be assumed that:

* Your app's client ID is: `p-jcoLKBynTLew`
* Your app's client secret is: `gko_LXELoV07ZBNUXrvWZfzE3aI`

*Note: You should NEVER post your client secret in public.* The client IDs, secrets, tokens and passwords used here are, obviously, fake and invalid.

Python Flask webserver example
------------------------------

*The following Python example relies on the [Flask](http://flask.pocoo.org/) web framework and the Python [requests](http://docs.python-requests.org) library*

For the sake of the example, configuration values are hardcoded into the python script and imports are done in the functions used. reddit recommends using external configuration, such as an [ini file](http://docs.python.org/2.7/library/configparser.html‎) and following [PEP 008](https://www.python.org/dev/peps/pep-0008) guidelines for organizing imports at the top of the file.

### Authorization

Serve a webpage on `http://localhost:65010/` that creates an "authorization" link out to reddit.com:

    CLIENT_ID = "p-jcoLKBynTLew"
    CLIENT_SECRET = "gko_LXELoV07ZBNUXrvWZfzE3aI"
    REDIRECT_URI = "http://localhost:65010/reddit_callback"

    from flask import Flask
    app = Flask(__name__)
    @app.route('/')
    def homepage():
        text = '<a href="%s">Authenticate with reddit</a>'
        return text % make_authorization_url()

    def make_authorization_url():
        # Generate a random string for the state parameter
        # Save it for use later to prevent xsrf attacks
        from uuid import uuid4
        state = str(uuid4())
        save_created_state(state)
        params = {"client_id": CLIENT_ID,
                  "response_type": "code",
                  "state": state,
                  "redirect_uri": REDIRECT_URI,
                  "duration": "temporary",
                  "scope": "identity"}
        import urllib
        url = "https://ssl.reddit.com/api/v1/authorize?" + urllib.urlencode(params)
        return url
    
    # Left as an exercise to the reader.
    # You may want to store valid states in a database or memcache,
    # or perhaps cryptographically sign them and verify upon retrieval.
    def save_created_state(state):
        pass
    def is_valid_state(state):
        return True
    
    
    if __name__ == '__main__':
        app.run(debug=True, port=65010)

### Authorization Code Retrieval

Running the above code serves a simple text page with a URL that sends the user to reddit to request that this app be able to access their reddit identity. If the user chooses to "allow" the app to access their reddit identity, they'll be sent to the REDIRECT_URI (configured during app registration) of `http://localhost:65010/reddit_callback` with additional information in the query parameters. The Flask server needs to be configured to respond to that URL:

    from flask import abort, request
    @app.route('/reddit_callback')
    def reddit_callback():
        error = request.args.get('error', '')
        if error:
            return "Error: " + error
        state = request.args.get('state', '')
        if not is_valid_state(state):
            # Uh-oh, this request wasn't started by us!
            abort(403)
        code = request.args.get('code')
        # We'll change this next line in just a moment
        return "got a code! %s" % code

### Turning a code into an OAuth access token

The Flask app now handles reddit's redirect back into the app, but doesn't quite do anything yet. The next step is to take that code and get an OAuth access token with it. The code is only good once!

First, change the last line of that previous function from:

        return "got a code! %s" % code

to:

        return "got an access token! %s" % get_token(code)

Then, add the `get_token(code)` function:

    import requests
    import requests.auth
    def get_token(code):
        client_auth = requests.auth.HTTPBasicAuth(CLIENT_ID, CLIENT_SECRET)
        post_data = {"grant_type": "authorization_code",
                     "code": code,
                     "redirect_uri": REDIRECT_URI}
        response = requests.post("https://ssl.reddit.com/api/v1/access_token",
                                 auth=client_auth,
                                 data=post_data)
        token_json = response.json()
        return token_json["access_token"]

### You've got the token, now use it!

Alright, take a deep breath. The hard part is over. Now you have a token, and your app can use it to make requests to the reddit API on behalf of that user until it expires. (If it expires, the user will need to re-authorize your app. Alternatively, if you need to have ongoing access on behalf of the user, you could use a [refresh token](/r/redditdev/wiki/oauth2#wiki_refreshing_the_token). That flow is beyond the scope of this example.)

As an example, we'll request the user's name and display it to them as the content of the page we show the user after they authorize the app. First, change the last part of the callback function one more time from:

        return "got an access token! %s" % get_token(code)

to:

        access_token = get_token(code)
        return "Your reddit username is: %s" % get_username(access_token)

Then add the `get_username` function:

    def get_username(access_token):
        headers = {"Authorization": "bearer " + access_token}
        response = requests.get("https://oauth.reddit.com/api/v1/me", headers=headers)
        me_json = response.json()
        return me_json['name']

That's it! Now you can run your app. [See the full source file](OAuth2-Python-Example-Source)
