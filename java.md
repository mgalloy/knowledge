# Java

Java was pretty easy to pick up.
It's verbose, but I like it.
I'm not yet up to speed on some of the more advanced techniques.

It really pays to develop with [Eclipse](./eclipse.md).
See also [ant](./ant.md).

## Generic methods

A generic method allows one method to handle multiple types. Create a
generic method with the type parameter `T`. For example:

	public <T> void printItem(T inputItem) {
		System.out.printf("%s", T inputItem);
	}

Can also return type `T` (this is a JSNI method):

	private final native <T> T parseJSON(String jsonStr) /*-{
		return eval("(" + jsonStr + ")");
	}-*/;

I have examples of generic methods in the **wmt-client**
[ValueJSO](https://github.com/mdpiper/wmt-client/blob/master/src/edu/colorado/csdms/wmt/client/data/ValueJSO.java) and [DataTransfer](https://github.com/mdpiper/wmt-client/blob/master/src/edu/colorado/csdms/wmt/client/control/DataTransfer.java) classes.

## Class literals

Writing `.class` after a class name references the Class object it
represents. It's called a "class literal", and used when there isn't
an instance of the class available.

For example, if your class is `Print` (it is recommended that class name
begin with an uppercase letter), then `Print.class` is an object that
represents the class `Print` on runtime. It is the same object that is
returned by the `getClass()` method of any (direct) instance of `Print`.

	Print myPrint = new Print();
	System.out.println(Print.class.getName());
	System.out.println(myPrint.getClass().getName());

## Abstract class versus template

An abstract class (a language feature) can be used as a template (a
design pattern).

## Unit testing

A nice discussion of unit testing with JUnit can be found at
[http://www.vogella.com/tutorials/JUnit/article.html](http://www.vogella.com/tutorials/JUnit/article.html).

I've written a bunch of unit tests for [csdms/wmt-client](https://github.com/csdms/wmt-client) and [csdms/bmi-java](https://github.com/csdms/bmi-java).

## Initializing an array

I often seem to forget how to do this:

	private String[] connections = new String[N_CONNECTIONS];
	double[] temperature = new double[10];

Note that `List` and `ArrayList` are preferred in Java.
However,
in scientific programming,
we prefer to use arrays.
Multidimensional arrays are difficult to work with in Java.
See [csdms/bmi-java](https://github.com/csdms/bmi-java) for examples
(especially `BmiHeat:flattenArray2D`).

## Annotations

The `@` syntax is an [annotation](https://docs.oracle.com/javase/tutorial/java/annotations/basics.html).
It provides metadata.
Actually,
the [Wikipedia page](https://en.wikipedia.org/wiki/Java_annotation)
provides better information.

## Interfaces

A Java interface is like a class,
but it can only contain method signatures and fields;
it can't contain an implementation of any of the methods.

I've implemented (<--colloquial meaning) the BMI in [csdms/bmi-java](https://github.com/csdms/bmi-java).
See **BMI.java** and the set of interfaces it extends.
The class `BmiHeat` *implements* (<--Java meaning) the BMI interface.

In Java,
if a class implements an interface,
the class must define all the methods included in the interface.
(They can be stubs, but they must be present.)

## Run a program from a .class or a .jar file

This is a bit incomplete, but here goes.

Say I create a package `edu.colorado.csdms.mdpiper`
that has a class `Foo` which includes a main program.
And this is under a directory, **src**.
And I use `ant`
(which is easier than using `javac` and `java`)
to compile `Foo` to **.class** and **.jar** files,
under **build** and **dist** directories.

Then,
to run the **.class** file,
use:

    java -cp ./build edu.colorado.csdms.mdpiper.Foo

And to run the **.jar** file,
use:

    java -cp ./dist/Foo.jar edu.colorado.csdms.mdpiper.Foo

A better explanation of setting an application's entry point,
and calling the application,
can be found in Oracle's Java
[docs](http://docs.oracle.com/javase/tutorial/deployment/jar/appman.html).
