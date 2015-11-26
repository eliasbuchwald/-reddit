While there's always manual testing involved in programming, automated tests can make your life a lot easier by helping to determine if you broke anything you haven't thought about.  Plus, who likes testing the same thing over and over again?

# Running Tests

First off, you'll need a `test.ini`.  The easiest way to go about creating one of these is to reuse the file you're using for the server:

    [$]> cd /path/to/r2
    [$]> ln -s development.ini test.ini

Now you can run `nosetests` to run the tests:

    [$]> nosetests                             # Run all available tests in r2/tests.
    [$]> nosetests -w unit/lib                 # Run only the unit tests.
    [$]> nosetests --tests unit.lib.utils_test # Run only the utils unit tests.

You may find yourself running into a Postgres connection limit error, eg `remaining connection slots are reserved for non-replication superuser connections`.  An easy way to deal with this is to stop the server, run your tests, then start the server once again when you want to view the website:

    [$]> sudo initctl emit reddit-stop
    [$]> nosetests
    [$]> sudo initctl emit reddit-start

See also [the Pylons](http://docs.pylonsproject.org/projects/pylons-webframework/en/v0.9.7/testing.html) and [nose](http://nose.readthedocs.org/en/latest/) docs.

# Writing Tests

For the most part, you should use the general [`unittest.TestCase`](https://docs.python.org/2/library/unittest.html#unittest.TestCase).  This will allow nose to figure out which things are tests that need to be run.  Additionally, it provides you with [a large set of assertion helpers](https://docs.python.org/2/library/unittest.html#unittest.TestCase.assertEqual); it's always preferable to use `self.assertLess(a, b)` over `self.assertTrue(a < b)`, as the former produces more useful output when the test fails.

> If you're getting errors about the environment, make sure your test file has `from r2.tests import RedditTestCase` before you try to import any models.

## Mocking Behavior

Sometimes things are difficult to test without running into external systems.  The common example in reddit's codebase is testing a function that does a database lookup; since the database state isn't controlled by the unit test framework, the behavior can change from machine to machine and even between runs.  A naive approach would be to require all developers to maintain a certain set of data on their machines, but that's labor-intensive and prone to error.

As an alternative approach, we can use [the `mock` library](https://docs.python.org/dev/library/unittest.mock.html) to bypass certain behavior and provide a static result.  It's good to be cautious when using mocking, as it means you're no longer testing the behavior that you've replaced with the mock, but as long as you're aware of the limitations, it's a powerful tool that allows testing of seemingly-untestable code.

The first tool from `mock` you're likely to use is [`MagicMock`](https://docs.python.org/dev/library/unittest.mock.html#unittest.mock.MagicMock).  `MagicMock` is a pretty simple class: any attribute you access on it will return another `MagicMock` (which is truthy), unless you've specified otherwise.  Need to pretend you have an `Account` object and aren't interested in testing any of its behavior?  `MagicMock` will do the job.  There *is* actually a bit more that it can do other than that, but you'll have to read through [the docs](https://docs.python.org/dev/library/unittest.mock.html#unittest.mock.Mock) for that if you need it.

The second incredibly helpful tool from `mock` is [`patch`](https://docs.python.org/dev/library/unittest.mock.html#unittest.mock.patch), which solves the problem of "I need to mock out something, but the code that references that something is in a non-test library!".  With `patch`, anything you can reference by name can be replaced with a `MagicMock`.  Figuring out how this works, particularly [which name to use](https://docs.python.org/dev/library/unittest.mock.html#where-to-patch), can lead to tears of frustration, so an example is perhaps in order:

    ## library.py

    from r2.models import Account

    def prettify_user(username):
        """Produce a pretty version of a username."""
        user = Account._by_name(username)
        if user.pref_no_pretties:
            return username

        return '~*~%s~*~' % username



    ## test_library.py

    import unittest

    from mock import patch

    from library import prettify_user

    class TestLibrary(unittest.TestCase):
        # We have to patch the version of `Account` that's been imported into `library`,
        # so we specify *that* instead of 'r2.models.Account' (where it's defined).
        @patch('library.Account')
        def test_prettify_user(self, Account):
            # Inside `library`, Account is now a `MagicMock`; thus, we can modify what
            # its functions return.
            Account._by_name('bob').pref_no_pretties = True
            self.assertEqual(prettify_user('bob'), 'bob')

            Account._by_name('bob').pref_no_pretties = False
            self.assertEqual(prettify_user('bob'), '~*~bob~*~')