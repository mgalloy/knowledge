# Python packaging 

Packaging is complex. 

* The official Python 2 [docs](https://docs.python.org/2/tutorial/modules.html#packages) that define a package.
* This tutorial -- [Packaging and Distributing Projects](https://packaging.python.org/distributing/) -- is the best. It includes a [glossary](https://packaging.python.org/glossary/) of packaging terms and a GitHub repo, [pypa/sampleproject](https://github.com/pypa/sampleproject).
* I also found this article useful: [Use setup.py to Deploy Your Python App with Style](http://www.siafoo.net/article/77)

Eric's canonical packaging example is https://github.com/csdms/standard_names.
Mine is https://github.com/csdms/dakota.

An
[example](https://docs.python.org/2/distutils/setupscript.html#installing-scripts)
that shows how to include package data.

Packages are installed to `{prefix}/lib/pythonX.Y/site-packages`;
e.g., **/usr/local/lib/python2.7/site-packages/**.


## Setuptools

Documentation for
[setuptools](https://setuptools.readthedocs.io/en/latest/index.html),
in particular,
[building and distributing packages](https://setuptools.readthedocs.io/en/latest/setuptools.html)
with setuptools.

The standard call to install a package:

	$ cd {package}
	$ [sudo] python setup.py install

If I lack permissions to install
in the distro's **site-packages** directory,
I can install into **~/.local** with

	$ python setup.py install --user

If scripts are installed,
be sure to include **~/local/bin** in `$PATH`.

Use `develop` to install in 'development mode',
which puts links to the source code
in **site-packages** instead of installing an egg:

	$ python setup.py develop
	$ python setup.py develop --uninstall

Can't use `pip` to uninstall a package installed with `develop`.

See other commands with:

	$ python setup.py --help-commands

### Dependencies

I've been a little confused about how dependencies are handled
by setuptools.
Here are two articles that helped:

* [install_requires vs Requirements files](https://packaging.python.org/requirements/)
* [setup.py vs requirements.txt](https://caremad.io/2013/07/setup-vs-requirement/)

I'm using `install_requires` now,
because I haven't yet needed to specify versions for dependencies.


## Pip

Uninstall with `pip`:

	$ pip uninstall foo


## Namespace packages

I'd like the software I develop at CSDMS to be under the `csdms` namespace.
See [csdms/dakota](https://github.com/csdms/dakota)
as an example.

**References:**

* [PEP 420](https://www.python.org/dev/peps/pep-0420/) [python.org]
* [Discussion on stackoverflow](http://stackoverflow.com/a/1676069/1563298) [stackoverflow.com]
* An informative [blog post](http://cdent.tumblr.com/post/216241761/python-namespace-packages-for-tiddlyweb) [tumblr.com]


## Upload to PyPI

The [Python Package Index](https://pypi.python.org/pypi) (PyPI)
allows public access to a package through `pip`.
I used instructions from
[this](http://peterdowns.com/posts/first-time-with-pypi.html) blog post
to publish a [package](https://pypi.python.org/pypi/basic-modeling-interface)
on PyPI.
[Here's](https://docs.python.org/2/distutils/packageindex.html)
the offical documentation.


## Virtualenv

Installing
[virtualenv](http://virtualenv.readthedocs.org/en/latest/virtualenv.html).
And an
[example](http://matthew-brett.github.io/pydagogue/installing_scripts.html)
of using it to install a package.
Virtualenv is really
[the only way](http://stackoverflow.com/questions/11170827/how-tell-python-script-to-use-particular-version)
to force a Python script to use a particular Python version.

**Note:** I've moved away from virtualenv in favor of using
disposable Miniconda distributions.
