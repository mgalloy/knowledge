# GWT

GWT was the Google Web Toolkit,
now it's just GWT.

**TODO**: Import my notes from [http://csdms.colorado.edu/trac/csdms/wiki/UsingGWT](http://csdms.colorado.edu/trac/csdms/wiki/UsingGWT).

## Documentation and examples

* [Project Home](http://www.gwtproject.org/) [gwtproject.org]
* [Showcase](http://samples.gwtproject.org/samples/Showcase/Showcase.html) [gwtproject.org]
* [API Reference](http://www.gwtproject.org/javadoc/latest/index.html) [gwtproject.org]

## Development Mode

Development Mode (Dev Mode)
lets you test a GWT application on a local machine
without a webserver.
More info on it [here](http://www.gwtproject.org/doc/latest/DevGuideCompilingAndDebugging.html#dev_mode).

I typically run Dev Mode from Eclipse,
but it can be run from the CL with `ant`:

	$ ant devmode

Dev Mode has been superceded by Super Dev Mode in GWT 2.7.
(WMT currently uses GWT 2.5.1.)
More on Super Dev Mode at
[http://www.gwtproject.org/articles/superdevmode.html](http://www.gwtproject.org/articles/superdevmode.html).

Note:
The technology behind Development Mode is no longer supported on browsers.
I installed Firefox 24,
the last version of Firefox that it runs on,
on ***solaria***.

## Build

Build **wmt-client** with

    $ ant build

## Determine browser

Use `Window.Navigator.getUserAgent()`,
then test the returned user agent string
for a browser name.


