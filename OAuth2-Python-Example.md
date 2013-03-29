Here is a complete sample of Python code using the [rauth](https://github.com/litl/rauth) library.

You will need to install rauth first, e.g.:

    $ pip install rauth

This example should run as-is.

```python
from rauth import OAuth2Service

from hashlib import sha1
from random import random

import re
import json
import webbrowser

# Get a real client id and secret from:
#
#   https://ssl.reddit.com/prefs/apps
#
auth_url = 'https://ssl.reddit.com/api/v1/'
reddit = OAuth2Service('HwDvGAXEIa1IPw',
                       'tdlqGw72hvisj1EqvkWrDK8zRXM',
                       authorize_url=auth_url + 'authorize',
                       access_token_url=auth_url + 'access_token',
                       base_url='https://oauth.reddit.com/api/v1/')

redirect_uri = 'https://github.com/litl/rauth'

# CSRF protection; your app should check this value upon redirect!
state = sha1(str(random())).hexdigest()

params = {'scope': 'identity',
          'response_type': 'code',
          'redirect_uri': redirect_uri,
          'state': state,
          'duration': 'permanent'}

authorize_url = reddit.get_authorize_url(**params)

print 'Visit this URL in your browser: ' + authorize_url
webbrowser.open(authorize_url)

url_with_code = raw_input("Copy URL from your browser's address bar: ")

# Retrieve code parameter
code = re.search('\code=([^&]*)', url_with_code).group(1)

# Retrieve state parameter
returned_state = re.search('\?state=([^&]*)', url_with_code).group(1)

assert returned_state == state, 'State parameters do no match! Bailing out.'

data = {'code': code,
        'redirect_uri': redirect_uri,
        'grant_type': 'authorization_code'}

creds = (reddit.client_id, reddit.client_secret)

s = reddit.get_auth_session(data=data,
                            auth=creds,  # Basic Auth
                            decoder=json.loads)

user = s.get('me').json()
print 'Currently logged in as {name}'.format(name=user['name'])
```