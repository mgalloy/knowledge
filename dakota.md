# Dakota

[Dakota](http://dakota.sandia.gov/about.html)
(Design Analysis Kit for Optimization and Terascale Applications)
is a software toolkit for iterative systems analysis of computational models
on high-performance computers.

Dakota contains algorithms for:

* optimization with gradient and nongradient-based methods;
* uncertainty quantification (UQ) with sampling, reliability, stochastic
expansion, and epistemic methods;
* parameter estimation with nonlinear least squares methods; and
* sensitivity analysis (SA) with design of experiments and parameter study methods.


## Information:

* The current version is [6.2](https://dakota.sandia.gov/download.html) (05/29/15)
* [Installation instructions](https://dakota.sandia.gov/install.html)
* [Documentation](https://dakota.sandia.gov/content/manuals),
  consisting of four manuals: user (300+ pp), theory, reference and developer.
  The Developer's Manual is available in HTML,
  which is convenient for citing URLs for methods.
* [Quick start guide](https://dakota.sandia.gov/quickstart.html) with an example
* [FAQ](https://dakota.sandia.gov/faq.html)

Dakota 6.0 is built on RHEL 6.3.
It should run on other Linux distributions that maintain binary
compatibility with RHEL, such as CentOS.

Installed in `/usr/local` on ***solaria***, ***anacreon***
and ***beach***.


## Citing Dakota:

See [https://dakota.sandia.gov/content/citing-dakota](https://dakota.sandia.gov/content/citing-dakota).


## Issue with Python matplotlib on Mac OS X

Dakota sets `DYLD_LIBRARY_PATH` on Mac OS X.
This apparently conflicts with `matplotlib` on Mac OS X
-- I can't use `pyplot` or `pylab`.

As a workaround,
I can unset `DYLD_LIBRARY_PATH` in the shell:

    $ unset DYLD_LIBRARY_PATH

Dakota won't work, but `matplotlib` will.

This issue is also referenced, randomly, [here](https://github.com/deep-introspection/Udgan/issues/2).
