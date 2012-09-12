
## Ordering of `import` statements

In general [PEP-8 import ordering](http://www.python.org/dev/peps/pep-0008/#imports) should be used.  However there are a few additions.

* Imports should be grouped in the following order:
   1. standard library imports
   2. related third party imports
   3. Reddit packages not part of the local package scope
   4. Reddit packages part of the the local package scope or sub scopes of the current package.
* No relative imports:  All imports should have fully qualified package names.
* for each imported group the order of imports should be:
   1. `import <package>.<module>` style lines in alphabetical order
   2. `from <package>.<module> import <symbol>` style in alphabetical order

## Importing multiple symbols `from <module> import <multiple>`
* Only one `from` line per module is permitted
* If there are too many symbols for a single line of < 80 characters then the import should be indented.

It should look as follows:

    from foo import (#Classes
                     Foo,
                     Bar,
                     Baz,
                     #Functions
                     lookup_foo,
                     lookup_all,
                     #Constants/Variables
                     ALL_USERS,
                     OTHER_THING,
                     )



* This should only be used if there are 10 or fewer symbols to import.  Otherwise consider using an `import <module> as <alias>` and prefixing all symbols from that module with the appropriate alias.

## Symbol exporting, `__all__` and @export
* Each file should have an `__all__` declaration

It should look like:

    __all__ = [
               #Constants Only, use @export for functions/classes
               ]

* Any functions or classes the need to be exported should use the @export decorator found in r2.lib.export
