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

For the most part, you should use the general [`unittest.TestCase`](https://docs.python.org/2/library/unittest.html#unittest.TestCase).  If you're getting errors about the environment, check out the helpers in [`tests/__init__.py`](https://github.com/reddit/reddit/blob/master/r2/r2/tests/__init__.py).