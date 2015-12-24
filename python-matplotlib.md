# Matplotlib

NumPy is a dependency for Matplotlib.
Matplotlib isn't supported on Python 3.

## Example 1

A simple Matplotlib example (using IPython):

	In [1]: import matplotlib.pyplot as plt
	In [2]: x = range(5)
	In [3]: plt.plot(x, [xi**2 for xi in x]) # using list comprehension
	In [4]: plt.show(block=False)  	     	 # experimental block keyword

To overplot, call `plt.plot` multiple times before calling `plt.show`.

## Example 2

Numpy is better:

	In [27]: import numpy as np
	In [28]: x = np.arange(0, 5, 0.5) # start, stop, interval
	In [29]: plt.plot(x, x*2)
	In [30]: plt.plot(x, x*3)
	In [31]: plt.plot(x, x*4)
	In [32]: plt.show()

## Blocking figure window

Toggle figure window blocking the command line with:

	In [36]: plt.ion()
	In [37]: plt.ioff()

Or start with:

	$ ipython - pylab

## Pylab

Should I skip pyplot and go right to pylab, which combines pyplot and
numpy? Maybe not:

> "We and the authors of Matplotlib discourage the use of pylab,
> other than for proof-of-concept snippets. While being rather
> simple to use, it teaches developers the wrong way to use
> Matplotlib, so we intentionally do not present it in this book."

## Initial imports

In the preferred style, initial imports are:

	>>> import matplotlib.pyplot as plt
	>>> import numpy as np
	>>> from mpl_toolkits.basemap import Basemap
