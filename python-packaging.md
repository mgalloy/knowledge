# Notes on Python packaging 

Packaging is complex. 

Eric recommends this tutorial:

* [Installation & Packaging Tutorial](https://python-packaging-user-guide.readthedocs.org/en/latest/tutorial.html) [readthedocs.org]. It includes a [glossary](https://python-packaging-user-guide.readthedocs.org/en/latest/glossary.html) of packaging terms and a GitHub repo, [pypa/sampleproject](https://github.com/pypa/sampleproject).

Another article:

* [Use setup.py to Deploy Your Python App with Style](http://www.siafoo.net/article/77)

Eric has constructed the [csdms/coupling](https://github.com/csdms/coupling)
repo as a package,
and I'm working on
[csdms/packagebuilder](https://github.com/csdms/packagebuilder).
I think I did a good job with
[csdms/dakota](https://github.com/csdms/dakota);
maybe it can be my canonical reference.

The standard call:

	$ cd {package}
	$ [sudo] python setup.py install

See other commands with:

	$ python setup.py --help-commands

Installing
[virtualenv](http://virtualenv.readthedocs.org/en/latest/virtualenv.html).
And an
[example](http://matthew-brett.github.io/pydagogue/installing_scripts.html)
of using it to install a package.
Virtualenv is really
[the only way](http://stackoverflow.com/questions/11170827/how-tell-python-script-to-use-particular-version)
to force a Python script to use a particular Python version.

An
[example](https://docs.python.org/2/distutils/setupscript.html#installing-scripts)
that shows how to include package data.

Packages are installed to `{prefix}/lib/pythonX.Y/site-packages`;
e.g., **/usr/local/lib/python2.7/site-packages/**.

Documentation for
[setuptools](https://pythonhosted.org/setuptools/setuptools.html).

## More commands

Uninstall with `pip`:

	$ pip uninstall foo

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


## Namespace packages

I'd like the software I develop at CSDMS to be under the `csdms` namespace.

**References:**

* [PEP 420](https://www.python.org/dev/peps/pep-0420/) [python.org]
* [Discussion on stackoverflow](http://stackoverflow.com/a/1676069/1563298) [stackoverflow.com]
* An informative [blog post](http://cdent.tumblr.com/post/216241761/python-namespace-packages-for-tiddlyweb) [tumblr.com]
