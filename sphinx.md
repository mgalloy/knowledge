# Sphinx

[Sphinx](http://www.sphinx-doc.org/en/stable/)
is a documentation tool for Python.

Here are the steps I took to create docs for the
[bmi-forum/bmi-python](https://github.com/bmi-forum/bmi-python)
project.

1. Install (or link with develop) the package.

        python setup.py develop
		
1. From the root directory of the repo, run `sphinx-apidoc`.

        sphinx-apidoc -F -e -M -o docs basic_modeling_interface/

   The `-F` flag specifies that `sphinx-apidoc` should generate a full project
   with `sphinx-quickstart`.
   The `-e` flag separates the docs from each module into separate files.
   The `-M` flag lists modules before submodules.
   The target directory, **docs**, is created by the `-o` flag.
   The source directory is the package directory, **basic_modeling_interface**.

1. Next, optionally, make a **docs/source** directory and move everything
   except **Makefile** and **_build/** in it. This helps with `numpydoc`, next.
   
1. Install `numpydoc` as a submodule into the **docs** directory.

		git submodule add https://github.com/numpy/numpydoc
		
   This clones `numpydoc` into **docs** and adds it to **.gitmodules**.

1. Edit **source/conf.py** to use `numpydoc`. Also update project name,
   author, and version information.

1. Optionally, edit the doc files, starting with **source/index.rst**.
   This can be time-consuming.

1. Make the documentation.

		make html
		
   Output will be in **_build/html/index.html**.

Check out the results at http://bmi-python.readthedocs.io.


**Addendum:**
At a later point, when cloning the main repository (in this example,
[bmi-forum/bmi-python](https://github.com/bmi-forum/bmi-python)),
manually fetch the contents of the `numpydoc` submodule with

    git submodule update --init --recursive
