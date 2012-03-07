# API: user

# Documentation
API: `http://www.reddit.com/user/USERNAME/` where `USERNAME` is the username of the account in question.  Format extensions such as `.json` or `.xml` can be appended to specify a template for the data. 

| **path**  | **description** |
|:----------|:-------------------------|
| `/`       | Returns a Listing of comments and links by the user. Returns the same result as `/overview/` or selecting the Overview tab. |
| `/overview/` | Returns comments and links submitted by the user. Returns the same result as `/`. |
| `/comments/` | Returns comments by the user. |
| `/submitted/` | Returns links submitted by the user. |
| `/liked/` | Returns links liked by the user.  Does not include comments liked by the user.  Requires the `reddit_session` cookie of the logged in user. |
| `/disliked/` | Returns links disliked by the user.  Does not include comments disliked by the user.  Requires the `reddit_session` cookie of the logged in user. |
| `/hidden/` | Returns links hidden by the user.  Does not include comments hidden by the user.  Requires the `reddit_session` cookie of that particular user. |
| `/about/` | Returns information about the user such as when it the account was created and how much karma the user has.  Cannot be accessed without a format extension.|


# How To Use

## Without Cookies
Only `/`, `/overview/`, `/comments/`, `/submitted/`, and `/about/` can be used without cookies.

To use these paths, submit a GET request to the url and substitute USERNAME with the account name.  We will use `null` as the account name in the following examples:

GET `http://www.reddit.com/user/null/` returns the HTML formatted overview of null's links and comments.

GET `http://www.reddit.com/user/null/about/.json` returns a JSON thing of type `Listing` whose children will be either a JSON thing of type `t1` or `t3`.


## With Cookies
Submit a GET to `/liked/`, `/disliked/`, or `/hidden/` with `reddit_session` set in the `cookie` request properties.

GET `http://www.reddit.com/user/null/liked/` with request property `Cookie: reddit_session=f0f0f0f0f0f0f0f0f0f0f0f0f0;`.

### Errors

If the user is not logged in or the cookie does not belong to the user being looked up, accessing pages that require a cookie will return a HTTP 404 File Not Found status code.

The formatted JSON result would be:

>{"error": 404}