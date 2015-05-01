Sometimes it feels like web development is just creating pages of forms that change data for other pages.  Fortunately, there are a number of tools to help reduce the drudgery.

# Form Creation

# Form Submission

[`validator.py`](https://github.com/reddit/reddit/blob/master/r2/r2/lib/validator/validator.py) contains a number of classes for form validation - `VLang`, `VLength`, etc.  These are passed into the `validate()` function (also in `validator.py`) as it decorates a controller:

```python
@validate(
    url=VUrl('url'),
    count=VLimit('limit'),
    things=VByName('id', multiple=True, limit=100),
)
def GET_url_info(self, url, count, things):
```

You can see plenty of examples of this in [`api.py`](https://github.com/reddit/reddit/blob/master/r2/r2/controllers/api.py).

Many of these validators use the errors defined in [`errors.py`](https://github.com/reddit/reddit/blob/master/r2/r2/lib/errors.py).  The [`set_error()` method](https://github.com/reddit/reddit/blob/master/r2/r2/lib/validator/validator.py#L93) stores errors in a global location.

Now, `validate.py` also provides a `validatedForm()` decorator.  This function checks to see if any errors are found in that global error store, and if so, alters the response appropriately so we get error messages.