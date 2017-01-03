# Linux

Notes from my years and years of using it.

There may be some crossover here with my
[bash](./bash.md) notes.


## Get distro info

Use `uname` and/or `lsb_release`:

    $ uname -a
    $ lsb_release -a

On RH systems, also try:

    $ cat /etc/redhat-release


## Get shell

Which shell am I currently using (`bash`, `tsch`, `zsh`, etc.)?
Check with:

    $ echo -0


## Building a package from source

The three canonical steps:

    $ ./configure
    $ make
    $ sudo make install

By convention,
packages built from source are installed in **/usr/local**,
whereas packages from a package manager
(like [yum](./yum.md))
are installed in **/usr**.
The `configure` script usually has a `--prefix` option to change this.
