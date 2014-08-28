## Resources
* reddit's built-in [live API documentation](http://www.reddit.com/dev/api)
* The [Apigee API console](https://apigee.com/console/reddit)
* reddit supports [[OAuth for authentication|OAuth2]].
* Don't forget the list of [[API Wrappers]].

<h2 id="rules">Rules</h2>
We're happy to have API clients, crawlers, scrapers, and Greasemonkey scripts,
but they have to obey some rules:

* **Make no more than thirty requests per minute.** This allows some burstiness
  to your requests, but keep it sane. On average, we should see no more than
  one request every two seconds from you. Monitor the following response headers
  to ensure that you're not exceeding the limits:
    * `X-Ratelimit-Used`: Approximate number of requests used in this period
    * `X-Ratelimit-Remaining`: Approximate number of requests left to use
    * `X-Ratelimit-Reset`: Approximate number of seconds to end of period
* Clients connecting via [[OAuth2]] may make up to 60 requests per minute.
* Change your client's User-Agent string to something unique and descriptive,
  preferably referencing your reddit username.
    * Example: `User-Agent: flairbot/1.0 by spladug`
    * Many default User-Agents (like "Python/urllib" or "Java") are drastically
      limited to encourage unique and descriptive user-agent strings.
    * If you're making an application for others to use, please include a
      version number in the user agent. This allows us to block buggy versions
      without blocking all versions of your app.
    * **NEVER lie about your user-agent.** This includes spoofing popular
      browsers and spoofing other bots. We will ban liars with extreme
      prejudice.
* Requests for multiple resources at a time are always better than requests for
  single-resources in a loop. Talk to us on the mailing list or in #reddit-dev
  if we don't have a batch API for what you're trying to do.
* Our robots.txt is for search engines, not API clients. Obey these rules for
  API clients instead.

## Glossary ##

### Thing ###

See [[JSON]] for an overview of some of the response types.

## Changes to the API ##

Changes to the API can happen without warning if necessary, subscribe to [/r/redditdev](http://www.reddit.com/r/redditdev) for announcements of changes.
