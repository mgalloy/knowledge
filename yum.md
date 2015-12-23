# Using yum

Yum is a Linux package manager.

## Documentation and examples

* [Yum Package Manager](http://yum.baseurl.org/) [baseurl.org]
* [Managing Packages With Yum](http://prefetch.net/articles/yum.html) [prefetch.net]
* [Managing Software With Yum](http://www.centos.org/docs/4/html/yum/) [centos.org]

Also see a few inline references below.

## Search for a package

Use `search` when you don't know a package name:

	$ yum search fortran
	compat-libgfortran-41.i686 : Compatibility Fortran 95 runtime library version
	compat-libgfortran-41.x86_64 : Compatibility Fortran 95 runtime library version
	gcc-gfortran.x86_64 : Fortran support
	libgfortran.i686 : Fortran runtime
	libgfortran.x86_64 : Fortran runtime
	mingw32-gcc-gfortran.x86_64 : MinGW Windows cross-compiler for FORTRAN
	compat-gcc-34-g77.x86_64 : Fortran 77 support for compatibility compiler
	compat-libf2c-34.i686 : Fortran 77 compatibility runtime
	compat-libf2c-34.x86_64 : Fortran 77 compatibility runtime

The full names of the packages that contain the search string are listed.

## List packages

Use `list` when you do know a package name:

	$ yum list gcc-gfortran
	Available Packages
	gcc-gfortran.x86_64                       4.4.7-4.el6                       base

See also an example using `--showduplicates` below.

## Get info about a package

Get details about a package with `info`:

	$ yum info gcc-gfortran
	Available Packages
	Name        : gcc-gfortran
	Arch        : x86_64
	Version     : 4.4.7
	Release     : 4.el6
	Size        : 4.7 M
	Repo        : base
	Summary     : Fortran support
	URL         : http://gcc.gnu.org
	License     : GPLv3+ and GPLv3+ with exceptions and GPLv2+ with exceptions
	Description : The gcc-gfortran package provides support for compiling Fortran
				: programs with the GNU Compiler Collection.

## Install a package

You need to be root to `install` a package:

	$ sudo yum install gcc-gfortran

And the package name must be exact.

A package can also be installed from an URL; e.g.:

	$ sudo yum install http://siwenna.colorado.edu/hello-2.9.1.rpm

although this is nonstandard.

## Set up a repository

I set up the CSDMS repository on ***river*** using the following steps.

1. Set up a repo directory.

		$ cd /data/web/htdocs/csdms
		$ mkdir repo && cd repo

2. Populate the repo with RPMs.

		$ mv ~/build/*.rpm .

3. Run `createrepo` on the repo directory.

		$ createrepo .

	This makes a new subdirectory, **repodata**, that contains
    metadata about the RPMs in the repo.

4. Make the repo file. Here's **csdms.repo**:

		[csdms]
		name=CSDMS Repository
		baseurl=http://csdms.colorado.edu/repo
		enabled=1
		gpgcheck=0

Now a user can access the packages in the repo
by adding it to their repo list
with `yum-config-manager`.
For example, on ***siwenna***, I used:

	# yum-config-manager --add-repo http://csdms.colorado.edu/repo/csdms.repo

Enable the repo (not sure this is needed):

	# yum-config-manager --enable csdms

And here it is, yay!

	$ yum repolist
	repo id                        repo name                                  status
	base                           CentOS-6 - Base                            6,367
	csdms                          CSDMS Repository                              16
	extras                         CentOS-6 - Extras                             14
	updates                        CentOS-6 - Updates                         1,362
	repolist: 7,759

Is HydroTrend available?

	$ yum info hydrotrend
	Available Packages
	Name        : hydrotrend
	Arch        : x86_64
	Version     : head
	Release     : 1.el7.centos
	Size        : 178 k
	Repo        : csdms
	Summary     : A hydrological water balance and transport model
	URL         : http://csdms.colorado.edu/wiki/Model:HydroTrend
	License     : GPLv3
	Description : HydroTrend is a climate-driven hydrological water balance and
				: transport model that simulates water discharge and sediment load
				: at a river outlet.

Success!

See also:

* [How to setup your own package repository](http://yum.baseurl.org/wiki/RepoCreate)
* [Adding, Enabling, and Disabling a Yum Repository](http://docs.fedoraproject.org/en-US/Fedora/16/html/System_Administrators_Guide/sec-Managing_Yum_Repositories.html) [fedoraproject.org]

## Update a repository

Add new packages to the repo, then run:

	$ sudo createrepo --verbose --update .

It may take awhile for the changes to propagate to clients.
Something about renewing an HTTP lease?

## Create a group

Yum supports the group commands
`grouplist`,
`groupinfo`,
`groupinstall`,
`groupremove`, and
`groupupdate`.

I need the "Development Tools" group:

	$ yum groupinfo "Development Tools"
	# yum install "Development Tools"

The group name is case insensitive.

I made a group, "CSDMS Software Stack",
for the [CSDMS repo](http://csdms.colorado.edu/repo).
All that's required is:

1. Create a **.xml** group file with info
1. Tell `createrepo` to include this file (with the `-g` flag)
   when creating or updating the repo

Here's my XML [groups file](http://csdms.colorado.edu/repo/csdms.xml).
(Note that I made this file by hand.
The `yum-groups-manager` command didn't work when I tried
to add packages.)

Now I can access/install the entire CSDMS software stack!

	$ yum groupinfo "csdms software stack"

	Group: CSDMS Software Stack
	Description: The packages to set up and run CSDMS software
	Default Packages:
	  2dflowvel
	  Cmt
	  adi-2d
	  aquatellus
	  avulsion
	  babel
	  cca-spec-babel
	  ccaffeine
	  cem
	  chasm
	  child
	  flex1d
	  flex2d
	  flex2d-adi
	  hydrotrend
	  ltrans
	  marssim
	  plume
	  sedflux
	  storm

Reference:

* [Yum groups and repositories](http://yum.baseurl.org/wiki/YumGroups)
  [yum.baseurl.org]

## Install a specific version of a package

By default,
`yum` won't install (or even list)
a release of a package below what's available.

For example,
I have `el6` and `el7` versions of `hydrotrend`.
When I try to install,
I'm given no choice but the latest:

	$ yum list hydrotrend
	Available Packages
	hydrotrend.x86_64                    head-1.el7.centos                     csdm

However, setting `--showduplicates` shows that `el6` is available:

	$ yum list hydrotrend --showduplicates
	Available Packages
	hydrotrend.x86_64                    head-1.el6                            csdms
	hydrotrend.x86_64                    head-1.el7.centos                     csdms

Force `yum` to install the `el6` package
by specifying it's full _name-version-release_:

	$ sudo yum install hydrotrend-head-1.el6.x86_64

## Refresh the database

Clear cached information with `clean`:

	# yum clean all
