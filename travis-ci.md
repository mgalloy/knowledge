# Travis CI

[Travis CI](https://travis-ci.com) is a continuous integration tool.
Every time you push a project to a repository,
Travis CI will build / integrate / test the project
on a virtual machine running Ubuntu 12.04 LTS.
[Here's](https://blog.codecentric.de/en/2012/05/travis-ci-or-how-continuous-integration-will-become-fun-again/)
a nice article that explains how Travis CI works (using a Java
project).

See the [steps](http://docs.travis-ci.com/user/getting-started/)
needed to set up Travis CI with a project.
Briefly, they are:

1. Sign in to Travis CI with Github credentials
2. Activate webhook in project settings in Travis CI (not Github!)
3. Add **.travis.yaml** file to project
4. Trigger build with a `git push`

My projects can be found at [travis-ci.org](https://travis-ci.org/).

Validate a **.travis.yaml** file at
[Travis WebLint](http://lint.travis-ci.org/).

## General

The lifecycle of a Travis CI build:

1. `before_install`
1. `install`
1. `before_script`
1. `script`
1. `after_success` or `after_failure`
1. `after_script`

More detail at
[http://docs.travis-ci.com/user/build-lifecycle/](http://docs.travis-ci.com/user/build-lifecycle/).

### Container-based infrastructure

If you don't need `sudo` in your Travis script,
you can use the new Docker-based infrastructure.
See the article [here](http://docs.travis-ci.com/user/migrating-from-legacy/).

### Other

* Skip a build by placing `[ci skip]` anywhere in the commit message.


## Java

See the Travis CI
[docs](http://docs.travis-ci.com/user/languages/java/) on using Java.

Travis CI provides OpenJDK 6, OpenJDK 7 and Oracle JDK 7.

	jdk:
	  - oraclejdk7
	  - openjdk7
	  - openjdk6

Need to create a **build.xml** file to run tests with
[Apache Ant](http://ant.apache.org/). The default test command is:

	ant test

so this target needs to be included in the **build.xml** file.

*Question:* Do libraries, like JUnit, need to be included in the
project?  It seems like JUnit would be included in the Java distros
that Travis CI provides.
I've added JUnit to each of my projects that use it.


## GWT

GWT releases can be
[downloaded](http://www.gwtproject.org/versions.html) as **.zip** files.
[Here's](http://google-web-toolkit.googlecode.com/files/gwt-2.5.1.zip)
the link to the 2.5.1 release we used in WMT (it's 106 MB). Given these
[directions](http://docs.travis-ci.com/user/installing-dependencies/#Installing-Projects-from-Source),
I can install this in Travis CI with:

	before_script:
	  - wget http://google-web-toolkit.googlecode.com/files/gwt-2.5.1.zip -O /tmp/gwt-2.5.1.zip
	  - unzip -qq /tmp/gwt-2.5.1.zip
	  - export PATH=$PATH:$PWD/gwt-2.5.1/

I'm not sure the `PATH` export is necessary;
all I need is the location of the GWT libraries
in my **build.xml** file.

I chose to include JUnit in my project.

Travis allows
[parallelization](http://docs.travis-ci.com/user/speeding-up-the-build/#Parallelizing-your-builds-across-virtual-machines)
of tests. I could use this with GWT to run the `test.dev` and
`test.prod` targets in parallel.

## Bash

The project [csdms/rpm-models](https://github.com/csdms/rpm-models)
uses bash (at least for now).

Setting

	language: bash

in the **.travis.yml** file doesn't work -- Travis reverts to Ruby --
but it doesn't appear to affect the build process.

[This](http://arongriffis.com/blog/articles/2013-03-25-bashes.html)
article provides some clues for testing with bash.

## Conda / Python

This is what @mcflugen does,
since the Debian repos used by Travis
may not have needed packages.

See [this](http://conda.pydata.org/docs/travis.html)
article
and [this](https://github.com/csdms/bmi-python/blob/master/.travis.yml)
**.travis.yml** file.
