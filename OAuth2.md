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

# Examples

Here is example code on using OAuth in various languages:

* [Python](OAuth2-Python-Example)
* [PHP](OAuth2-PHP-Example)

# What can I once I have access?

Right now  not quite all of the Reddit API is available to OAuth clients, furthermore OAuth tokens only last a short amount of time. This is slated to change in the future, but for the meantime this means OAuth is not the ideal solution for things like mobile Reddit apps.

You can look at [the Reddit API documentation page](http://www.reddit.com/dev/api) to see if a particular API has oauth support.

# Other resources

[Relevant /r/changelog entry](http://www.reddit.com/r/changelog/comments/ynxg8/reddit_change_oauth_2_bearer_token_support_for_all/)