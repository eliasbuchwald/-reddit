# Documentation
`http://www.reddit.com/saved/` will return links saved by the user in order of most recently saved first.

# How To Use
Submit a GET to `http://www.reddit.com/saved/` with `reddit_session` set in the `cookie` request properties.  Format extensions such as `.json` or `.xml` can be appended to specify a template for the data.

GET `http://www.reddit.com/saved/` with request property `Cookie: reddit_session=f0f0f0f0f0f0f0f0f0f0f0f0f0;`.

There does not seem to be a way to sort the content via query arguments.