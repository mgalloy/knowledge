# CentOS (and siwenna)

CentOS 6.6 is installed on ***siwenna***.
```
$ lsb_release -a
LSB Version: :base-4.0-amd64:base-4.0-noarch:core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-noarch
Distributor ID: CentOS
Description: CentOS release 6.6 (Final)
Release: 6.6
Codename: Final
```

Note that there's some crossover with my
[Linux](./linux.md) notes.

## Package manager

`yum` is the package manager.

	$ yum list
	$ yum search
	$ yum info
	$ yum install

## Emacs

Emacs isn't installed by default.
I downloaded the source tarball, installed it to **~/local**,
and added it to `PATH`:

    $ PATH=~/local/bin:$PATH


## Dev tools

Need to install gcc, autoconf, automake, etc.:

	$ sudo yum groupinstall development


## Login service

I started `sshd` so I can login from ***anacreon*** and ***solaria***:

	$ sudo /sbin/service sshd status
	$ sudo /sbin/service sshd start

I also made sure the daemon starts on boot. Set this up with

	$ sudo system-config-services

See also:

[http://www.techotopia.com/index.php/Configuring_CentOS_Remote_Access_using_SSH](http://www.techotopia.com/index.php/Configuring_CentOS_Remote_Access_using_SSH)

Set the "-X" option to enable X forwarding:

	$ ssh -X mapi8461@siwenna.colorado.edu

## File transfer

Remember how to call `scp`? Example for single file:

	$ scp N3100.tif mapi8888.colorado.edu:/data/ftp/pub/users/mapi8888

Recursively copy a directory with the `-r` flag.

Can't `scp` files into a directory requiring root permissions.


## Web server

I installed apache (package name is `httpd`).
Configuration in **/etc/httpd/conf** and
document root in **/var/www/html**.

Start web server with:

	$ sudo /sbin/service httpd start

And set it to start on boot (see sshd above).

Can now access http://localhost.

Instructions for setting up webserver: 

* http://ostechnix.wordpress.com/2013/03/05/install-apache-webserver-in-centos-6-3-scientific-linux-6-3-rhel-6-3/

It works! See http://siwenna.colorado.edu.
Need VPN to access.

I haven't yet set up my **public_html/** directory.
See: 

* http://www.coreymaynard.com/blog/centos-serving-home-directory-with-apache

I disabled stoopid SELinux:
edit **/etc/sysconfig/selinux**.


## Firewall

CentOS 6 uses `iptables` to configure and manage its firewall.
Access it through `/sbin/service` with:

    sudo /sbin/service iptables status

Also `stop`, `start`, `restart`, and `save`.
Save is important because modifications to the firewall don't persist.
(This is also a good thing because you can experiment,
then restart to reset any changes.)

Rules in iptables are executed sequentially.
There's a reject rule at the bottom of the input table.
Insert new rules above this rule.
For example, allow HTTPS:

    iptables -I INPUT 6 -p tcp --dport 443 -j ACCEPT

Open a range of ports:

    iptables -I INPUT 6 -p tcp --dport 8000:8100 -j ACCEPT

The flags are
`A` = append,
`I` = insert (at top, by default),
`D` = delete.

See also:

* https://wiki.centos.org/HowTos/Network/IPTables
* https://stackoverflow.com/a/19034727/1563298
* https://serverfault.com/a/594846
* https://stackoverflow.com/a/10197461/1563298


## Newer versions of packages

CentOS, like RHEL, values stability, so many of its packages are old.

### Python 2.7

Use anaconda,
or follow [this article](https://www.digitalocean.com/community/tutorials/how-to-set-up-python-2-7-6-and-3-3-3-on-centos-6-4) [digitalocean.com].

### CMake > 2.8

[Dakota](./dakota.md) requires CMake 2.8.9 or higher.

I [downloaded](http://www.cmake.org/files/v2.8/) CMake 2.8.9 and installed with

	$ ./bootstrap --prefix=/usr/local
	$ make
	# make install

I used `/usr/local` so as not to conflict with the CentOS 6.4 version
(cmake 2.6.4).

### Subversion 1.7

I enabled the `rpmforge-extras` repo:

	$ sudo yum-config-manager --enable rpmforge-extras

and used it to install version 1.7.
It successfully removed 1.6,
which is the CentOS default.

