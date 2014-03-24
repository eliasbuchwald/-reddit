```python
    #!/usr/bin/env python
    from flask import Flask, abort, request
    from uuid import uuid4
    import requests
    import requests.auth
    import urllib
    CLIENT_ID = None # Fill this in with your client ID
    CLIENT_SECRET = None # Fill this in with your client secret
    REDIRECT_URI = "http://localhost:65010/reddit_callback"


    app = Flask(__name__)
    @app.route('/')
    def homepage():
        text = '<a href="%s">Authenticate with reddit</a>'
        return text % make_authorization_url()

    def make_authorization_url():
        # Generate a random string for the state parameter
        # Save it for use later to prevent xsrf attacks
        state = str(uuid4())
        save_created_state(state)
        params = {"client_id": CLIENT_ID,
                  "response_type": "code",
                  "state": state,
                  "redirect_uri": REDIRECT_URI,
                  "duration": "temporary",
                  "scope": "identity"}
        url = "https://ssl.reddit.com/api/v1/authorize?" + urllib.urlencode(params)
        return url

    # Left as an exercise to the reader.
    # You may want to store valid states in a database or memcache.
    def save_created_state(state):
        pass
    def is_valid_state(state):
        return True

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
        access_token = get_token(code)
        return "Your reddit username is: %s" % get_username(access_token)
    
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
    
    
    def get_username(access_token):
        headers = {"Authorization": "bearer " + access_token}
        response = requests.get("https://oauth.reddit.com/api/v1/me", headers=headers)
        me_json = response.json()
        return me_json['name']
    
    if __name__ == '__main__':
        app.run(debug=True, port=65010)
```