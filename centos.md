# Notes on using CentOS

It's a bit different from Ubuntu.

CentOS 6.4 is installed on ***siwenna***.

## Package manager

`yum` is the package manager.

	$ yum list
	$ yum search
	$ yum info
	$ yum install

## Emacs

Emacs isn't installed by default.
Install with:

	$ sudo yum install emacs.x86_64

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

	$ scp N3100.tif mapi8461@river.colorado.edu:/data/ftp/pub/users/mapi8461

Recursively copy a directory with the "-r" flag.

Can't `scp` files into a directory requiring root permissions.

## Webserver

I installed `apache` (package name = `httpd`).
Configuration in **/etc/httpd/conf**.
Started with:

	$ sudo /sbin/service httpd start

And set it to start on boot (see sshd above).

Can now access [http://localhost](http://localhost).

Tried to set up
[http://localhost/~mapi8461](http://localhost/~mapi8461), but I'm
getting a 403: Forbidden error. (I want to serve my pyjs examples from
here.)  Solution: need to modify stoopid SELinux:

	# setsebool -P httpd_read_user_content=1 httpd_enable_homedirs=1

See also: 

 - http://www.coreymaynard.com/blog/centos-serving-home-directory-with-apache
 - http://wiki.centos.org/HowTos/SELinux
 - https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/4/html/SELinux_Guide/rhlcommon-section-0068.html (section 5.2.8)

Can now access my **public_html/** directory, but not the directories
beneath it. This is driving me nutty. 

I put my pyjs and GWT examples in **/var/www/html/examples**.
Access them with:

	http://siwenna.colorado.edu/examples/

Still can't access http://siwenna.colorado.edu outside VPN. I think we
could use this for:

 - sharing examples
 - testing
 - working from home

Instructions for setting up webserver: 

	http://ostechnix.wordpress.com/2013/03/05/install-apache-webserver-in-centos-6-3-scientific-linux-6-3-rhel-6-3/

It works!

## Newer versions of packages

CentOS, like RHEL, values stability, so many of its packages are old.

### Python 2.7

Use anaconda,
or follow [this article](https://www.digitalocean.com/community/tutorials/how-to-set-up-python-2-7-6-and-3-3-3-on-centos-6-4) [digitalocean.com].

### CMake > 2.8

[DAKOTA](./dakota.md) requires CMake 2.8.9 or higher.

I [downloaded](http://www.cmake.org/files/v2.8/) CMake 2.8.9 and installed with

	$ ./bootstrap --prefix=/usr/local
	$ make
	# make install

I used `/usr/local` so as not to conflict with the CentOS 6.4 version
(cmake 2.6.4).

### Subversion 1.7

I enabled the `rpmforge-extras` repo:

	$ sudo yum-config-manager --enable rpmforge-extras

and used it to install svn 1.7.
It successfully removed svn 1.6,
which is the CentOS default.

