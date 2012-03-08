Use a GET to `http://www.reddit.com/domain/DOMAIN/` where `DOMAIN` is the domain to filter for.  You can append `.json` or `.xml` to the path in order to retrieve a JSON or XML object instead.

## Response

When retrieving a JSON object, the top level object will be a `thing` with `Listing` as its kind.  The `children` will be a ListNode of `things` of kind link (t3).