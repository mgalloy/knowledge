# Mac OS X

I'm still a Linux guy.

## Homebrew

I'm trying [Homebrew](http://brew.sh/) for package management
on ***anacreon*** and ***solaria***.

I want to install Bazaar and Mercurial:

	$ brew info bzr hg
	$ brew install bzr
	$ brew install hg

These packages are installed,
and a link for them is set in **/usr/local/bin**.

Nice!  I'm not sure how I went this long without Homebrew.  Mike is
teasing me because he wrote an
[article](http://michaelgalloy.com/2010/01/04/homebrew.html) on
Homebrew years ago.  I replied that I was busy at the time getting
work done in Linux, which has a built-in package management system.

Installed:

* `bzr`
* `hg`
* `wget`
* `pandoc`

## Switch between desktops

I've mapped the keyboard shortcuts `<Ctrl>-<right arrow>` and
`<Ctrl>-<left arrow>` to move between Desktops on ***solaria***.

You can also use the `F3` key to get Expose (shows an overview of
what's on all Desktops).

## Screenshots

Take a screenshot of the entire screen:

	Cmd - Shift - 3

or of a chosen area:

	Cmd - Shift - 4

or of an application window:

	Cmd- Shift - 4 - Space

## Opening applications from shell prompt

To open an application from the command line, use `open`:

	$ open /Applications/Preview.app/ mpiper-references.pdf

Even better, skip the application name:

	$ open mpiper-references.pdf

## Opening applications with Spotlight

Use `Cmd-Space` and type. I'm a fan.

## System resource use

Use the **Activity Monitor** app instead of `top`.

## Symlinks

Symlinks are a little crankier than on Linux, requiring `sudo` and a
target for directories. Here's an example:

	$ sudo ln -s ~/projects/pyjs_test/output/ ./output

## Restore tabs on Chrome

I have a bad habit of hitting `Cmd-Q` on Mac, which quits an
application. If I do this with Chrome, I can restart and restore my
open tabs by hitting `Cmd-Shift-T`. Yay!

## Emacs meta key

I made meta = command key instead of option key on Mac OS X. In my
**.emacs**, add:

	(setq mac-option-key-is-meta nil)
	(setq mac-command-key-is-meta t)
	(setq mac-command-modifier 'meta)
	(setq mac-option-modifier nil)

This setting borks the standard `Cmd-h` "hide" function on OS X.
Luckily, the Emacs standard `C-z` minimizes and maximizes the Emacs
window.

There are also conflicts with **iTerm**.
To delete a word, use `Alt-d` instead of `M-d`.
I also set the "confirm quit" **iTerm** preference,
because quit collides with the `M-q` code formatter.
I haven't got the code formatter to work in the `-nw` version of emacs.

## Emacs start directory

In emacs, I set

	(setq default-directory "~/")

to help with opening files with `C-x C-f`.

## Emacs -nw

Starting emacs from the command line runs the (old) version of emacs
provided by Apple.
Make a script named **emacs**:

	#!/bin/sh
	/Applications/Emacs.app/Contents/MacOS/Emacs -nw "$@"

and place it ahead of **/usr/bin/emacs** in `PATH` to run the
version of emacs I've installed in **/Applications**.

## Preview tips

* Move between links with the keyboard shortcuts `Cmd-[` and `Cmd-]`
* Annotations can be made and saved with a PDF

## Webserver

Apache is already installed. Execute:

	$ sudo su -
	$ apachectl start

then check [http://localhost](http://localhost).

Apache is located in **/etc/apache2**. 
DocumentRoot = **/Library/WebServer/Documents**.

I found [this article](http://jason.pureconcepts.net/2012/10/install-apache-php-mysql-mac-os-x/) helpful.
It includes directions on setting up PHP and MySQL.

Alternately,
Python includes a simple HTTP server
that allows any directory to be the document root.
Start it with:

    $ cd scratch
    $ python -m SimpleHTTPServer 8080
	
I chose `8080`. It should be open.
Then, in a web browser, open http://solaria.colorado.edu:8080,
or http://127.0.0.1:8080.
If no **index.html** file exists,
all files in the directory will be listed.

Here's the canonical test **index.html**:

    <html><body><h1>It works!</h1></body></html>


## List shared libraries of an executable

On Linux,
the `ldd` command is used to list the shared libraries of an executable.
On MacOS,
the equivalent is `otool -L`.


## Running pyjs examples

The stock pyjs examples can be executed from:

	http://localhost/pyjs-examples/

My pyjs examples can be executed from:

	http://localhost/mp-pyjs-examples/

I set up this directory in **/etc/apache2/httpd.conf** (at the very end) with:

	<IfModule alias_module>
		Alias /mp-pyjs-examples "/Users/mpiper/projects/pyjs/examples/"
	</IfModule>
	<Directory "/Users/mpiper/projects/pyjs/examples/">
		Options Indexes MultiViews FollowSymLinks +ExecCGI
		AllowOverride None
		Order deny,allow
		Deny from all
		Allow from 127.0.0.0/255.0.0.0 ::1/128
		AddHandler cgi-script .py
	</Directory>


