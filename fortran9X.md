# Fortran 90/95

## References

* [Compact Fortran 95 Language Summary](https://www.csee.umbc.edu/~squire/fortranclass/summary.shtml) [umbc.edu].
* [Fortran Best Practices](http://www.fortran90.org/src/best-practices.html) [fortran90.org]. This is good.
* http://www.cs.rpi.edu/~szymansk/OOF90/F90_Objects.html

## Parlance

* Data hiding is done through *private* (and *public*) statements/attributes.
* Encapsulate with a *type*.
* Function overloading is accomplished through a *generic interface*.
* Import from modules with *use*. Explicitly import with *use*, *only*.
 * On import, rename an item with `=>`.
* The pointer assignment operator is also `=>`. For example, `p => target`.
 * The pointer variable must have the `pointer` attribute.
 * The target variable must have the `target` attribute.

## Style

* Use lower case for variable names, connected with underscores.

## Notes

An *interface* in F90 is defined (for example) in a main program
to provide an explicit interface to a subprogram,
thereby allowing keyword and optional arguments.
If the subprogram is located in a module,
then an interface is not needed.
It's preferrable to use modules, when possible.
An interface would be used, for example,
to wrap a F77 subroutine,
rather than rewriting the routine to use an F90 module.

So, a F90 interface isn't like what I think of
an an interface, like in Java, for example.
