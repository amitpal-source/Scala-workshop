Table of Contents:

* [Exercise 0: Getting Started with GraalVM](#exercise-0-getting-started-with-graalvm)
* [Exercise 1: Running Scala applications on GraalVM](#exercise-1-running-scala-applications-on-graalvm)
* [Exercise 2: Building Native Platform binaries from Scala Applications with GraalVM](#exercise-2-building-native-platform-binaries-from-scala-applications-with-graalvm)
* [Exercise 3: Running Scala and R in a Polyglot on GraalVM](#exercise-3-running-scala-and-r-in-a-polyglot-on-graalvm)
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
   > Total:	60.0773 secs
   > Slowest:	0.2389 secs
   > Fastest:	0.0088 secs
   > Average:	0.0359 secs
   > Requests/sec:	1170.0767
   > Total data:	341573483 bytes
   > Size/request:	5221 bytes
   >``` 
   >```sh
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
   >```
   >```sh
   >Latency distribution:
   >10% in 0.0226 secs
   >25% in 0.0295 secs
   >50% in 0.0350 secs
   >75% in 0.0393 secs
   >90% in 0.0458 secs
   >95% in 0.0531 secs
   >99% in 0.0897 secs
   >```
   >```sh
   >Details (average, fastest, slowest):
   >DNS+dialup:	0.0053 secs, 0.0088 secs, 0.2389 secs
   >DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0000 secs
   >req write:	0.0002 secs, 0.0000 secs, 0.0588 secs
   >resp wait:	0.0299 secs, 0.0010 secs, 0.2257 secs
   >resp read:	0.0004 secs, 0.0000 secs, 0.0628 secs
   >```
   >```sh
   >Status code distribution:
   >[200]	65423 responses
   >```

h. Stop the sbt run in the original terminal:

   ![user input](images/userinput.png)
   >```sh
   >Ctrl + d
   >```
   
i. Compare to the performance of another JDK. 
   
   ![user input](images/userinput.png)
   >```sh
   >Open the terminal where you had OpenJDK installed and configured (see excercise 0.7 h))  
   >```
    
j. Run the app with OpenJDK:

   ![user input](images/userinput.png)
   >```sh
   >sbt run
   >```   

k. In the terminal where you ran `hey` run the load tests and compare the results to the previous run:

   ![user input](images/userinput.png)
   >```sh
   >./hey -z 300s -disable-keepalive http://127.0.0.1:9000/
   >```   

## 1.1. Performance of scalac on Scala projects, on the example of Akka

a. Clone a sample reasonably large scala project, for example: https://github.com/akka/akka.git

   ![user input](images/userinput.png)
   >```sh
   >git clone https://github.com/akka/akka.git
   >```
   
b. Navigate to akka directory.
   
   ![user input](images/userinput.png)
   >```sh
   >cd akka  
   >```
    
c. Enter `sbt`, run the `clean compile` cycle twice, notice the time it takes for 1 cycle. 
   `akka >` is the sbt REPL prefix, you don’t have to type it in.

   ![user input](images/userinput.png)
   >```sh
   >sbt
   >akka > clean 
   >akka > compile
   >akka > clean 
   >akka > compile
   >```   

d. The very first build needs to download dependencies and will be much slower, 
   please disregard that result, because it’s not representative of the normal 
   development setup, where you have the dependencies downloaded and cached by sbt. 
   The second clean compile cycle is representative of the results.
   
e. Compare to the results with the performance of OpenJDK. Use the terminal where you 
   had OpenJDK installed and configured.   

   ![user input](images/userinput.png)
   >```sh
   >sbt
   >```   
   
f. Build the project repeatedly, notice times:   

   ![user input](images/userinput.png)
   >```sh
   >>clean compile
   >>clean compile
   >```   
   
g. Build times are often cited as an important problem in the Scala community and 
   speeding up the compilation cycle by using a better performing runtime. 

# Exercise 2: Building Native Platform binaries from Scala Applications with GraalVM

GraalVM can compile JVM bytecode to native platform binaries (or shared libraries) ahead of time. 
The result is then an executable file (or a library) which doesn’t require the JVM to run. 
It also doesn’t need to initialize a lot of services and JVM components at startup, because 
it has precompiled many things already. 

In this section you’ll learn how to use GraalVM native-image utility to create GraalVM native images 
– the native binary. 

The native image capability of GraalVM is available as Early Adopter technology. We have installed it 
in the first part of this workshop.

a. Let’s create a small Scala application to test the native images. Use the following command to initialize the project:

   ![user input](images/userinput.png)
   >```sh
   >mkdir sbt-example
   >cd sbt-example
   >touch build.sbt
   >mkdir -p src/main/scala/example
   >```

b. Then in the example directory create the application file `Hello.scala`:

   ![user input](images/userinput.png)
   >```sh
   >package example
   >object Hello extends App {
   >println("Hello World!")
   >}
   >```
   
c. You can use GraalVM native images through various Maven/Gradle/sbt plugins, but in its purest form 
   it’s a command line utility that takes your application class files and jar files as the input. 
   Perhaps the simplest way is to build a “fat” jar which contains all the classes which will used in the app. 
   Add the assembly plugin to the build:

   ![user input](images/userinput.png)
   >```sh
   >mkdir project
   >echo 'addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.14.10")' > project/plugins.sbt
   >```   
   
d. Run the build to get the fat jar:

   ![user input](images/userinput.png)
   >```sh
   >sbt assembly
   >```
   
e. Run the native-image utility on the application archive to build the native image:

   ![user input](images/userinput.png)
   >```sh
   >native-image -cp target/scala-2.12/sbt-example-assembly-0.1.0-SNAPSHOT.jar -H:Class=example.Hello
   >```  
   
f. Check how much time the application takes when run normally:

   ![user input](images/userinput.png)
   >```sh
   >/usr/bin/time -v java -jar target/scala-2.12/sbt-example-assembly-0.1.0-SNAPSHOT.jar
   >```   

g. You will see a result similar to the below.

   >```sh
   >Hello World!
   >Command being timed: "java -jar target/scala-2.12/sbt-example-assembly-0.1.0-SNAPSHOT.jar"
   >User time (seconds): 0.54 
   >System time (seconds): 0.05
   >Percent of CPU this job got: 138%
   >Elapsed (wall clock) time (h:mm:ss or m:ss): 0:00.42
   >Average shared text size (kbytes): 0
   >Average unshared data size (kbytes): 0
   >Average stack size (kbytes): 0
   >Average total size (kbytes): 0
   >Maximum resident set size (kbytes): 64640 <- 64M
   >Average resident set size (kbytes): 0
   >Major (requiring I/O) page faults: 0
   >Minor (reclaiming a frame) page faults: 8065
   >Voluntary context switches: 1062
   >Involuntary context switches: 10
   >Swaps: 0
   >File system inputs: 0
   >File system outputs: 64
   >Socket messages sent: 0
   >Socket messages received: 0
   >Signals delivered: 0
   >Page size (bytes): 4096
   >Exit status: 0
   >``` 

h. Compare with the time it takes to start the native image:

  ![user input](images/userinput.png)
   >```sh
   >/usr/bin/time -v ./example.hello
   >```
   
i. You will see a result similar to the below. 

   >```sh
   >Hello World!
   >Command being timed: "./example.hello"
   >User time (seconds): 0.00
   >System time (seconds): 0.00
   >Percent of CPU this job got: 100%
   >Elapsed (wall clock) time (h:mm:ss or m:ss): 0:00.00
   >Average shared text size (kbytes): 0
   >Average unshared data size (kbytes): 0
   >Average stack size (kbytes): 0
   >Average total size (kbytes): 0
   >Maximum resident set size (kbytes): 4932 <- 4M
   >Average resident set size (kbytes): 0
   >Major (requiring I/O) page faults: 0
   >Minor (reclaiming a frame) page faults: 169
   >Voluntary context switches: 1
   >Involuntary context switches: 1
   >Swaps: 0
   >File system inputs: 0
   >File system outputs: 0
   >Socket messages sent: 0
   >Socket messages received: 0
   >Signals delivered: 0
   >Page size (bytes): 4096
   >Exit status: 0
   >```

j. Go back to the workshop dir:

   ![user input](images/userinput.png)
   >```sh
   >cd..
   >```   

k. GraalVM native images have certain runtime properties – they start fast and consume less memory 
   because they do not include the JIT compilation facilities for the bytecode. They operate under 
   the closed world assumption – all the bytecodes that are going to be executed need to be analyzed 
   and compiled at the generation step. This mean that certain dynamic features of the language need 
   to be configured, for example, Java reflection API usage. Please consult documentation or your 
   workshop leader for additional information: https://github.com/oracle/graal/tree/master/substratevm
   
   However that doesn’t mean that useful programs can’t be compiled ahead of time. In the following 
   example we’ll build the native image of the `scalac` so it doesn’t spend time starting a jvm every 
   time we call it.
   
## 2.1. Native image of Scalac 

For this particular exercise you need to install Scala 2.12.6. Scala 2.13 would work in a similar fashion, 
but currently this issue is blocking it: https://github.com/oracle/graal/issues/877

a. So please install Scala 2.12.6 (the version that is known to work).

   ![user input](images/userinput.png)
   >```sh
   >sdk install scala 2.12.6
   >```

b. It will install scala 2.12.6. If you want to switch between Scala versions, use:

   ![user input](images/userinput.png)
   >```sh
   >sdk use scala 2.12.6 # or sdk use scala 2.13.1
   >```
   
c. Clone the repository with the GraalVM demos:

   ![user input](images/userinput.png)
   >```sh
   >git clone https://github.com/graalvm/graalvm-demos.git
   >```   
   
d. Navigate to the scalac-native demo:

   ![user input](images/userinput.png)
   >```sh
   >cd graalvm-demos/scala-days-2018/scalac-native/
   >```
   
e. Make sure that environment variables SCALA_HOME and GRAALVM_HOME point to Scala 2.12.6 and GraalVM. 
   Then build the sbt project scalac-substitutions:

   ![user input](images/userinput.png)
   >```sh
   >export SCALA_HOME=<path to Scala>/.sdkman/candidates/scala/2.12.6/
   >export GRAALVM_HOME=<path to GraalVM>/Contents/Home/
   >cd scalac-substitutions
   >sbt package
   >cd ../
   >```  
   
f. To build the native image of the Scala compiler run:

   ![user input](images/userinput.png)
   >```sh
   >./scalac-image.sh
   >``` 
   
g. The above command builds the native image like in the previous section, but of scalac which is a Scala 
   program itself. This allows to compile Scala apps without needing to start and warmup the JVM which for 
   short invocations of scalac can by much faster than otherwise.

h. Let's see how we compare to the JVM on the first run (scalac-native is a shell script that sets environment 
   variables and calls native image of scalac):

   ![user input](images/userinput.png)
   >```sh
   >$ time $SCALA_HOME/bin/scalac HelloWorld.scala
   >real	0m2.315s
   >user	0m5.868s
   >sys	0m0.248s
   >```
   >```sh
   >& time ./scalac-native HelloWorld.scala
   >real	0m0.177s
   >user	0m0.129s
   >sys	0m0.034s
   >```

i. The time difference is clearly visible and you can expect similar effects. 
   One project that uses GraalVM native images is Scalafmt, you can read more about it here:
   https://scalameta.org/scalafmt/docs/installation.html#native-image

# Exercise 3: Running Scala and R in a Polyglot on GraalVM

An example project that demonstrates how to execute R files from Scala using GraalVM
>```sh
>import org.graalvm.polyglot.{Context, Source}
>object Main extends App {
>// We need to initialise a GraalContext that will do the mediation between the JVM languages and R
>val context: Context = Context.newBuilder("R").allowAllAccess(true).build()
>// Next, we need to create a Source, which needs to know what language it features and where to find the code.
>val source: Source = Source.newBuilder("R", Main.getClass.getResource("funHelloWorld.R")).build()
>/*
>* Then, we need to tell our compiler what kind of function this new Source represents.
>* In this case it is a function that doesn't take an argument and returns a String.
>* We use the graal context to convert the source into the function.
>* Because R is very dynamically typed, the compiler cannot help you here: it trusts that you give it correct instructions!
>* This also means that you may want to wrap any call to this function in a Try!
>*/
>val rHelloWorld: () => String = context.eval(source).as(classOf[() => String])
>/*
>* Finally, we can run the function. Because it doesn't take any arguments, we don't provide any.
>* Let's also print the returned String, for good measure.
>*/
>println(s"Also printing the return String: ${rHelloWorld()}")
>}
>``` 

https://medium.com/codestar-blog/in-search-of-the-holy-graalvm-putting-the-r-in-scala-or-java-or-b057494f77

a. Install R

   ![user input](images/userinput.png)
   >```sh
   >cd /Users/adhillon/Desktop/InJapan/GraalVM-latest/graalvm-ee-java8-19.3.0/Contents/Home/bin
   >gu install R
   >cd /usr/local
   >sudo mkdir include
   >sudo chown -R $(whoami) $(brew --prefix)/include
   >brew link gcc@4.9
   >/Users/adhillon/Desktop/InJapan/GraalVM-latest/graalvm-ee-java8-19.3.0/Contents/Home/jre/languages/R/bin/configure_fastr
   >R
   >R version 3.6.1 (FastR)
   >```

b. Clone the following project into current directory:

   ![user input](images/userinput.png)
   >```sh
   >git clone https://github.com/NRBPerdijk/example-graalvm-r-scala.git
   >```
   
c. Run sbt

   ![user input](images/userinput.png)
   >```sh
   >sbt run
   >``` 
   
d. See Output 
   >```sh
   >example-graalvm-r-scala-master adhillon$ sbt run
   >[info] Updating ...
   >[info] Done updating.
   >[info] Compiling 1 Scala source to /Users/adhillon/Desktop/InJapan/GraalVM-latest/example-graalvm-r-scala 
   >[info] Done compiling.
   >[info] Packaging /Users/adhillon/Desktop/InJapan/GraalVM-latest/example-graalvm-r-scala-master/target/scala-2.12/example- graalvm-r-scala-master_2.12-0.1.0-SNAPSHOT.jar ...
   >[info] Done packaging.
   >[info] Running Main 
   >[1] "Hello, World!"
   >Also printing the return String: Hello, World! executed in R
   >[success] Total time: 20 s, completed 21 Dec, 2019 12:26:15 PM
   >```

### Conclusions

You have seen GraalVM in action with Scala, Please provide us feedback!

## Appendix


