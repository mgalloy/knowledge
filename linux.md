# Notes on using Linux

Notes from my years and years of using it.

Note that there's some crossover with my
[bash](./bash.md) notes.

## Get distro

Use `uname` and/or `lsb_release`:

	$ uname -a
	$ lsb_release -a

On RH systems, also try:

	$ cat /etc/redhat-release

## Building a package from source

The three canonical steps:

	$ ./configure
	$ make
	# make install

By convention,
packages built from source are installed in **/usr/local**,
whereas packages from a package manager
(like [yum](./yum.md))
are installed in **/usr**.
The `configure` script usually has a `--prefix` option to change this.
