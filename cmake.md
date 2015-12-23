# CMake

The [CMake home page](http://www.cmake.org/).

## Documentation

* CMake [documentation](http://www.cmake.org/cmake/help/v3.0/index.html), including [commands](http://www.cmake.org/cmake/help/v3.0/manual/cmake-commands.7.html) and [variables](http://www.cmake.org/cmake/help/v3.0/manual/cmake-variables.7.html) [cmake.org]
* CTest [documentation](http://www.cmake.org/Wiki/CMake/Testing_With_CTest) [cmake.org]

I have a pretty good example of using CMake
in the [csdms-contrib/storm](https://github.com/csdms-contrib/storm)
project.

## Use pattern

From the source directory:

	$ mkdir build && cd build
	$ cmake ..
	$ make
	# make install

To run CTest, add:

	$ make test
