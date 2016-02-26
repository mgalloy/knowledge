# The CSDMS Software Stack

The [CSDMS Software Stack](https://github.com/csdms/csdms-stack)
is the set of programs
that are used to couple and run models
in the CSDMS framework.

We use `conda` to build and manage the stack.
Precompiled binaries are included in the `csdms` channel,
along with several models that can optionally be
installed along with the stack.
A list of all the packages in the CSDMS channel can be found at
[https://anaconda.org/csdms/packages](https://anaconda.org/csdms/packages).


## Install precompiled binaries

These are instructions for installing the stack
using our set of precompiled binaries.

Start by setting up a build directory:

    cd && mkdir csdms && cd csdms

Install `conda`:

    curl http://repo.continuum.io/miniconda/Miniconda-latest-Linux-x86_64.sh -o miniconda.sh
    bash miniconda.sh -f -b -p $(pwd)/conda
	PATH=$(pwd)/conda/bin:$PATH

Optionally (but really, why not?) install `ipython`:
```bash
conda install ipython
```

Install the stack using the CSDMS dev channel:
```bash
conda install coupling wmt-exe --override-channels -c csdms/channel/dev -c defaults
# conda install coupling wmt-exe -c csdms  # the main channel
```


## Build

These are instructions for building the stack from source, by hand.

I like to build on ***siwenna***, just to be safe.
On ***siwenna***,
source tarballs for the stack are in **~/build**.

Start by setting up a build directory:

    cd && mkdir x && cd x
	xprefix=$PWD

Install `conda` and some packages:

    curl http://repo.continuum.io/miniconda/Miniconda-latest-Linux-x86_64.sh -o miniconda.sh
    bash miniconda.sh -f -b -p $(pwd)/conda
    conda install netcdf4 numpy nose scipy shapely pyyaml

Install `babel`:

    ./configure --prefix=$xprefix --disable-documentation --enable-python=$xprefix/conda/bin/python --enable-java=/usr/lib/jvm/java-1.6.0-openjdk.x86_64 --with-F90-vendor=GNU --with-libparsifal=local
    make all -j4
    make install

Install `cca-spec-babel`:

    ./configure --prefix=$xprefix --disable-contrib --with-babel-config=$xprefix/bin/babel-config
    make all
    make install

Install `boost` libraries:

    cd boost_1_34_0
    cp -R boost $xprefix/include

Install `ccaffeine`:

    ./configure --prefix=$xprefix --disable-doc --without-mpi --with-boost=$xprefix/include --with-cca-babel=$xprefix --with-babel-libtool=$xprefix/bin/babel-libtool
    make
    make install

Install `bocca`:

    ./configure --prefix=$xprefix --with-ccafe-config=$xprefix/bin/ccafe-config --with-cca-spec-babel-config=$xprefix/bin/cca-spec-babel-config --with-babel-config=$xprefix/bin/babel-config
    make build
    make install

Install `boccatools`:

    $xprefix/conda/bin/python setup.py install --prefix=$xprefix --single-version-externally-managed --record="installed.txt"

**Todo:** Build and install other packages, including `esmf`, `coupling`, etc.

After building, may need to set paths:

    PATH=$xprefix/bin:$xprefix/lib:$PATH
    LD_LIBRARY_PATH=$xprefix/lib:$LD_LIBRARY_PATH


## Tests

Try to load and run CEM after installing.
In a Python/IPython session:

```python
import os
from cmt.components import Cem
help(Cem)
cem = Cem()
d = cem.setup()
cem.initialize(os.path.join(d, 'cem.txt'))
cem.get_output_var_names()
cem.update()
cem.finalize()
```
