Table of Contents:

* [Exercise 0: Getting Started with GraalVM](#exercise-0-getting-started-with-graalvm)
* [Exercise 1: Running Scala applications on GraalVM](#exercise-1-running-scala-applications-on-graalvm)
* [Exercise 2: Building Native Platform binaries from Scala Applications with GraalVM](#exercise-2-building-native-platform-binaries-from-scala-applications-with-graalvm)
* [Appendix](#appendix)

# Exercise 0: Getting started with GraalVM

> As you make your way through this lab, look out for this icon.

![user input](images/userinput.png) Whenever you see it, it's time for you to
perform an action.

Hello and welcome to the GraalVM Workshop. 
GraalVM offers a high-performance runtime for JVM languages with support for others 
like JavaScript, Python, Ruby, R. In this workshop, you'll learn how Scala developers 
can benefit from GraalVM. In this workshop we're going to introduce some of the GraalVM 
abilities and how you can benefit from using GraalVM for your Scala workloads. 

This workshop assumes you have access to a Linux environment or MacOS

## 0.5. Getting started with GraalVM on premise

a. Navigate to Oracle Technology Network Downloads page and accept the license agreement:

   ![user input](images/userinput.png)   
   https://www.oracle.com/technetwork/graalvm/downloads/index.html
   
b. Select and download Oracle GraalVM Enterprise Edition based on JDK8 for your operating system (19.3.0). 
   On Windows it is currently recommended for this workshop to use WLS (Linux subsystem) or Docker. 
   Windows builds are experimental and while JVM and the JIT compiler would most probably work fine,
   you can find difficulties with the native image commands.
   
c. Extract the archive to your file system. To extract the file to the current directory from the console, type

   ![user input](images/userinput.png)
   >```sh
   >tar -xvf archive.tar.gz # on Windows – double click to unzip
   >```
   
d. There can be multiple JDKs installed on the machine and the next step is to configure the runtime environment. 
   Note, Contents/Home is a macOS construct, on Windows/Linux the commands would be using: <path to GraalVM>/bin

   Add the GraalVM bin folder to the PATH environment variable:
   
   ![user input](images/userinput.png)
   >```sh
   >export PATH=<path to GraalVM>/Contents/Home/bin:$PATH.
   >```

   Verify whether you are using GraalVM with the echo command:

   ![user input](images/userinput.png)
   >```sh
   >echo $PATH.
   >```

   Set the JAVA_HOME environment variable to resolve to the GraalVM installation directory:

   ![user input](images/userinput.png)
   >```sh
   >export JAVA_HOME=<path to GraalVM>/Contents/Home
   >```

e. On Windows skip this step. Download the native image component for your operating system from OTN, 
   for example for Linux use native-image-installable-svm-svmee-java8-linux-amd64-19.3.0.jar
   
f. On Windows skip this step. Install native-image component into GraalVM 19.3.
   	
   ![user input](images/userinput.png)
   >```sh
   >gu install -L ../native-image-installable-svm-svmee-java8-linux-amd64-19.3.0.jar
   >```
   
## 0.7. Setting up GraalVM and Installing Scala

a. Check the that the `java` command is now available and its version:

   ![user input](images/userinput.png)   
   >```sh
   >java -version
   >```
   
   Should Output:

   >```sh
   >java version "1.8.0_231"
   >Java(TM) SE Runtime Environment (build 1.8.0_231-b11)
   >Java HotSpot(TM) 64-Bit GraalVM EE 19.3.0 (build 25.231-b11-jvmci-19.3-b05, mixed mode)
   >```
   
b. Change the working directory for convenience

   ![user input](images/userinput.png)   
   >```sh
   >cd ..
   >mkdir workshop
   >cd workshop
   >```
   
c. Install SDKMan for simpler sbt/maven installation:https://sdkman.io/install

   ![user input](images/userinput.png)
   >```sh
   >curl -s "https://get.sdkman.io" | bash
   >source "$HOME/.sdkman/bin/sdkman-init.sh"
   >```
   
d. Install sbt:

   ![user input](images/userinput.png)   
   >```sh
   >sdk install sbt
   >``` 
   
e. Check that sbt installed correctly:

   ![user input](images/userinput.png)   
   >```sh
   >sbt -version
   >```
   
   Should Output:

   >```sh
   >sbt version in this project: 1.3.3
   >sbt script version: 1.3.3
   >```
   
f. Install Scala:

   ![user input](images/userinput.png)
   >```sh
   >sdk install scala
   >```
   
g. Check that Scala installed correctly:

   ![user input](images/userinput.png)    
   >```sh
   >scala -version
   >```
   
   Should Output:

   >```sh
   >Scala code runner version 2.13.1 -- Copyright 2002-2019, LAMP/EPFL and Lightbend, Inc.
   >```
   
h. Install a similar version of OpenJDK for comparisons:

   ![user input](images/userinput.png)    
   >```sh
   >sdk install java 8.0.232.hs-adpt
   >```

i. In a different terminal to the same machine: use that Java version. For enabling that run the following commands: 

   ![user input](images/userinput.png)
   >```sh
   >sdk use java 8.0.232.hs-adpt
   >export PATH=$JAVA_HOME/bin:$PATH
   >```   
 
j. Check this Java version now active:

   ![user input](images/userinput.png)    
   >```sh
   >java -version
   >```
   
   Should Output:

   >```sh
   >java version "1.8.0_231"
   >Java(TM) SE Runtime Environment (build 1.8.0_231-b11)
   >Java HotSpot(TM) 64-Bit GraalVM EE 19.3.0 (build 25.231-b11-jvmci-19.3-b05, mixed mode)
   >```
   
k. Look at the `bin` directory of GraalVM, you can see the common JDK commands there, javac, javap, java, etc. 
   It also includes GraalVM specific things, for example `node` for running node.js applications:
   
   ![user input](images/userinput.png)    
   >```sh
   >ls $JAVA_HOME/bin
   >```
   
l. Extract the archive to your file system. To extract the file to the current directory from the console, type

   ![user input](images/userinput.png)
   >```sh
   >tar -xvf archive.tar.gz # on Windows – double click to unzip
   >```

# Exercise 1: Running Scala Applications on GraalVM

GraalVM is a JDK distribution and you can use it as such, run you Scala workloads on GraalVM. 
GraalVM comes with a different top tier optimizing JIT compiler, so your long running processes 
can be optimized better than what other OpenJDK distributions can offer. 

GraalVM operates on the JVM bytecode as in the program is the compiled classes and jar files, 
so you don’t need to recompile your applications to get the performance benefits of using GraalVM. 

In this section we’ll run a sample Scala application and apply some load to it to measure the 
performance. Please note that this is sample application and while it might be representative 
of the real world results you should always be careful when extrapolating benchmark data.

The GraalVM team runs a lot of benchmarks, for example you can take a look at Renaissance.dev. 
Renaissance is a modern, open, and diversified benchmark suite for the JVM, aimed at testing 
JIT compilers, garbage collectors, profilers, analyzers and other tools.

Below is the sample result for Renaissance. You can see some Scala related workloads there: 
akka, dotty, finagle, scala-kmeans. 

https://github.com/renaissance-benchmarks/measurements/raw/master/stripe.png

In this section we’ll run a simpler experiment, we’ll use a hello world play framework application 
for our measurements. 

a. Create a sample app

   ![user input](images/userinput.png)
   >```sh
   >go to https://developer.lightbend.com/start/?group=play&project=play-samples-play-scala-hello-world-tutorial
   >click generate project
   >```

b. Unzip the archive containing the sample project

   ![user input](images/userinput.png)
   >```sh
   >cd workshop
   >unzip play-samples-play-scala-hello-world-tutorial.zip
   >cd play-samples-play-scala-hello-world-tutorial
   >sbt run
   >```
   
c. Create a sample app

   ![user input](images/userinput.png)
   >```sh
   >go to https://developer.lightbend.com/start/?group=play&project=play-samples-play-scala-hello-world-tutorial
   >click generate project
   >```   
   
d. In another tab in the terminal if running locally). Verify you can access the application:

   ![user input](images/userinput.png)
   >```sh
   >wget localhost:9000
   >```
   
e. Install hey, one of the load testing tools:

   ![user input](images/userinput.png)
   >```sh
   >wget -O hey https://storage.googleapis.com/hey-release/hey_linux_amd64
   >chmod a+x hey
   >```  
   
f. Run hey on the application. Notice the results, average req/s for example.

   ![user input](images/userinput.png)
   >```sh
   >./hey -z 300s -disable-keepalive http://127.0.0.1:9000/
   >```   

g. You will see a result similar to the below. Notice the Requests/sec value.

   >```sh
   >Summary:
    >Total:	60.0773 secs
    >Slowest:	0.2389 secs
    >Fastest:	0.0088 secs
    >Average:	0.0359 secs
    >Requests/sec:	1170.0767
    >Total data:	341573483 bytes
    >Size/request:	5221 bytes

   >Response time histogram:
    >0.009 [1]	|
    >0.032 [21608]	|■■■■■■■■■■■■■■■■■■■■■
    >0.055 [40900]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
    >0.078 [2020]	|■■
    >0.101 [448]	|
    >0.124 [225]	|
    >0.147 [106]	|
    >0.170 [48]	|
    >0.193 [36]	|
    >0.216 [26]	|
    >0.239 [5]	|

   >Latency distribution:
    >10% in 0.0226 secs
    >25% in 0.0295 secs
    >50% in 0.0350 secs
    >75% in 0.0393 secs
    >90% in 0.0458 secs
    >95% in 0.0531 secs
    >99% in 0.0897 secs

   >Details (average, fastest, slowest):
    >DNS+dialup:	0.0053 secs, 0.0088 secs, 0.2389 secs
    >DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0000 secs
    >req write:	0.0002 secs, 0.0000 secs, 0.0588 secs
    >resp wait:	0.0299 secs, 0.0010 secs, 0.2257 secs
    >resp read:	0.0004 secs, 0.0000 secs, 0.0628 secs

   >Status code distribution:
    >[200]	65423 responses
   >```   
   
## 1.1. Setting up GraalVM and Installing Scala

### Conclusions

You have seen GraalVM in action with Scala, Please provide us feedback!

## Appendix


