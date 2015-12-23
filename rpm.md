# RPM package management

How do I build software into an **.rpm**?
How do I use `rpm`?
(Actually, use [yum](./yum.md) instead of `rpm`, if possible.)

## Documentation and examples

Primary articles:

* [How to create an RPM package](https://fedoraproject.org/wiki/How_to_create_an_RPM_package) [fedoraproject.org]
* [Building RPMs](http://www.tldp.org/HOWTO/RPM-HOWTO/build.html) [tldp.org]

Quick examples:

* [How to create a GNU Hello RPM package](https://fedoraproject.org/wiki/How_to_create_a_GNU_Hello_RPM_package) [fedoraproject.org]
* [CentOS RPM tutorial](http://www.lamolabs.org/blog/164/centos-rpm-tutorial-1/)

Possibly also:

* [RPM Building](https://access.redhat.com/documentation/en-US/Red_Hat_Network_Satellite/5.3/html/Deployment_Guide/satops-rpm-building.html) - for RHEL 5 [redhat.com]
* [How to rebuild a source RPM](http://wiki.centos.org/HowTos/RebuildSRPM) [centos.org]
* [RPM Package Manager](http://en.wikipedia.org/wiki/.rpm) [Wikipedia]
* [Make your own DEB and RPM packages](http://www.linuxuser.co.uk/tutorials/make-your-own-deb-and-rpm-packages)

Note that the articles from the Fedora Project use some
Fedora-specific tools, such as `mock`.

This article on conditional macros was crazy helpful:

* [Some tips on RPM conditional macros](http://backreference.org/2011/09/17/some-tips-on-rpm-conditional-macros/) [backreference.org]

and this write-up was nicely descriptive:

* [pkgbuild's spec files](http://pkgbuild.sourceforge.net/spec-files.txt)
[sourceforge.net]

See also some references included in the sections below.

## Preparation

Make sure the following packages are installed:

* rpmbuild (package name is rpm-build)
* rpmdevtools (optional, but really helpful)
* rpmlint
* redhat-rpm-config
* make, cmake
* gcc

Recall, to install with `yum`, the syntax is:

	$ sudo yum install <package>

For reference, see:

* [Set up an RPM build environment under CentOS](http://wiki.centos.org/HowTos/SetupRpmBuildEnvironment) [centos.org]

## Building a package

I'm basing this section on the
[How to create a GNU Hello RPM package](https://fedoraproject.org/wiki/How_to_create_a_GNU_Hello_RPM_package)
example listed above.

**Goal:**
Build the GNU `hello` program into an **.rpm** package,
given the source code in the tarball 
downloaded from the GNU FTP site.

Note that RPM normally builds both binary and source packages.

1. From `$HOME`, run `rpmdev-setuptree`.

		$ rpmdev-setuptree

	This wonderful command stubs out the RPM build directories under a
	directory called **rpmbuild**.

		$ l rpmbuild/
		BUILD/  BUILDROOT/  RPMS/  SOURCES/  SPECS/  SRPMS/

1. Change to **rpmbuild/SOURCES** and grab the GNU `hello` tarball.

		$ cd rpmbuild/SOURCES
		$ wget http://ftp.gnu.org/gnu/hello/hello-2.9.tar.gz

	The tarball doesn't need to be unpacked.

1. Create a new **.spec** file for the package, using the base name of
   the package.

		$ cd ~/rpmbuild/SPECS
		$ rpmdev-newspec hello
		hello.spec created; type minimal, rpm version >= 4.11.

	I could use a template here, for scripting.

1. Edit the all-important **.spec** file. See
   [Creating a SPEC file](https://fedoraproject.org/wiki/How_to_create_an_RPM_package#Creating_a_SPEC_file)
   and
   [SPEC file overview](https://fedoraproject.org/wiki/How_to_create_an_RPM_package#SPEC_file_overview). Result:

		Name:           hello
		Version:        2.9
		Release:        1%{?dist}
		Summary:        The "Hello World" program from GNU
		Group:          Applications/Productivity
		License:        MIT
		URL:            http://ftp.gnu.org/gnu/%{name}
		Source0:        http://ftp.gnu.org/gnu/%{name}/%{name}-%{version}.tar.gz
		BuildRoot:      %{_topdir}/BUILDROOT/%{name}-%{version}-%{release}

		BuildRequires:  gettext
		Requires(post): info
		Requires(preun): info

		%description
		The GNU "Hello World" program, with bells and whistles.

		%prep
		%setup -q # or '%autosetup' after RHEL 5

		%build
		%configure
		make %{?_smp_mflags}

		%install
		rm -rf $RPM_BUILD_ROOT
		make install DESTDIR=$RPM_BUILD_ROOT # or '%make_install' after RHEL 5
		%find_lang %{name}

		%clean
		rm -rf $RPM_BUILD_ROOT

		%post
		/sbin/install-info %{_infodir}/%{name}.info %{_infodir}/dir || :

		%preun
		if [ $1 = 0 ] ; then
		/sbin/install-info --delete %{_infodir}/%{name}.info %{_infodir}/dir || :
		fi

		%files -f %{name}.lang
		%defattr(-,root,root,-)
		%doc AUTHORS ChangeLog COPYING NEWS README THANKS TODO
		%{_mandir}/man1/hello.1.gz
		%{_infodir}/%{name}.info.gz
		%{_bindir}/hello

		%changelog
		* Fri Aug 1 2014 Mark Piper <mark.piper@colorado.edu> - 2.9-1
		- Initial version of the package

	The Summary tag can't end with a period.
	
	The Group and BuildRoot tags are optional after RHEL 5.

	You can use `$RPM_BUILD_ROOT` instead of `%{buildroot}`.
	Both are acceptable, but be consistent.

	In the changelog, the date format is generated with:

		$ date +"%a %b %d %Y"
		Thu Jul 31 2014

	Some macros (e.g., `%autosetup`) were introduced after RHEL 5.

1. Call `rpmbuild` to build the source, binary and debugging packages.

		$ cd ~/rpmbuild
		$ rpmbuild -ba SPECS/hello.spec

	On success,
	the binary RPM will be in **~/rpmbuild/RPMS**
	and the source RPM will be in **~/rpmbuild/SRPMS**.

	Note that this may be an iterative process: edit **.spec** file,
	rebuild package.

1. When finished, call `rpmlint` to check for conformance with RPM
   design rules.

		$ rpmlint hello.spec ../SRPMS/hello* ../RPMS/*/hello*

## Installing/uninstalling a package

1. Install the package with `yum` or `rpm`.

		$ sudo yum install hello.rpm # better
		$ sudo rpm -ivp hello.rpm

	[Yum](./yum.md) is preferred because it checks dependencies.

1. Check that the package was successfully installed.

		$ hello --version
		...
		$ echo $?
		0
		
1. Uninstall the package.

		$ sudo yum remove hello # better
		$ sudo rpm -e hello

	Optionally, check the uninstall.

		$ hello --version
		...
		$ echo $?
		127 

## Subpackages

Subpackages can be used to make separate RPMs for programs contained
in a package.
For example,
`sedflux` contains `avulsion` and `plume`.
I'd like to make separate RPMs for each model.
[Here's](https://github.com/csdms/rpm_models/blob/master/sedflux/sedflux.spec)
the spec file I created.

See:

* [Creating subpackages](http://www.rpm.org/max-rpm/ch-rpm-subpack.html) [rpm.org], especially the section [Spec File Changes For Subpackages](http://www.rpm.org/max-rpm/s1-rpm-subpack-spec-file-changes.html)
* [How to create an RPM package](https://fedoraproject.org/wiki/How_to_create_an_RPM_package) [fedoraproject.org] has just everything, if you read carefully

## Relocatable packages

A relocatable package can be installed wherever a user chooses.
Enable by adding the `Prefix` tag in the spec file:

	Prefix:  /usr

This means that any package `%files` under the install path of **/usr**
can be moved elsewhere.

On install, use the `--prefix` flag:

	# rpm -Uvh --prefix /usr/local hydrotrend.x86_64.rpm

All the `hydrotrend` files are now placed under **/usr/local**
instead of **/usr**.

One big problem is that `yum` doesn't work with relocatable packages!

See:

* [Making a Relocatable Package](http://www.rpm.org/max-rpm/ch-rpm-reloc.html) [rpm.org]
* [http://stackoverflow.com/a/25386412](http://stackoverflow.com/a/25386412) on why `yum` doesn't work

## Defining macros

Macros are variables that define settings for the RPM system.
I like to use them in spec files, but they can be set elsewhere.

Example:

	%define name1 avulsion

Set this at the top of the spec file.
When `%{name1}` is used, it'll be replaced with "avulsion".

See:

* [Customizing with RPM Macros](http://docs.fedoraproject.org/en-US/Fedora_Draft_Documentation/0.1/html/RPM_Guide/ch-customizing-rpm.html) [fedoraproject.org]

## Get info on an uninstalled RPM

Use `rpminfo` in the
[RPM development tools](http://fedoraproject.org/wiki/Rpmdevtools):

    $ rpminfo sedflux-head-3.el6.x86_64.rpm

Or use `rpm` itself.
This is everything:

	$ rpm -qpil dist/Cmt-0.1.0-1.noarch.rpm | less
	...

This is less verbose:

	$ rpm -qip RPMS/x86_64/sedflux-head-2.el6.x86_64.rpm
	Name        : sedflux                      Relocations: (not relocatable)
	Version     : head                              Vendor: (none)
	Release     : 2.el6                         Build Date: Wed 27 Aug 2014 06:19:31 PM MDT
	Install Date: (not installed)               Build Host: siwenna.colorado.edu
	Group       : Applications/Engineering      Source RPM: sedflux-head-2.el6.src.rpm
	Size        : 16437610                         License: GPLv2
	Signature   : (none)
	URL         : http://csdms.colorado.edu/wiki/Model:Sedflux
	Summary     : Basin-filling stratigraphic model
	Description :
	Sedflux-2.0 is the newest version of the Sedflux basin-filling
	model. Sedflux-2.0 provides a framework within which individual
	process-response models of disparate time and space resolutions
	communicate with one another to deliver multi grain sized sediment
	load across a continental margin.

## Unpack an RPM

Best: use `rpmdev-extract` from the
[RPM development tools](http://fedoraproject.org/wiki/Rpmdevtools):

    $ rpmdev-extract sedflux-head-3.el6.x86_64.rpm

When the dev tools aren't available,
use `rpmcpio` and `cpio`. For example:

	$ rpm2cpio hydrotrend-head-2.el6.x86_64.rpm | cpio -idmv
	./usr/local/csdms/bin/hydrotrend
	./usr/local/csdms/include/bmi_hydrotrend.h
	./usr/local/csdms/include/hydrotrend_cli.h
	./usr/local/csdms/lib64/libhydrotrend.so
	./usr/local/csdms/lib64/pkgconfig/hydrotrend.pc
	./usr/local/csdms/share/hydrotrend/input/HYDRO.IN
	./usr/local/csdms/share/hydrotrend/input/HYDRO0.HYPS
	./usr/local/csdms/share/hydrotrend/output/HYDROASCII.Q
	./usr/share/doc/hydrotrend-head
	./usr/share/doc/hydrotrend-head/FLOWCHART
	./usr/share/doc/hydrotrend-head/NEWS
	./usr/share/doc/hydrotrend-head/README
	972 blocks

See:

* [http://stackoverflow.com/a/18787544/1563298](http://stackoverflow.com/a/18787544/1563298)

## Signing an RPM

Export my GPG key from my keyring:

    $ gpg --export -a 'Mark Piper (CSDMS)' > RPM-GPG-KEY-mpiper

Import the key into the local RPM setup:

    $ sudo rpm --import RPM-GPG-KEY-mpiper

Then, to sign an RPM, execute:

    $ rpm --addsign foo.rpm

and you'll be prompted for the passphrase.

See:

* [How to sign your custom RPM package with GPG Key](http://fedoranews.org/tchung/gpg/) [fedoranews.org]

## Notes

* Never build RPMs as root.

* For testing,
my VM on ***solaria*** is CentOS 7,
***siwenna*** is CentOS 6,
and ***river*** is RHEL 5.

* See my successful build setup for CSDMS models at
[https://github.com/csdms/rpm-models](https://github.com/csdms/rpm-models).

* Information on the
[rpath issue](http://fedoraproject.org/wiki/RPath_Packaging_Draft),
which gives errors like:

		ERROR   0001: file '/usr/lib64/libsidl-1.4.0.so' contains a standard rpath '/usr/lib64' in [/usr/lib64]

	When building babel, I worked around this with:

		$ QA_RPATHS=$[ 0x0001|0x0010 ] rpmbuild -bi SPECS/babel.spec
