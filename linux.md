# Linux

Notes from years and years of using it.

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
    $ [sudo] make install

By convention,
packages built from source are installed in **/usr/local**,
whereas packages from a package manager
(like [yum](./yum.md))
are installed in **/usr**.
The `configure` script usually has a `--prefix` option
to change the install location.


## See shared library dependencies

Use `ldd` to get a list of shared libraries that an executable
or a library depends on.
This command is invaluable for determining what dependencies are missing,
and which libraries are being used
(I've seen libraries from default locations,
like **/usr/lib64**,
pulled in before a local library).

Example:

```bash
$ ldd /usr/lib64/libgfortran.so.3
    linux-vdso.so.1 =>  (0x00007fff5cf3d000)
    libm.so.6 => /lib64/libm.so.6 (0x00000034df800000)
    libc.so.6 => /lib64/libc.so.6 (0x00000034de800000)
    /lib64/ld-linux-x86-64.so.2 (0x00000034de400000)
```

Another example:

```bash
$ cd ~/projects/wunderkammer
$ gfortran convert_array_indices.f90
$ ldd a.out
    linux-vdso.so.1 =>  (0x00007fff61377000)
    libgfortran.so.3 => /home/mpiper/anaconda2/lib/libgfortran.so.3 (0x00007f86e0eaa000)
    libm.so.6 => /lib64/libm.so.6 (0x00000034df800000)
    libgcc_s.so.1 => /home/mpiper/anaconda2/lib/libgcc_s.so.1 (0x00007f86e0c93000)
    libquadmath.so.0 => /home/mpiper/anaconda2/lib/libquadmath.so.0 (0x00007f86e0a57000)
    libc.so.6 => /lib64/libc.so.6 (0x00000034de800000)
    /lib64/ld-linux-x86-64.so.2 (0x00000034de400000)
```

See below for how I forced the linker to use
the **libgfortran.so.3** and **libquadmath.so.0** libs
in my Anaconda distro.


## Locate shared libraries

I like Fortran.
I use `gfortran`, provided by the GCC.
I also like Anaconda.
I installed GCC through Anaconda.
However,
when I tried to use `gfortran`,
the linker complained that a library (**libquadmath.so.0**) was missing.
Where is it?
I used `locate` to find it:

```bash
$ locate libquadmath.so.0
    /home/mpiper/anaconda2/lib/libquadmath.so.0
    /home/mpiper/anaconda2/lib/libquadmath.so.0.0.0
    /home/mpiper/anaconda2/pkgs/gcc-4.8.5-7/lib/libquadmath.so.0
    /home/mpiper/anaconda2/pkgs/gcc-4.8.5-7/lib/libquadmath.so.0.0.0
    /var/lib/mock/epel-7-x86_64/root/usr/lib64/libquadmath.so.0
    /var/lib/mock/epel-7-x86_64/root/usr/lib64/libquadmath.so.0.0.0
```

Ah, so it's in my anaconda distro, but why isn't it being found?
I'll fix this next.

## Set shared library dependencies

To force the libquadmath shared library in my Anaconda distribution
to be found,
I used `ldconfig`:

```bash
$ ldconfig -v /home/mpiper/anaconda2/lib
```
