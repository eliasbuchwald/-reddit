reddit's email system lives primarily in two places, [a lib](https://github.com/reddit/reddit/blob/master/r2/r2/lib/emailer.py) and [a model](https://github.com/reddit/reddit/blob/master/r2/r2/models/mail_queue.py), although as an end-user you'll probably only need the library.

The entry-point for emails is `emailer.py`'s `_system_email()`.  However, you'll generally want to wrap this in a little bit of logic specific to your particular email use-case.  This is what `verify_email()` does, for instance, for email-verification emails.

`_system_email()` chucks your carefully-crafted email into a queue to be processed later.  This allows fast web responses back to the user, batching, throttling, and some other cool stuff, but makes life a bit more difficult in development.  There's [[an email cronjob|Cron-jobs#email]] you'll need to kick off manually when you want to process queued emails:

    [$]> sudo start reddit-job-email

If you have `level = DEBUG` in the `[logger_reddit]` section of your `.ini` file, you should see output from the job in `/var/log/upstart/reddit-job-email.log`.

Oh!  One more thing: if you don't have an SMTP server configured in your development environment (that'd be a bad way to accidentally send out a bunch of emails), you'll need to launch `send_queued_mail()` with `test=True`.  `/etc/init/reddit-job-email.conf` is a copy of [the upstart file from the repo](https://github.com/reddit/reddit/tree/master/upstart); you need to just edit it and pass in `True` to the function.  That will bypass trying to send any emails and print them out to the log file instead.

In summary:

Setup: write code to call `_system_email()`, modify `reddit-job-email.conf`.  
Testing: start `reddit-job-email`, check `reddit-job-email.log`.