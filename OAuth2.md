OAuth2 support allows you to use Reddit to authenticate on non-Reddit websites.

# Getting Started

First you need an application id and secret so Reddit knows your application. You get this information by going to https://ssl.reddit.com/prefs/apps and clicking "are you a developer? create an app..."

Most of the fields don't matter much, just give it a reasonable name and description. The most important thing is the redirect uri, this should be a URL where you plan to host the script that users are redirected to after allowing access.

![App Information Screen](http://i.imgur.com/rAmUe.png)

The part underlined in red is your consumer key (also called the client id), and the part in blue is your consumer secret (also called the client secret. *You should never share this.*) Hold on to them because you'll need them to write your app later.

## Other important information

This is information that you'll need in addition to your consumer key and secret to set up an OAuth app:

* Authorization url: `https://ssl.reddit.com/api/v1/authorize` (Also known as the authorization endpoint)
* Access token url: `https://ssl.reddit.com/api/v1/access_token` (Also known as the token endpoint)

The authorization url is where you send a user's browser, so they can grant limited access to their account to your application. If the user grants access, they are redirected to your redirect uri with a code. Your app then makes a call to the access token url, using the code it received, to obtain the access token.

Once your app receives an access token, it can use it on the https://oauth.reddit.com domain to make API calls as that user (until the token expires or is revoked by the user). Note the different domains involved:

* https://ssl.reddit.com - for authorization and access_token fetching
* https://oauth.reddit.com - for API calls with an access token

# Authorization Parameters

In addition to the standard authorization parameters (response_type, client, redirect_uri), you may specify these additional parameters that may be important to the development and usage of your application:

* <u>scope</u> - In order to use oauth approved calls on [this page](http://www.reddit.com/dev/api/oauth), you must specify the scope from which the call originates from. For example, if you wish to use the '/api/v1/me' call, you must specify the **identity** scope. If you wish to use '/api/approve' and '/api/remove', specify the **modposts** scope. If you need to use more than one scope, they should be specified in a comma delimited manner.
* <u>state</u> - You can pass a value into the authorization page that will be included in the redirect back to you if the user grants access. This is useful for preventing cross-site request forgery (CSRF). By choosing (and remembering) a sufficiently random value, you can ensure that the request that comes back to your site was initiated by visiting the authorization URL you generated.
* <u>duration</u> - Duration is useful if you require a permanent token. By default if duration is not specified, _temporary_ will be chosen. The alternative is to specify _permanent_ and a permanent token will be issued. The user will also be aware you are requesting permanent access. Learn more about refreshing that token [here](http://www.reddit.com/r/changelog/comments/11jab9/reddit_change_permanent_oauth_grants_using/).

# Examples

Here is example code on using OAuth in various languages:

* [Python](OAuth2-Python-Example)
* [Python (with PRAW)](https://github.com/praw-dev/praw/wiki/OAuth)
* [PHP](OAuth2-PHP-Example)

# What can I do once I have access?

Right now  not quite all of the Reddit API is available to OAuth clients, <strike>furthermore OAuth tokens only last a short amount of time. This is slated to change in the future, but for the meantime this means OAuth is not the ideal solution for things like mobile Reddit apps</strike> (permanent tokens are [now available](http://www.reddit.com/r/changelog/comments/11jab9/reddit_change_permanent_oauth_grants_using/)).

You can look at [the Reddit API documentation page](http://www.reddit.com/dev/api) to see if a particular API has oauth support.

# Other resources

[Relevant /r/changelog entry](http://www.reddit.com/r/changelog/comments/ynxg8/reddit_change_oauth_2_bearer_token_support_for_all/)