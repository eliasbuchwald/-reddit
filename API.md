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
  one request every two seconds from you.
* Change your client's User-Agent string to something unique and descriptive,
  preferably referencing your reddit username.
    * Example: `User-Agent: super happy flair bot v1.0 by /u/spladug`
    * Many default User-Agents (like "Python/urllib" or "Java") are drastically
      limited to encourage unique and descriptive user-agent strings.
    * If you're making an application for others to use, please include a
      version number in the user agent. This allows us to block buggy versions
      without blocking all versions of your app.
    * **NEVER lie about your user-agent.** This includes spoofing popular
      browsers and spoofing other bots. We will ban liars with extreme
      prejudice.
* Most pages are cached for 30 seconds, so you won't get fresh data if you
  request the same page that often. Don't hit the same page more than once
  per 30 seconds.
* Requests for multiple resources at a time are always better than requests for
  single-resources in a loop. Talk to us on the mailing list or in #reddit-dev
  if we don't have a batch API for what you're trying to do.
* Our robots.txt is for search engines not API clients. Obey these rules for
  API clients instead.

## Glossary ##

### Thing ###

See [[JSON]] for an overview of some of the response types.

## Changes to the API ##

Changes to the API can happen without warning, though we try to document
changes in [/r/redditdev](http://www.reddit.com/r/redditdev). Use the Firefox
add-on
[LiveHeaders](https://addons.mozilla.org/en-us/firefox/addon/live-http-headers/)
or the Webkit inspector's Network section to see what fields are being posted
this week.
