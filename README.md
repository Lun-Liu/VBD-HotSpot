# VBD-HotSpot

This project modified HotSpot JVM in OpenJDK8u to provide Volatile-By-Default semantics for HotSpot JVM, including a Volatile-By-Default interpreter for x86-64 and a Volatile-By-Default c2 compiler. VBD-HotSpot compiler also supports "relaxed" semantics for programmer specified variables, methods, or classes

This repo is cloned from the openjdk8u-master Mercurial repository (last changeset: 1645:b77f17326a42, Jan 25 2016). The original README file can also be found in this repo

## Build

0. before attempting to build, setup the sytem according to [System Setup] section of file 
./Readme-builds.html

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

the resulting jdk image should be found in build/*. 

\* OpenJDK8u has some compatability issue with gcc 6.0. See (http://git.net/ml/hotspot-dev/2016-05/msg00133.html). You can consider downgrade your gcc if you run into this issue.

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

