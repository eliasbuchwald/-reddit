When creating an [OAuth 2](oauth2) app, the most important decision to make is "what kind of application am I?" Different application types have different API access patterns, and there are differences in how the reddit servers treat your app.

Web app
--------

The app runs as the backend of a web server, on a server that only YOU have access to. Select this if:

* You're going to run a webservice that requires or takes advantage of access to user accounts. For example, http://ifttt.com allows any redditor to access their webservice and set-up tasks such as automatically putting all the user's posts in their Evernote account.
* You can keep your client secret secure and safe.
* Your service is available over `http` or `https`. (It is preferred that your service be accessible over `https` to properly protect the authorization code)

Installed app
-------------

An app installed on a computer that you don't own or control. Select this if:

* Your app won't be able to keep a client secret safe and secure. For example, Android, Windows or iOS apps that access the reddit API should choose this app type, as anyone who installs the app would be able to figure out your client secret.
* You want to reddit to redirect the user to an arbitrary URI after they grant your app permissions, such as `my-cool-app:/my/redirect/uri`

Script
------

The simplest type of app. Select this if:

* YOU are the only person who will use your app. For example, you're writing a simple bot to check your own private messages once in a while or make a weekly post to a subreddit that you moderate.
* You want to jump right in and want [an easier way](OAuth2-Quick-Start-Example) to get and use tokens without going to a web browser.
