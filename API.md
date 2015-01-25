## Resources
* reddit's built-in [live API documentation](http://www.reddit.com/dev/api)
* The [Apigee API console](https://apigee.com/console/reddit)
* reddit supports [[OAuth for authentication|OAuth2]].
* Don't forget the list of [[API Wrappers]].

<h2 id="rules">Rules</h2>
We're happy to have API clients, crawlers, scrapers, and Greasemonkey scripts,
but they have to obey some rules:

* Please ensure that all API clients are following reddit's [licensing guidelines](http://www.reddit.com/wiki/licensing)
* **Make no more than thirty requests per minute.** This allows some burstiness
  to your requests, but keep it sane. On average, we should see no more than
  one request every two seconds from you. Monitor the following response headers
  to ensure that you're not exceeding the limits:
    * `X-Ratelimit-Used`: Approximate number of requests used in this period
    * `X-Ratelimit-Remaining`: Approximate number of requests left to use
    * `X-Ratelimit-Reset`: Approximate number of seconds to end of period
* Clients connecting via [[OAuth2]] may make up to 60 requests per minute.
* Change your client's User-Agent string to something unique and descriptive,
  including the target platform, a unique application identifier, a version string,
  and your username as contact information, in the following format:  
  `<platform>:<app ID>:<version string> (by /u/<reddit username>)`  
    * Example: `User-Agent: android:com.example.myredditapp:v1.2.3 (by /u/kemitche)`
    * Many default User-Agents (like "Python/urllib" or "Java") are drastically
      limited to encourage unique and descriptive user-agent strings.
    * Including the version number and updating it as your build your application allows us
      to safely block old buggy/broken versions of your app.
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

## Changes to the API #swipty.org#swipty.net

Changes to the API can happen without warning if necessary, subscribe to [/r/redditdev](http://www.reddit.com/r/redditdev) for announcements of changes.
