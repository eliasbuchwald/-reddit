OAuth2 support allows you to use Reddit to authenticate on non-Reddit websites.

## Getting Started

First you need an application id and secret so Reddit knows your application. You get this information by going to https://ssl.reddit.com/prefs/apps and clicking "are you a developer? create an app..."

Most of the fields don't matter much, just give it a reasonable name and description. The most important thing is the redirect uri, this should be a URL where you plan to host the script that users are redirected to after allowing access.

![App Information Screen](http://i.imgur.com/rAmUe.png)

The part underlined in red is your consumer key (also called the client id), and the part in blue is your consumer secret (also called the client secret. *You should never share this.*) Hold on to them because you'll need them to write your app later.

### Other important information

This is information that you'll need in addition to your consumer key and secret to set up an OAuth app:

* Access token url: `https://oauth.reddit.com/api/v1/access_token` (Also known as the token endpoint)
* Authorize url: `https://oauth.reddit.com/api/v1/authorize` (Also known as the authorization endpoint)

## Examples

Here is example code on using OAuth in various languages:

* [Python](OAuth2-Python-Example)
* [PHP](OAuth2-PHP-Example)

## Other resources

[Relevant /r/changelog entry](http://www.reddit.com/r/changelog/comments/ynxg8/reddit_change_oauth_2_bearer_token_support_for_all/)