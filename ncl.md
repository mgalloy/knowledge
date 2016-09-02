# NCL

NCL is the NCAR Command Language.


## Documentation

* The NCL [home page](https://www.ncl.ucar.edu/index.shtml) [ucar.edu]
* [A list of functions by category](https://www.ncl.ucar.edu/Document/Functions/index.shtml) [ucar.edu]


## Strings

String need double, not single, quotes:

    a = 'Foo'  ; fails
	a = "Foo"  ; works


## Print function

The NCL `print` function accepts only one argument

    print("Yo.")

However,
if the argument is a variable with metadata,
it'll print this info, as well,
like the IDL `HELP` procedure.

Minimize `print` output by using `printVarSummary` instead.


## Variable reassignment

Variables can only be reassigned if they have the same shape
and a coercible type.
For example,
assigning an integer array to a scalar string will fail

    x = "Hi there"
	x = (/1,2,3/)  ; fails

However, a variable can be clobbered with the reassignment operator `:=`

    x = "Hi there"
	x := (/1,2,3/) ; works


## Type and type conversion

Use `typeof` to determine the type of a variable.

    y = 5.0
	print("The type of y is " + typeof(y))

There are a set of "XtoY" functions
(although they are inconsistently named)
for converting a variable
from one type to another.
For example,
to convert `y` from float to integer, use

    y := floattointeger(y)
	print("The type of y is " + typeof(y))

Be sure to use `:=` to reassign the variable `y`.
