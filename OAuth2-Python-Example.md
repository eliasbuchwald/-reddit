Here is an incomplete sample of Python code using the [rauth](https://github.com/litl/rauth/) library, provided by [/u/intortus](http://www.reddit.com/user/intortus):

```python
from rauth import OAuth2Service

reddit = OAuth2Service(name='reddit',
                       client_id=CLIENT_ID,
                       client_secret=CLIENT_SECRET,
                       access_token_url='https://ssl.reddit.com/api/v1/access_token',
                       authorize_url='https://ssl.reddit.com/api/v1/authorize',
                       base_url='http://www.reddit.com/api/')

authorize_url = reddit.get_authorize_url(response_type='code',
                                         scope='identity',
                                         state='...',  # some unguessable value to prevent CSRF
                                         redirect_uri=CLIENT_REDIRECT_URI)

# The should now authorize the application by following this link:
print authorize_url

...

# The user will then be redirected back to our application:
code = request.args.get('code')

if code is None:
    print "Whoops, you didn't authorize the app!"

s = reddit.get_auth_session(data={'grant_type': 'authorization_code',
                                  'code': code,
                                  'redirect_uri': CLIENT_REDIRECT_URI})

print s.access_token

# Now make further requests with the `s` session object, e.g.: `r = s.get('me.json')`
```