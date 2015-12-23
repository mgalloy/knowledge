# Debian package management

## Documentation and examples

* [https://wiki.debian.org/Packaging](https://wiki.debian.org/Packaging)
* [https://wiki.debian.org/IntroDebianPackaging](https://wiki.debian.org/IntroDebianPackaging)
* [https://wiki.debian.org/HowToPackageForDebian](https://wiki.debian.org/HowToPackageForDebian)
* [https://wiki.debian.org/BuildingTutorial](https://wiki.debian.org/BuildingTutorial)

Also:

* [https://wiki.debian.org/SourcePackage](https://wiki.debian.org/SourcePackage)

## Preparation

Make sure the following packages are installed:

* build-essential
* devscripts (includes debuild)
* debhelper
* dh_make (package is dh-make)
* lintian
* make, cmake
* gcc

Recall, to install with `apt-get`, the syntax is:

	$ sudo apt-get install <package>

## Building a package

I'm following the
[IntroDebianPackaging](https://wiki.debian.org/IntroDebianPackaging)
example listed above.
It works with the sample code they've provided.
I'll need to expand this example for a more complex package, though.

**Goal:**
Build the GNU `hello` program into an **.deb** package,
given the source code in the tarball 
downloaded from the GNU FTP site.

1. Make a **hello** directory and change to it. This is the _package
   directory_.

		$ mkdir hello ; cd hello

1. Get the tarball.

		$ wget http://ftp.gnu.org/gnu/hello/hello-2.9.tar.gz

1. Rename the tarball, using the
   naming convention: lowercase, replace hyphens with
   underscores, add the text "orig" before the file extension.

		$ mv hello-2.9.tar.gz hello_2.9.orig.tar.gz

1. Unpack the source tarball.

		$ tar -zxf hello_2.9.orig.tar.gz

1. Change to the unpacked source directory (note the dash), and create
   a **debian** directory.

		$ cd hello-2.9
		$ mkdir debian

1. Run `dh_make` to populate the **debian** directory. (See
   [this list](http://www.debian.org/doc/manuals/maint-guide/dreq.en.html)
   of required files that are built.)

		$ dh_make -c mit -e mark.piper@colorado.edu -s -a

   Many of the files are stubs, and will need to be edited afterward.
   For example, the **rules**:

        #!/usr/bin/make -f
        # -*- makefile -*-

		export DH_VERBOSE=1

		%:
			dh $@

		override_dh_auto_install:
			$(MAKE) DESTDIR=$$(pwd)/debian/hello prefix=/usr install

   Also add a new file, **hello.dirs**, with this content:

		usr/bin

1. Build the package.

		$ debuild -us -uc

   The `lintian` tool may show warnings and errors.
   **Note**: I had to run `./configure` on ***arbre***
   before running `debuild`,
   but I think that's because I was using an old version.

1. View the package that was built.

		$ ls ..
		hello-2.9/		           hello-2.9-1.debian.tar.gz
		hello-2.9-1_amd64.build    hello-2.9-1.dsc
		hello-2.9-1_amd64.changes  hello-2.9.orig.tar.gz
		hello-2.9-1_amd64.deb

   The **.deb** file is what can be distributed. I guess the entire
   directory could be packaged as the source distribution?

1. Install the package.

		$ cd ..
		$ sudo dpkg -i hello-2.9-1_amd64.deb
		
1. Check that the package was successfully installed.

		$ dpkg -s hello
		...
		$ echo $?
		0

1. Optionally, uninstall package.

		$ sudo dpkg -r hello
		...
		$ dpkg -s hello
		...
		$ echo $?
		1

_Fin._
