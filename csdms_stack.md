# The CSDMS Software Stack

The CSDMS Software Stack is the set of programs
that are used to couple models
in the CSDMS framework.


## Build

These are instructions for building the stack from source, by hand.

I like to build on ***siwenna***, just to be safe.
On ***siwenna***,
source tarballs for the stack are in **~/build**.

Start by setting up a build directory:

    cd && mkdir x && cd x
	xprefix=$PWD

Install conda

    curl http://repo.continuum.io/miniconda/Miniconda-latest-Linux-x86_64.sh -o miniconda.sh
    bash miniconda.sh -f -b -p $(pwd)/conda
    conda install netcdf4 numpy nose scipy shapely pyyaml

Install babel

    ./configure --prefix=$xprefix --disable-documentation --enable-python=$xprefix/conda/bin/python --enable-java=/usr/lib/jvm/java-1.6.0-openjdk.x86_64 --with-F90-vendor=GNU --with-libparsifal=local
    make all -j4
    make install

Install cca-spec-babel

    ./configure --prefix=$xprefix --disable-contrib --with-babel-config=$xprefix/bin/babel-config
    make all
    make install

Install boost

    cd boost_1_34_0
    cp -R boost $xprefix/include

Install ccaffeine

    ./configure --prefix=$xprefix --disable-doc --without-mpi --with-boost=$xprefix/include --with-cca-babel=$xprefix --with-babel-libtool=$xprefix/bin/babel-libtool
    make
    make install

Install bocca

    ./configure --prefix=$xprefix --with-ccafe-config=$xprefix/bin/ccafe-config --with-cca-spec-babel-config=$xprefix/bin/cca-spec-babel-config --with-babel-config=$xprefix/bin/babel-config
    make build
    make install

Install boccatools

    $xprefix/conda/bin/python setup.py install --prefix=$xprefix --single-version-externally-managed --record="installed.txt"


## Setup


    PATH=$xprefix/bin:$xprefix/lib:$PATH
    LD_LIBRARY_PATH=$xprefix/lib:$LD_LIBRARY_PATH


