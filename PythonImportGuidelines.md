## Ordering of `import` statements

In general [PEP-8 import ordering](http://www.python.org/dev/peps/pep-0008/#imports) should be used.  However there are a few additions.

* Imports should be grouped in the following order:
   1. standard library imports
   2. related third party imports
   3. reddit packages not part of the local package scope
   4. reddit packages part of the the local package scope or sub scopes of the current package.
* No relative imports:  All imports should have fully qualified package names.
* for each imported group the order of imports should be:
   1. `import <package>.<module>` style lines in alphabetical order
   2. `from <package>.<module> import <symbol>` style in alphabetical order

## Importing multiple symbols `from <module> import <multiple>`
* Only one `from` line per module is permitted
* If there are too many symbols for a single line of < 80 characters then the import should be indented as shown below.
* Indented imports should be broken into 3 groups of symbol types Classes, functions, Constants/Variables.  Each grouping should have a comment header and the items in the group be in alphabetical order.  See below.

It should look as follows:

    from foo import (#Classes
                     Bar,
                     Baz,
                     Foo,
                     #Functions
                     lookup_all,
                     lookup_foo,
                     #Constants/Variables
                     ALL_USERS,
                     OTHER_THING,
                     )



* This should only be used if there are 10 or fewer symbols to import.  Otherwise consider using an `import <module> as <alias>` and prefixing all symbols from that module with the appropriate alias.

## Symbol exporting, `__all__` and @export
* Each file should have an `__all__` declaration for constants and variables that are needed outside of the current module. 

It should look like:

    __all__ = [
               #Constants Only, use @export for functions/classes
               ]

* Any functions or classes the need to be exported should use the @export decorator found in r2.lib.export