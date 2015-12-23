# Mock

Mock is used to build RPMs in a [chroot](http://en.wikipedia.org/wiki/Chroot) 
-- a virtual environment on Linux.

## References

* [http://fedoraproject.org/wiki/Projects/Mock](http://fedoraproject.org/wiki/Projects/Mock)
* [https://fedoraproject.org/wiki/Using\_Mock\_to\_test\_package\_builds](https://fedoraproject.org/wiki/Using_Mock_to_test_package_builds)
* [How to build an RPM using Mock](http://vertis.io/2013/09/19/how-to-build-an-rpm-using-mock.html) [vertis.io]
* [http://blog.scottlowe.org/2013/01/22/using-mock-to-build-libvirt-1-0-1-rpms-for-centos-6-3/](http://blog.scottlowe.org/2013/01/22/using-mock-to-build-libvirt-1-0-1-rpms-for-centos-6-3/)

For notes on building an EL5 RPM, see:

* [https://wiki.nikhef.nl/grid/Building\_RPMs\_using\_mock](https://wiki.nikhef.nl/grid/Building_RPMs_using_mock), with money quote: *The source RPM must be generated on an EL5 machine, because EL5 and older systems have a version of RPM that doesn't understand the new SHA1 checksum.*
* [http://repoforge.org/package/quickstart.html](http://repoforge.org/package/quickstart.html)

## Process

See all the build options in **/etc/mock**.

I had to edit **epel-6-x86_64.cfg** to remove beta repos and enable the 
base and updates repos.

Get and install EPEL:

    $ wget http://fedora.mirrors.pair.com/epel/6/i386/epel-release-6-8.noarch.rpm
    $ yum localinstall epel-release-6-8.noarch.rpm

Need to install Fedora packager tools:

    $ yum install fedora-packager

Add me to mock:

    $ sudo usermod -a -G mock mpiper

Set up the chroot:

    $ mock -r epel-7-x86_64 --init

Mock builds the chroot in **/var/lib/mock**.
This took a bit because it made a Linux filesystem in this directory.

I had to patch **/usr/lib/rpm/redhat/brp-strip-static-archive** in the 
chroot:

```bash
	$ diff -u /usr/lib/rpm/redhat/brp-strip-static-archive /usr/lib/rpm/redhat/brp-strip-static-archive.mp
	--- /usr/lib/rpm/redhat/brp-strip-static-archive	2014-10-24 10:28:05.169702439 -0600
	+++ /usr/lib/rpm/redhat/brp-strip-static-archive.mp	2014-10-24 10:35:36.048475524 -0600
	@@ -12,5 +12,11 @@
	grep -v "^${RPM_BUILD_ROOT}/\?usr/lib/debug"  | \
	grep 'current ar archive' | \
	sed -n -e 's/^\(.*\):[ 	]*current ar archive/\1/p'`; do
-	$STRIP -g "$f"
+	if test -w "$f"; then
+		$STRIP -g "$f"
+	else
+		chmod u+w "$f"
+		$STRIP -g "$f"
+		chmod u-w "$f"
+	fi
	done
```

Build the binary RPM for `csdms-python`:

      $ mock -r epel-7-x86_64 --no-clean --define="_prefix /usr/local/csdms" csdms-python-2.7.6-2.el6.src.rpm

Results and/or logs in: **/var/lib/mock/epel-7-x86_64/result**.

I used `--no-clean` so my patch didn't get overwritten.
Also, this will prevent installed packages (discussed next)
from getting wiped.


Install the package into the chroot with `--install`:

    $ cd /var/lib/mock/epel-7-x86_64
    $ mock -r epel-7-x86_64 --install result/csdms-python-2.7.6-2.el7.centos.x86_64.rpm 

Can also install other packages available in `yum`; e.g.:

    $ mock -r epel-7-x86_64 --install zlib-devel blas-devel lapack-devel

Copy files into the chroot with `--copyin`.
