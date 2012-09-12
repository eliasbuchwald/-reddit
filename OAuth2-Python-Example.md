Here is an incomplete sample of Python code using the [rauth](https://github.com/litl/rauth/) library, provided by [/u/intortus](http://www.reddit.com/user/intortus):

    auth = rauth.service.OAuth2Service(
        name="reddit",
        consumer_key=CLIENT_ID,
        consumer_secret=CLIENT_SECRET,
        access_token_url="https://oauth.reddit.com/api/v1/access_token",
        authorize_url="https://oauth.reddit.com/api/v1/authorize")

    # first, make the user follow this link:
    authorize_url = auth.get_authorize_url(
        response_type="code",
        scope="identity",
        state="...", # some unguessable value to prevent CSRF
        redirect_uri=CLIENT_REDIRECT_URI)

    # when user is redirected back after authorizing:
    code = request.args["code"]
    response = auth.get_access_token(
        auth=(auth.consumer_key, auth.consumer_secret),
        data=dict(
            grant_type="authorization_code",
            code=code,
            redirect_uri=CLIENT_REDIRECT_URI))
    access_token = response.content["access_token"]