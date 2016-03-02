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


## Tests

Try to load and run CEM after installing.

Start by creating an instance.
```python
from cmt.components import Cem
cem = Cem()
```

Note that I can get help on the class.
```python
help(Cem)
```

Check the default parameter values.
```python
cem.defaults
[('sediment_flux_flag', (1, '-')),
 ('shoreface_depth', (10.0, 'm')),
 ('shelf_slope', (0.001, '-')),
 ('shoreface_slope', (0.01, '-')),
 ('grid_spacing', (1000, 'm')),
 ('number_of_cols', (1000, '-')),
 ('number_of_rows', (50, '-')),
 ('run_duration', (3650.0, 'd'))]
```


Initialize the model with the help of the `setup` method.
```python
d = cem.setup()
cem.initialize(d + 'cem.txt')
```

Look at the current parameter values.
```python
cem.parameters
[('sediment_flux_flag', 1),
 ('shoreface_depth', 10.0),
 ('shelf_slope', 0.001),
 ('shoreface_slope', 0.01),
 ('grid_spacing', 5000.0),
 ('number_of_cols', 100),
 ('number_of_rows', 300),
 ('run_duration', 3650.0)]
```

Look at the input and output variable names.
```python
cem.get_input_var_names()
('sea_surface_water_wave__azimuth_angle_of_opposite_of_phase_velocity',
 'basin_outlet_water_sediment~bedload__mass_flow_rate',
 'land_surface_water_sediment~bedload__mass_flow_rate',
 'sea_surface_water_wave__period',
 'basin_outlet_water_sediment~suspended__mass_flow_rate',
 'sea_surface_water_wave__height',
 'land_surface__elevation')

cem.get_output_var_names()
('basin_outlet~coastal_center__x_coordinate',
 'basin_outlet~coastal_water_sediment~bedload__mass_flow_rate',
 'land_surface__elevation',
 'sea_water__depth',
 'basin_outlet~coastal_center__y_coordinate')
```

Update the model.
```python
cem.update()
```

Finalize the model.
```python
cem.finalize()
```

See also @mcflugen's gist: https://gist.github.com/mcflugen/0ef5d40076628f06594f.


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
