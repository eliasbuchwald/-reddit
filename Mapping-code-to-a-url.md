While trying to figure out what exactly some bit of code you're looking at does, or trying to test a bugfix, it's helpful to be able to visit pages that actually *use* the code in question.

First, let's take a look at how urls get mapped to controllers, as that will make the reverse process much easier to understand.

# From a URL to the code that serves it

The first place to start is [`routing.py`](https://github.com/reddit/reddit/blob/master/r2/r2/config/routing.py).  As the name suggests, this file contains the primary *routing logic* to decide what code should be called for each request.  While Pylons has [some documentation](http://docs.pylonsproject.org/projects/pylons-webframework/en/v0.9.7/thirdparty/routes.html) on the module, it's not terribly helpful; you'll probably find [the routing section of the tutorial](http://docs.pylonsproject.org/projects/pylons-webframework/en/v0.9.7/tutorials/quickwiki_tutorial.html#routing) easier to understand.

There's some code in [`controllers/__init__.py`](https://github.com/reddit/reddit/blob/master/r2/r2/controllers/__init__.py) that handles the specifics of mapping `controller` to an actual file, but what you need to know is this: do a case-insensitive search in that file for `<controller name>Controller` and you'll find where it's imported from.

From there, you should be pretty much set - that class should be defined in a file in [the `controllers` directory](https://github.com/reddit/reddit/tree/master/r2/r2/controllers), and the `action` from `routing.py` maps to a method. Now it's on to normal code execution and tracing.

# From code to urls

The backward process is a bit more difficult, since there is no trivial way to find mappings from definitions back to callsites.  [`sgrep`](https://github.com/facebook/pfff/wiki/Sgrep), `git grep`, `grep`, and `ack` are your friends, and hopefully you'll eventually work your way back to `routing.py`.