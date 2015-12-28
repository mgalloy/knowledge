# Subversion

My primary `svn` repository is located at

	file:///Users/mpiper/Dropbox/repository

At home, on ***arbre***,
this path is mapped to the shell variable `$DROPBOXREPO`.

When updating a working directory,
make sure that Dropbox has synced to the machine I'm working on--I've
experienced this on ***anacreon***, which doesn't maintain
a persistent wireless connection.

## Create a new repository

Here's what I did to create my `svn` repository on Dropbox:

	$ cd ~/Dropbox
    $ svnadmin create repository

## Import

Import a directory into the repository:

	$ svn import C file:///home/mpiper/Dropbox/repository/C \
	> -m "Importing C example programs"

Note that `svn` will recurse through a directory to add its contents.

## List

List the contents of a repository or a working directory:

	$ svn list $DROPBOXREPO
	C/
	F77/
	F95/
	Java/
	JavaScript/
	MATLAB/
	Python/
	bash/
	dot_files/
	geocarpentry/
	knowledge/
	make/
	mpiper-resume-plus-cv/

## Info

Get information about a repository or a working directory:

	$ svn info $DROPBOXREPO
	Path: repository
	URL: file:///home/mpiper/Dropbox/repository
	Repository Root: file:///home/mpiper/Dropbox/repository
	Repository UUID: 00c80037-1e96-4bd5-9677-fe9882b5c904
	Revision: 62
	Node Kind: directory
	Last Changed Author: mpiper
	Last Changed Rev: 62
	Last Changed Date: 2014-05-30 22:19:52 -0600 (Fri, 30 May 2014)

Or even an individual file:

	$ svn info ~/code/dot_files/bash_profile_macosx
	Path: code/dot_files/bash_profile_macosx
	Name: bash_profile_macosx
	Working Copy Root Path: /Users/mpiper/code/dot_files
	URL: file:///Users/mpiper/Dropbox/repository/dot_files/bash_profile_macosx
	Repository Root: file:///Users/mpiper/Dropbox/repository
	Repository UUID: 00c80037-1e96-4bd5-9677-fe9882b5c904
	Revision: 78
	Node Kind: file
	Schedule: normal
	Last Changed Author: mpiper
	Last Changed Rev: 78
	Last Changed Date: 2014-06-09 10:46:21 -0600 (Mon, 09 Jun 2014)
	Text Last Updated: 2014-06-09 10:45:51 -0600 (Mon, 09 Jun 2014)
	Checksum: a88e6a3341e5442ab57193edabe6c5636ee18576

This is handy because I'm tired of setting `propset:keywords` on files.

## Checkout

Checkout a local copy of a project from my repository:

	$ svn checkout file:///Users/mpiper/Dropbox/repository/knowledge

This creates the directory **knowledge** in `$PWD`.

Alternately,
you can check out a project to a named directory:

	$ svn co $DROPBOXREPO/F77 fortran_examples

## Status

Check the status of files in the working directory:

	$ svn status 
	M       emacs.md
	?       python.md

## Add

Add a new file from the working directory to the repository:

	$ svn add python.md 
	A         python.md

## Commit

Commit changes introduced in the working directory to the repository:

	$ svn commit -m "Added Python knowledge (har)"

## Export

Export a project (removing the **.svn** directory) like this:

	$ svn export $DROPBOXREPO/knowledge ~/Dropbox/knowledge

## Patching

Checkout the code to modify.
Make changes.
Create a patch file with:

	$ svn diff > the_patch.diff

The `.diff` file can be sent to the author of the project.

Note that emacs, e.g., recognizes the `.diff` extension.

Reference:
[this](https://ariejan.net/2007/07/03/how-to-create-and-apply-a-patch-with-subversion/)
article.

