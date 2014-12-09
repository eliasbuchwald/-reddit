reddit's templating system is a bit less direct and explicit than some other systems.  Here's a very high-level overview:

At some point in time, a class is constructed and `.render()` called on it.  There are many of these classes, but what you'll find in common between them is that they inherit, directly or indirectly, from [`Templated`](https://github.com/reddit/reddit/blob/master/r2/r2/lib/wrapped.pyx#L129-L138).  Here's an example:

    class PasswordChangeEmail(Templated):
        """Notification e-mail that a user's password has changed."""
        pass

You'll note there is absolutely no body to the class; how then does it change functionality at all?  The answer lies in some meta-magic in `Templated`'s constructor, which stores `self.__class__` as the *render class*.

Eventually we end up in `tp_manager`, and there we find that it uses [the template name and style to identify the appropriate template file](https://github.com/reddit/reddit/blob/master/r2/r2/lib/manager/tp_manager.py#L69).  The name is the aforementioned render class's name, lowercased; the style, if not passed in to `.render()`, defaults to `html`.

So then, when you see a line like this:

    PasswordChangeEmail(user=user).render(style='email')

you should look for [`templates/passwordchangeemail.email`](https://github.com/reddit/reddit/blob/master/r2/r2/templates/passwordchangeemail.email).  Simple enough once you know!

One more thing: since it's common to want to render the same data a number of ways (desktop-oriented HTML, mobile web, JSON, etc.), there's [a mapping of extensions](https://github.com/reddit/reddit/blob/master/r2/r2/config/extensions.py#L35-L50) that force rendering to happen with a particular style.  So if you tack on `.compact` to a url, it will render with `whatever.compact` instead of `whatever.html`, without you having to write code to switch based on that.