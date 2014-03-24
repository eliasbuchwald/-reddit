OAuth2 support allows you to use Reddit to authenticate on non-Reddit websites and applications.

*Looking to jump right in? See the [Quick Start Example](OAuth2-Quick-Start-Example), for bots and personal scripts only*

# Getting Started

First you need an application id and secret so Reddit knows your application. You get this information by going to https://ssl.reddit.com/prefs/apps and clicking "are you a developer? create an app..."

![App Information Screen](http://i.imgur.com/e2kOR1a.png)

When registering your app, it's important to choose the correct and relevant app "type," as the type determines what authentication paths your app may take. Read more on [app types](oauth2-app-types).

* Web app: Runs as part of a web service on a server you control. Can keep a secret.
* Installed app: Runs on devices you don't control, such as the user's mobile phone. *Cannot* keep a secret.
* Script app: Runs on hardware you control, such as your own laptop or server. Can keep a secret. Only has access to your account.

Be sure to give the app a reasonable name and description. The redirect uri is important - for web apps, it points to a URL on a webserver that you control.

The part underlined in red is your client secret. *You should never share this.*

Authorization
------------

In order to make requests to reddit's API via OAuth, you must act in the context of a user. The user has to let reddit.com know that they're ok with your app performing certain actions on their behalf, such as reading their subreddit subscriptions or sending a private message. In order to do so, your website or app should send the user to the authorization URL:

    https://ssl.reddit.com/api/v1/authorize?client_id=CLIENT_ID&response_type=TYPE&
        state=RANDOM_STRING&redirect_uri=URI&duration=DURATION&scope=SCOPE_STRING

Parameter | Values | Description
----------|---------|---------
`client_id` | The Client ID generated during app [registration](https://ssl.reddit.com/prefs/apps) | Tells reddit.com which app is making the request
`response_type` | `code` | Must be the string "code" - other OAuth2 authorization flows are not currently supported
`state` | A string of your choosing | You should generate a unique, possibly random, string for each authorization request. This value will be returned to you when the user visits your REDIRECT_URI after allowing your app access - you should verify that it matches the one you sent. This ensures that only authorization requests you've started are ones you finish. (You may also use this value to, for example, tell your webserver what action to take after receiving the OAuth2 bearer token)
`redirect_uri` | The redirect_uri you have specified during [registration](https://ssl.reddit.com/prefs/apps) | If this does not match the registered redirect_uri, the authorization request will fail. If authorization succeeds, the user's browser will be instructed to redirect to this location.
`duration` | Either `temporary` or `permanent` | Indicates whether or not your app needs a permanent token. All bearer tokens expire after 1 hour. If you indicate you need `permanent` access to a user's account, you will additionally receive a `refresh_token` when acquiring the bearer token. You may use the `refresh_token` to acquire a new bearer token after your current token expires. Choose `temporary` if you're completing a one-time request for the user (such as analyzing their recent comments); choose `permanent` if you will be performing ongoing tasks for the user, such as notifying them whenever they receive a private message.
`scope` | A comma-separated\* list of [scope strings](http://www.reddit.com/dev/api/oauth) | All bearer tokens are limited in what functions they may perform. You must explicitly request access to areas of the api, such as private messaging or moderator actions. See our [automatically generated API docs](http://www.reddit.com/dev/api/oauth) for a breakdown of which API endpoints require which scopes.

Error | Cause | Resolution
-----|--------|--------
User sees 403 error in browser | client_id is missing or invalid | Verify that client_id is set and correct for your app
User sees 403 error in browser | redirect_uri is invalid | Verify that redirect_uri is set and matches what is set for your app

\* *Note*: This is a slight deviation from the OAuth 2.0 specification, which states scopes should normally be space-separated.

When you send the user to the authorization URL, they will be shown what parts of their account you want access to based on the requested scopes:

![](http://i.imgur.com/1ufhEwN.png)

You will only be able to acquire a bearer token if the user decides they trust your app with the permissions (scopes) you've requested, so be sure to limit your permission request to only those that encompass the API endpoints you required.

Token Retrieval
--------------

If the user chooses to allow your application, their browser will be instructed to redirect to your app's registered `redirect_uri`. The redirect URI will have the information below attached as query parameters. You should parse the query parameters for the URI for use in the next step.

Parameter | Values | Description
----------|---------|---------
`error` | `access_denied`, others | See error table below for list of causes.
`code` | A string | A one-time use code that may be exchanged for a bearer token. See the next step
`state` | A string | This value should be the same as the one sent in the initial authorization request, and **your app should verify that it is, in fact, the same**. Your app may also do anything else it wishes with the state info, such as parse a portion of it to determine what action to perform on behalf of the user.

Error | Cause | Resolution
-----|--------|--------
`access_denied` | User chose not to grant your app permissions | Fail gracefully - let the user know you cannot continue, and be respectful of their choice to decline to use your app
`unsupported_response_type` | Invalid `response_type` parameter in initial Authorization | Ensure that the `response_type` parameter is one of the allowed values
`invalid_scope` | Invalid `scope` parameter in initial Authorization | Ensure that the `scope` parameter is a comma-separated list of valid scopes

If you didn't get an `error` and the `state` value checks out, you may then make a POST request with `code` to the following URL to retrieve your access token:

    https://ssl.reddit.com/api/v1/access_token

Include the following information in your POST data (NOT as part of the URL)

    grant_type=authorization_code&code=CODE&redirect_uri=URI

Parameter | Values | Description
----------|---------|---------
`grant_type` | `authorization_code` | Indicates that you're using the "standard" code based flow. Other values not relevant to this flow are `refresh_token` (for renewing an access token) and `password` (for script apps only)
`code` | A string | The `code` your app retrieved above
`redirect_uri` | The redirect_uri registered to your app | Yes, you need it here again, and yes, it must match exactly.

Error | Cause | Resolution
-----|--------|--------
401 response | Client credentials sent as HTTP Basic Authorization were invalid | Verify that you are properly sending HTTP Basic Authorization headers and that your credentials are correct
`unsupported_grant_type` | `grant_type` parameter was invalid | Verify that the `grant_type` sent is supported
`NO_TEXT` for field `code` | You didn't include the `code` parameter | Include the `code` parameter in the POST data
`invalid_grant` | The `code` has expired or already been used | Ensure that you are not attempting to re-use old `code`s - they are one time use.

The response from this request, if successful, will be JSON of the following format:

    {
        "access_token": Your access token,
        "token_type": "bearer",
        "expires_in": Unix Epoch Seconds,
        "scope": A scope string,
        "refresh_token": Your refresh token
    }

"refresh_token" will only be present if one was requested. You may now make API requests to reddit's servers on behalf of that user, by including the following header in your HTTP requests:

    Authorization: bearer TOKEN

**API requests with a bearer token should be made to `http://oauth.reddit.com`, NOT ssl.reddit.com.**

Refreshing the token
------------------

Access tokens expire after one hour. If your app requires access after that time, it must request a refresh token by including `duration=permanent` with the authorization request (see above). When the current access token expires, your app should send make another POST request to the access token URL:

    https://ssl.reddit.com/api/v1/access_token

Include the following information in your POST data (NOT as part of the URL)

    grant_type=authorization_code&code=CODE&redirect_uri=URI

Parameter | Values | Description
----------|---------|---------
`grant_type` | `refresh_token` | Indicates that you're requesting a new access token using a refresh token
`refresh_token` | A string | The refresh token retrieved during the initial request for an access token

Error | Cause | Resolution
-----|--------|--------
401 response | Client credentials sent as HTTP Basic Authorization were invalid | Verify that you are properly sending HTTP Basic Authorization headers and that your credentials are correct
`unsupported_grant_type` | `grant_type` parameter was invalid | Verify that the `grant_type` sent is supported
`NO_TEXT` for field `refresh_token` | `refresh_token` parameter was missing | Include a valid `refresh_token` in the POST parameters

The response will look the same as the initial access token request.

    {
        "access_token": Your access token,
        "token_type": "bearer",
        "expires_in": Unix Epoch Seconds,
        "scope": A scope string,
    }

# Examples

Here is example code on using OAuth in various languages:

* [Simple Script Example](OAuth2-Quick-Start-Example)
* [Python Web Server Example](OAuth2-Python-Example)
* [Python (with PRAW)](https://praw.readthedocs.org/en/latest/pages/oauth.html)
* [PHP](OAuth2-PHP-Example)

# What can I do once I have access?

You can look at [the Reddit API documentation page](http://www.reddit.com/dev/api) to see if a particular API has OAuth support and what scope it requires.

# Other resources

Be sure to watch [/r/redditdev](http://www.reddit.com/r/redditdev) for important OAuth 2 related announcements.