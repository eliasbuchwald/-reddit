*This guide shows example code for an iOS app that connects to a reddit account. For details on each step, see the [full OAuth2 login docs](oauth2).*

OAuth2 Web App Sample code
======================

First Steps
----------

Go to your [app preferences](https://ssl.reddit.com/prefs/apps). Click the "Create app" or "Create another app" button. Fill out the form like so:

* **name**: My Example App
* App type: Choose the **installed app** option
* **description**: You can leave this blank
* **about url**: You can leave this blank
* **redirect url**: myappscheme://somethingorblank

Hit the "create app" button. Make note of the client ID. Installed apps do not use secrets. For the rest of this page, it will be assumed that:

* Your app's client ID is: `Bq9yaGfIVdJXTQ`
* Your app's redirect URL is: `myappscheme://response`

*Note:* The client IDs, tokens and passwords used here are, obviously, fake and invalid.

###How Installed app OAuth flows work
1. App opens a webpage for the user to login and authenticate with reddit.
2. User authenticates in webview, and accepts app permissions in webview.
3. App receives a callback via the registered app scheme.
4. Your app dismisses the webview used for registration.

*Note: Apple has rejected apps that go to Safari for the authentication process. We encourage you to do this in your app.*


Register your scheme
----
In your Xcode project, navigate to the `Info` tab of Target settings. Add your URL scheme to the `URL Types` section. Here's a sample:  

![](http://i.imgur.com/LxYu6nG.png)

Setup your AppDelegate
------------------------------
The UIApplicationDelegate protocol has a method for handling URLs called `application:openURL:sourceApplication:annotation`. Implement it and you will be able to receive the authorization code without having to redirect users outside of your app.

Once OAuth is complete, you will receive a callback with a URL that looks something like this: `myappscheme://response/?state=TEST&code=5HyYTQ1xc76kx2PLwe8Bnlr2L32`.

You will need to parse out the `code` from the query string. Below is a sample of what that the AppDelegate method looks like:

```
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation
{
    if ([url.scheme isEqualToString:@"myappscheme"]) {
        NSArray *queryParams = [[url query] componentsSeparatedByString:@"&"];
        NSArray *codeParam = [queryParams filteredArrayUsingPredicate:[NSPredicate predicateWithFormat:@"SELF BEGINSWITH %@", @"code="]];
        NSString *codeQuery = [codeParam objectAtIndex:0];
        NSString *code = [codeQuery stringByReplacingOccurrencesOfString:@"code=" withString:@""];
        NSLog(@"My code is %@", code);
        // Finish the OAuth flow with this code
        return YES;
    }
    
    return NO;
}
```

Showing the Authentication Screen
--------
You will need to create an [Authorization URL](oauth2#authorization) to show to the user in a web view.

Since this is an installed application, you always want to request `duration=permanent` to get a refresh token.

Example:

```
NSURL *authorizationURL = [NSURL URLWithString:@"https://ssl.reddit.com/api/v1/authorize?client_id=Bq9yaGfIVdJXTQ&response_type=code&state=TEST&redirect_uri=myappscheme://response&duration=permanent&scope=read"];
NSURLRequest *request = [NSURLRequest requestWithURL:authorizationURL];
[self.webView loadRequest:request];
 ```