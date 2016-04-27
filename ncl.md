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
