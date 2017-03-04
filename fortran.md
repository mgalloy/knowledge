# Fortran

These notes are mainly for Fortran 90/95, and Fortran 2003 where specified.
Fortran 77 is ingrained.


## References

* [Compact Fortran 95 Language Summary](https://www.csee.umbc.edu/~squire/fortranclass/summary.shtml) [umbc.edu].
* [Fortran Best Practices](http://www.fortran90.org/src/best-practices.html) [fortran90.org]. This is good. Despite the domain name, it's not an official web site.
* http://www.cs.rpi.edu/~szymansk/OOF90/F90_Objects.html
* https://github.com/Unidata/netcdf-fortran. See how the Unidata engineers build netCDF-Fortran. Or maybe not -- they use includes more than modules.


## Parlance

* Data hiding is done through *private* (and *public*) statements/attributes.
* Encapsulate with a *type*.
* Function overloading is accomplished through a *generic interface*.
* Import from modules with *use*. Explicitly import with *use*, *only*.
 * On import, rename an item with `=>`.
* The pointer assignment operator is also `=>`. For example, `p => target`.
 * The pointer variable must have the `pointer` attribute.
 * The target variable must have the `target` attribute.
* The equivalent of a C header file is a module.


## Variables

* The maximum length of a variable name is 31 characters.
* Use lower case for variable names, connected with underscores.


## Interfaces

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


## Type-bound procedures

Type-bound procedures are defined in Fortran 2003.
See http://fortranwiki.org/fortran/show/Object-oriented+programming.
