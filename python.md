# Python

Style
------------------------------------------------------------------------------

The Python style guide is called the PEP-8, authored by Guido:

[http://www.python.org/dev/peps/pep-0008/](http://www.python.org/dev/peps/pep-0008/)

Also:

	>>> import this

Check for style errors with `pep8`:

    $ pep8 foo.py


Program types
------------------------------------------------------------------------------

Python has only functions, no procedures/subroutines. If no return is 
specified, Python returns `None`, the null value.

Definition syntax:

```python
def foo():
  print('bar!')
```

Calling syntax:

	>>> foo()
	bar!

Case sensitivity
------------------------------------------------------------------------------

Python is case-sensitive. 

	>>> a = 5
	>>> A = 6
	>>> A != a
	True

Path
------------------------------------------------------------------------------

`$PYTHONPATH` sets path.

	>>> import sys
	>>> sys.path	# prints path
	>>> sys.path.append('/path/to/stuff')	# local to session
	>>> sys.version

Importing modules
------------------------------------------------------------------------------

Importing modules:

	>>> import helloWorld as h
	>>> h.helloWorld()
	'Hello World!'
	>>> i = h.helloWorld()
	>>> i()
	'Hello World!'

or

	>>> from helloWorld import helloWorld
	>>> helloWorld()
	'Hello World!'

It's strongly encouraged to never import like this:

	>>> from <module> import *

Examples from my library (**minmax.py**):

	>>> from minmax import *
	>>> a = range(5)
	>>> minmax(a)
	[0, 4]

	>>> from minmax import minmax
	>>> a = range(5)
	>>> b = minmax(a)
	>>> b
	[0, 4]

	>>> import minmax as mm
	>>> mm.minmax(a)
	[0, 4]

Module test suite
------------------------------------------------------------------------------

Appending a main program at the bottom of an IDL routine is equivalent to 
the python construct

	if __name__ == '__main__':
		print helloWorld()

This is called the module test suite. It's a unit test. I use it as an example
program in IDL. Call this main program from the shell prompt with:

	$ python helloWorld.py

or from within IPython with:

	In [1]: run helloWorld.py

Arguments
------------------------------------------------------------------------------

Arguments in Python behave, by default, like positional parameters in IDL,
but can also be called like IDL keyword parameters.

Keyword parameters in Python supply default values to a function. They
can be overriden in the call to the function. Their order doesn't
matter.

For an example, see **examples/ClassWithKeyword.py**.

The dir() function
------------------------------------------------------------------------------

The `dir()` function lists everything about something.

	>>> import helloWorld as h 
	>>> dir(h)
	['__builtins__', '__doc__', '__file__', '__name__', 'helloWorld']

The callable() function
------------------------------------------------------------------------------

Is an object callable?

	>>> import helloWorld as h 
	>>> callable(h)
	False
	>>> callable(h.helloWorld)
	True

Doc strings
------------------------------------------------------------------------------

Get a doc string:

	>>> print pow.__doc__

The 'os' module
------------------------------------------------------------------------------

The 'os' module:

	>>> os.path.exists('/Users/Mark/README')
	True
	>>> os.path.sep
	'/'
	>>> os.getcwd()
	'/Users/Mark/code/python'
	>>> os.listdir(os.getcwd)) # same as my PWD program in IDL

Lists
------------------------------------------------------------------------------

Note that "x" is a list, not an array (which needs Numpy):

	In [40]: x = range(5)
	In [41]: x
	Out[41]: [0, 1, 2, 3, 4]
	In [42]: dir(x);    # remove ";" to see output, like MATLAB
	In [43]: x.__class__
	Out[43]: list
	In [44]: x.append(55)
	In [45]: x
	Out[45]: [0, 1, 2, 3, 4, 55]

Note the difference between `range` (list) and `arange` (array):

	In [58]: x = range(5)
	In [59]: y = np.arange(0,5)
	In [60]: x
	Out[60]: [0, 1, 2, 3, 4]
	In [61]: y
	Out[61]: array([0, 1, 2, 3, 4])
	In [62]: x*2
	Out[62]: [0, 1, 2, 3, 4, 0, 1, 2, 3, 4]
	In [63]: y*2
	Out[63]: array([0, 2, 4, 6, 8])

And `xrange` is optimized to be faster than `range`:

	In [10]: %timeit list1 = range(1000000)
	100 loops, best of 3: 15 ms per loop

    In [11]: %timeit list2 = xrange(1000000)
	10000000 loops, best of 3: 151 ns per loop

Get the length of a list:

	>>> len(x)
	5

Remove an element from a list:

	>>> x.pop(0)
	>>> x
	[1, 2, 3, 4]

Append versus extend:

	In [24]: x1 = range(5)
	In [25]: x2 = range(5)
	In [26]: y = range(5,10)
	In [27]: x1.append(y)
	In [28]: x1
	Out[28]: [0, 1, 2, 3, 4, [5, 6, 7, 8, 9]]
	In [29]: x2.extend(y)
	In [30]: x2
	Out[30]: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

### List comprehesion:

It's like an implied loop in Fortran.

	>>> x = range(5)
	>>> y = [i**2 for i in x]
	>>> x, y
	([0, 1, 2, 3, 4], [0, 1, 4, 9, 16])

### List filtering

List filtering is equivalent to running WHERE output through conditioned
array.

Tuples
------------------------------------------------------------------------------

Lists use [], tuples use ().

	>>> a = [1,2,3]
	>>> b = (1,2,3)
	>>> type(a)
	list
	>>> type(b)
	tuple

Tuples are immutable.

Lambda functions
------------------------------------------------------------------------------

Lambda functions:

	>>> g = lambda x: x**2

is equivalent to

	def g(x):
		return x**2

Typing
------------------------------------------------------------------------------

Python is dynamically and strongly (needs explicit type conversion) typed.
IDL is dynamically and weakly typed.

String formatting
------------------------------------------------------------------------------

String formatting in Python is the same as `sprintf` in C. The `+`
operator does concatenation, while `str()` does type coercion.

And it can be crazy. Check this out:

	>>> first_name = "Mark"
	>>> last_name = "Piper"
	>>> title = "Grand Poobah"
	>>> s = "Hi, my name is {0} {1}, {2}.".format(first_name, last_name, title)
	>>> s
	'Hi, my name is Mark Piper, Grand Poobah.'

Garbage collection
------------------------------------------------------------------------------

Python uses reference counting on objects, like IDL 8.0.

OOP
------------------------------------------------------------------------------

Python uses class and instance variables.

Array operations
------------------------------------------------------------------------------

Need numpy or scipy to do array operations, eh?

	>>> i = range(10)
	>>> j = i**2 # raises exception???

Arrays are not intrinsic in Python.

Slicing
------------------------------------------------------------------------------

Subscripting ("slicing") arrays is whack. Done on interval [start,end).

	In [34]: x = range(10)
	In [35]: x[0:4]
	Out[35]: [0, 1, 2, 3]

Negative indexing works like IDL, with -1 the last element in an
array.

Numpy doesn't copy
------------------------------------------------------------------------------

Arrays aren't copied. (Assignment creates a pointer to the original
array.)

	>>> x = np.arange(0,5)
	>>> y = x
	>>> y[-1] = 42.0
	>>> print x
	[ 0  1  2  3 42]

List to an array
------------------------------------------------------------------------------

Convert a list to an array:

	>>> x = range(5)
	>>> y = np.asarray(x)
	>>> x.__class__
	list
	>>> y.__class__
	numpy.ndarray

Install directory
------------------------------------------------------------------------------

Install directory: **/usr/lib/python2.7/**

Integers
------------------------------------------------------------------------------

Python supports integer division.

Numpy version
------------------------------------------------------------------------------

	>>> import numpy
	>>> print numpy.__version__
	1.6.1

Reading from a text file
------------------------------------------------------------------------------

Text is read into a list.

	with open("./config/dependencies.txt", "r") as f:
		dependencies = f.read().split("\n")

The subprocess module
------------------------------------------------------------------------------

Replaces `os.system` for calling shell commands.

	In [33]: b = ["foo", "bar", "baz"]
	In [34]: for i in b:
		subprocess.call(["echo", i])
		....:
	foo
	bar
	baz

Alternately, using a single string:

	In [36]: for i in b:
		subprocess.call("echo " + i, shell=True)
		....:
	foo
	bar
	baz

See the [docs](https://docs.python.org/2/library/subprocess.html#replacing-os-system).


NetCDF
------------------------------------------------------------------------------

For netCDF3 files:

	>>> from scipy.io import netcdf

See examples: http://www-pord.ucsd.edu/~cjiang/python.html

Magic functions
------------------------------------------------------------------------------

IPython has "magic" functions prefaced with the "%" character; e.g.,

	%whos
	%cd
	%pwd

Can also use shell commands; e.g.,

	ls
	mkdir

though some may need to be escaped:

	!date

History
------------------------------------------------------------------------------

Use `Ctrl-P` to search through history to match a command. 

`Ctrl-N` does a reverse search.

Works in Python and IPython.

Movies!
------------------------------------------------------------------------------

Make a movie from a bunch of PNG files with:

	>>> os.system('avconv -r 2.5 -i gph.%03d.png -vcodec libx264 gph.mp4')

However, is there a Pythonic solution?

Yes, though not with my version of matplotlib (1.1.1rc). 
See: http://stackoverflow.com/questions/4092927/generating-movie-from-python-without-saving-individual-frames-to-files


Some IDL equivalents
------------------------------------------------------------------------------

Or near-equivalents.

<table>

<tr>
<td><b>Python</b></td>
<td><b>IDL</b></td>
</tr>

<tr>
<td>dir()</td>
<td>HELP</td>
</tr>

<tr>
<td>help()</td>
<td>ONLINE_HELP</td>
</tr>

<tr>
<td>pickle()</td>
<td>SAVE</td>
</tr>

<tr>
<td>range()</td>
<td>*INDGEN</td>
</tr>

<tr>
<td>arange()</td>
<td>*INDGEN (numpy)</td>
</tr>

<tr>
<td>linspace()</td>
<td>*INDGEN(n)*scale + offset</td>
</tr>

<tr>
<td>f.seek()</td>
<td>SKIP_LUN, POINT_LUN</td>
</tr>

<tr>
<td>f.tell()</td>
<td>POINT_LUN</td>
</tr>

<tr>
<td>len()</td>
<td>N_ELEMENTS (only on first dim of array)</td>
</tr>

<tr>
<td>x.size()</td>
<td>N_ELEMENTS</td>
</tr>

<tr>
<td>x.shape()</td>
<td>SIZE</td>
</tr>

<tr>
<td>x.reshape()</td>
<td>REFORM</td>
</tr>

<tr>
<td>None</td>
<td>!NULL</td>
</tr>

<tr>
<td>os.chdir()</td>
<td>CD</td>
</tr>

<tr>
<td>os.getcwd()</td>
<td>CD</td>
</tr>

<tr>
<td>&gt;</td>
<td>GE</td>
</tr>

<tr>
<td>&lt;</td>
<td>LE</td>
</tr>

<tr>
<td>eval()</td>
<td>EXECUTE</td>
</tr>

<tr>
<td>exec()</td>
<td>EXECUTE</td>
</tr>

<tr>
<td>sys.exit()</td>
<td>EXIT</td>
</tr>

<tr>
<td>a = x.copy()</td>
<td>a = x</td>
</tr>

<tr>
<td>plt.plot()</td>
<td>PLOT()</td>
</tr>

<tr>
<td>plt.figure()</td>
<td>WINDOW()</td>
</tr>

</table>
