# SC-HotSpot

This project modified HotSpot JVM in OpenJDK8u to provide Volatile-By-Default semantics for HotSpot JVM, including a Volatile-By-Default interpreter for x86-64 and a Volatile-By-Default c2 compiler. VBD-HotSpot compiler also supports "relaxed" semantics for programmer specified variables, methods, or classes

This repo is cloned from the openjdk8u-master Mercurial repository (last changeset: 1645:b77f17326a42, Jan 25 2016). The original README file of the Mercurial File is attached at the end.

## Build

0. get the necessary system software/packages installed on your system, see
http://hg.openjdk.java.net/jdk8/jdk8/raw-file/tip/readme-builds.html

1. if you don't have a jdk7u7 or newer jdk, download and install it from
http://java.sun.com/javase/downloads/index.jsp
add the /bin directory of this installation to your path environment
variable.

2. configure the build, install necessary dependencies:
	```
	bash ./configure
	```

3. build the openjdk:
	```
	make all
	```
the resulting jdk image should be found in build/*

## Run Java with VBD-HotSpot
Volatile-By-Default can be enabled using -XX:+VBD flag (or -XX:+VBDInter and -XX:+VBDComp for interpreter and compiler only). Also -XX:-TieredCompilation is needed to turn off c1 compiler (only the c2 compiler is Volatile-By-Default now). Also remember to use -XX:-OptimizeStringConcat to disable String intrinsics optimizations, and -XX:+AggresiveMemBar to allow more optimizations for non-escaping objects.

```
BUILD_IMAGE/jdk/bin/java -XX:-TieredCompilation -XX:+VBD -XX:-OptimizeStringConcat -XX:+AggresiveMemBar SomeJavaProgram
```
## "relaxed" semantics for methods, fields, classes, etc.
VBD-HotSpot allows programmers to specify certain methods, fields, or classes to have "relaxed" semantics (current JMM semantics) instead of Volatile-by-Default semantics. To specify such methods, fields, or classes, use flags -XX:VBDRelaxedMethod, -XX:VBDRelaxedField, or -XX:VBDRelaxedClass.

```
BUILD_IMAGE/jdk/bin/java -XX:-TieredCompilation -XX:+VBD -XX:-OptimizeStringConcat -XX:+AggresiveMemBar -XX:VBDRelaxedMethod=to/relax/method1,to/relax/method2 SomeJavaProgram
```

=============================================================================
README:
  This file should be located at the top of the OpenJDK Mercurial root
  repository. A full OpenJDK repository set (forest) should also include
  the following 6 nested repositories:
    "jdk", "hotspot", "langtools", "corba", "jaxws"  and "jaxp".

  The root repository can be obtained with something like:
    hg clone http://hg.openjdk.java.net/jdk8/jdk8 openjdk8
  
  You can run the get_source.sh script located in the root repository to get
  the other needed repositories:
    cd openjdk8 && sh ./get_source.sh

  People unfamiliar with Mercurial should read the first few chapters of
  the Mercurial book: http://hgbook.red-bean.com/read/

  See http://openjdk.java.net/ for more information about OpenJDK.

Simple Build Instructions:
  
  0. get the necessary system software/packages installed on your system, see
     http://hg.openjdk.java.net/jdk8/jdk8/raw-file/tip/readme-builds.html

  1. if you don't have a jdk7u7 or newer jdk, download and install it from
     http://java.sun.com/javase/downloads/index.jsp
     add the /bin directory of this installation to your path environment
     variable.

  2. configure the build:
       bash ./configure
  
  3. build the openjdk:
       make all
     the resulting jdk image should be found in build/*/images/j2sdk-image

where make is GNU make 3.81 or newer, /usr/bin/make on Linux usually
is 3.81 or newer. Note that on Solaris, GNU make is called "gmake".

Complete details are available in the file:
     http://hg.openjdk.java.net/jdk8/jdk8/raw-file/tip/README-builds.html
