# Conda

We use `conda` at CSDMS for all our Python needs.

## Install, uninstall, and list packages

    conda install ipython
    conda uninstall ipython
    conda list

## Search for packages to install

Fragments of package names can be used.

    conda search cca

## Conda packaging

(Perhaps this should be a separate note?)

See http://conda.pydata.org/docs/building/recipe.html,
and, from there,
http://conda.pydata.org/docs/build_tutorials/pkgs2.html.

Information on the [meta.yaml](http://conda.pydata.org/docs/building/meta-yaml.html) file.

I also found good information at
http://conda-test.pydata.org/docs/build.html,
including more detailed info on the **meta.yaml** file
and patching.

Needs `conda-build`:

    conda install conda-build

Install a local package into a local `conda` install:

    cd ~/z  # This is where I have a conda distro
    conda build /Users/mpiper/projects/csdms-stack/conda-recipes/bmi-java
	conda install --use-local bmi-java

Build and install a package using a channel to handle dependencies:

    cd ~/z  # This is where I have a conda distro
    conda build /Users/mpiper/projects/csdms-stack/conda-recipes/cca-babel -c csdms
	conda install --use-local cca-babel -c csdms

