# Fortran 90/95

Parlance:

* Data hiding is done through *private* (and *public*) statements/attributes.
* Encapsulate with a *type*.
* Function overloading is accomplished through a *generic interface*.
* Import from modules with *use*. Explicitly import with *use*, *only*.
 * On import, rename an item with `=>`.
* The pointer assignment operator is also `=>`. For example, `p => target`.
 * The pointer variable must have the `pointer` attribute.
 * The target variable must have the `target` attribute.

Style:

* Use lower case for variable names, connected with underscores.

See:

* [Compact Fortran 95 Language Summary](https://www.csee.umbc.edu/~squire/fortranclass/summary.shtml) [umbc.edu].
* [Fortran Best Practices](http://www.fortran90.org/src/best-practices.html) [fortran90.org]. This is good.
* http://www.cs.rpi.edu/~szymansk/OOF90/F90_Objects.html
