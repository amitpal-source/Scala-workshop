Table of Contents:

* [Exercise 0: Connect to your Virtual Machine](#exercise-0-connect-to-your-virtual-machine)
* [Exercise 1: GraalVM](#exercise-1-graalvm)
* [Exercise 2: Microservices](#exercise-2-microservices)
* [Exercise 3: SpringBoot](#exercise-3-springboot)
* [Appendix: VNC](#appendix-vnc)

# Exercise 0: Connect to your Virtual Machine

> As you make your way through this lab, look out for this icon.
![user input](images/userinput.png) Whenever you see it, it's time for you to
perform an action.

To save you the trouble of having to install all of the software we're working
with in this lab on your laptop, we've set up OCI virtual machines in the cloud
for you to work with. The first step is connecting to your virtual machine via
SSH.

a. Ensure you are connected to the internet.
b. ![user input](images/userinput.png) Download this [SSH key](https://raw.githubusercontent.com/ostrain/graalvm-fn/master/key.txt) to your Desktop, or copy it into a file on
   your desktop from here:
   ```
   -----BEGIN RSA PRIVATE KEY-----
   MIIEowIBAAKCAQEAywdqSGo9GT71U2Hxg72sxY/Fpm2UlGnxFHeqsENwafujOFI1
   QXWqwyCI1ysdeOWtcASaGEh0Or72dGbBdAak2vsiwJzgQC924S/0eXsM5B0utte1
   hN91Bkuuv6OZgGUNjHr4zNem02YLyu4AH+G62bXJmpw7OJFjx2xK7ss/ncziLp3C
   c0nMOgPdxz2ZW26yZN9OgdfMle7bV92syaUgCm9SJVEDp47PEaDI6pR2dKzu14AT
   4l8DkWqv/G77lLpwRpD0pFNrLRITRYadEu1HmEBYB4sm4T2roUiQGtdyyAZReJdF
   qr+l2laVgqaY35LEht6JyrBVEU5T8/IJSOHw0wIDAQABAoIBAB/3r+1tBOfHuPsD
   DfMPV3fX9mgJEv73W2U8nlyYkceuPnnsv8Pp0hRdOA6hFEfn6hIcN2MhbWOek9Gq
   KEWrkiOErWkkik6w6of0dAr4rAPy8FsLxeIBCT0Ph5lXGiFUR/jZl/kw72yTwcGX
   Dd/3O+Nxsyer98slYU+SJo5I5GBlDVZJzmbKJM95hwq9J/Jhk5oU5+Xb0YKG1fJF
   tnEXIi/Zmi1qMz2WlVxw5ijro2c9xHrIdD75Cl7p6FFOPR/dGw/0yfH1AKnnLoDu
   fh/dnUigZUqHY5oh5+xUiYT7qYME2gBbRq99eTwD95Ri7Mv+s/62isDnQHIqLzYC
   SBgHWGkCgYEA69WTfB0VlMrcl2p095EanGJHGFKrtPIXGQ99jdfgCHsbmgD9ClxY
   iYxwonJRojBdfS1FveZHinpIJkM6Io+ikZMtRwDCo2NaSBcPpgsWubkM6d81ozRR
   tVA3D3fRxxmw6xzO+gb6OzOmniu43oFx3ysZURPS6WlfkAoQRN4rayUCgYEA3GO6
   pZZFZN5DRV0NPE9QfTjwCN1ioyexJR4V04Ge55rxU+PQ99XYy0NKO+isgXvHM7qg
   xReF3N8o6HZVXHQOccoXyowZEEGY0ov+RQt3dYdWNslf5+oDQY4pGWUx0VLUl9Jc
   U868V+7szpH8QuQ05FS8ejKvPjiZz7FhZ6yxZpcCgYAkziw6Tn+zvQU4TFD84hR5
   G41k6mIJ105rhtNdc7dvG2dvXYAgQdE/hj992sGKwmmUw6ACxxGbNwJTlmJYSnpg
   pcuHLUMzJKpOXer/SvO9AHhXd5JzahLmbSJqs5R1sji+OLzEoJok6yaxwLkVYLZY
   0e0dji54Zw2W+TsYxGOaUQKBgQC5SPcQY6fvx5US6cpI2/21aXsUWNumg6ZAqGx9
   J0qrOO6PMsbqtdTP/sCYYWM2cRPmf47OZJpcORmxZlsZoVDLhbY+yU4BBsUurheV
   gLBsFN38rYx985XNNXGEMjmR3GtzQJ3yr6pU20An40AMFMQK+tqCeb4vN3LI1D4p
   5Vw3/QKBgE9omsh+VXJc6zPVziDop4QEXXAksRd2YeDYpUE9MBnGmZXnOb0TiVn6
   DlvkbMBfaPucKAfZwKIuhfmu7Sf4J6aqKLzaDrAIw7Dzh6i0GAWj+N7i7nb8Fpzv
   u2cEAy1Je+k77/+5CSCO1yBSX5Eohy9PBfxI2gmtQ3G5TgJ/pp/y
   -----END RSA PRIVATE KEY-----
   ```
c. Open a terminal and modify the permissions of the downloaded key:

   ![user input](images/userinput.png)
   >```sh
   >chmod 400 ~/Desktop/key.txt
   >```
d. Connect to your assigned VM via its IP address:

   ![user input](images/userinput.png)
   >```sh
   >ssh -i ~/Desktop/key.txt -L 3000:localhost:3000 opc@<IP address>
   >```

   Note that we're also setting up SSH port-forwarding on port 3000 to your VM.
   You will use this later to access one of the examples using the your browser
   on your laptop.

# Exercise 1: GraalVM

There are a lot of different parts to [GraalVM](https://www.graalvm.org/), so
while you may have heard of it, there are almost certainly things that it can do
that you don't know about yet. In this exercise we'll go through some of the
diverse features of GraalVM and show you what they can do for you.

## 0. Setup

In this exercise, we'll be using materials from the following article:
https://medium.com/graalvm/graalvm-ten-things-12d9111f307d

You may want to clone the code onto your lab VM to help you follow along:

![user input](images/userinput.png)
>```sh
>git clone https://github.com/chrisseaton/graalvm-ten-things.git
>```

You have connected to the OCI Developer Cloud Image which contains the
Enterprise Edition of GraalVM 19.0.0 This gives us the Java and JavaScript
languages.

To ensure that the Graal versions will be used by default when invoking `java`,
`js`, or `node`, update your `$PATH` as follows:

![user input](images/userinput.png)
>```sh
>export PATH=/opt/graalvm/graalvm-ee-19.0.0/bin:$PATH
>```

You can verify that the Graal versions will be used with the following commands:

![user input](images/userinput.png)
>```sh
>java -version
>```

Should output:

```
java version "1.8.0_212"
Java(TM) SE Runtime Environment (build 1.8.0_212-b31)
Java HotSpot(TM) GraalVM EE 19.0.0 (build 25.212-b31-jvmci-19-b01, mixed mode)
```

Should output:

![user input](images/userinput.png)
```sh
js --version
```

Should output:

```
GraalVM JavaScript (GraalVM EE Native 19.0.0)
```

![user input](images/userinput.png)
```sh
node --version:graalvm
```

```
GraalVM Polyglot Engine Version 19.0.0
GraalVM Home /opt/graalvm/graalvm-ee-19.0.0
  Installed Languages:
    JavaScript version 19.0.0
    LLVM       version 19.0.0
    Python     version 3.7.0
    Ruby       version 2.6.2
  Installed Tools:
    CPU Sampler             version 0.4.0
    CPU Tracer              version 0.3.0
    Heap Allocation Monitor version 0.1.0
    Chrome Inspector        version 0.1
    Memory Tracer           version 0.2
```

## 1. High-performance modern Java

The *Graal* name in the GraalVM comes from the GraalVM compiler. GraalVM is
[one compiler to rule them
all](http://lafo.ssw.uni-linz.ac.at/papers/2013_Onward_OneVMToRuleThemAll.pdf),
meaning that it's a single implementation of a compiler written as a library
which can be used for many different things. For example we use the GraalVM
compiler to compile both *ahead-of-time* and *just-in-time*, to compile
multiple programming languages, and to multiple architectures.

One simple way to use GraalVM is to use it as your Java JIT compiler.

We'll use this example program, which gives you the top-ten words in a document.
It uses modern Java language features like streams and collectors.

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.Arrays;
import java.util.function.Function;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class TopTen {

    public static void main(String[] args) {
        Arrays.stream(args)
                .flatMap(TopTen::fileLines)
                .flatMap(line -> Arrays.stream(line.split("\\b")))
                .map(word -> word.replaceAll("[^a-zA-Z]", ""))
                .filter(word -> word.length() > 0)
                .map(word -> word.toLowerCase())
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
                .entrySet().stream()
                .sorted((a, b) -> -a.getValue().compareTo(b.getValue()))
                .limit(10)
                .forEach(e -> System.out.format("%s = %d%n", e.getKey(), e.getValue()));
    }

    private static Stream<String> fileLines(String path) {
        try {
            return Files.lines(Paths.get(path));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

}
```

GraalVM includes a `javac` compiler, but it isn't any different from the
standard one for the purposes of this demo, so you could use your system `javac`
instead if you wanted to.

![user input](images/userinput.png)
>```sh
> javac TopTen.java
>```

If we run the `java` command included in GraalVM we'll be automatically using
the GraalVM JIT compiler - no extra configuration is needed. I'll use the `time`
command to get the real, wall-clock elapsed time it takes to run the entire
program from start to finish, rather than setting up a complicated
micro-benchmark, and I'll use a large input so that we aren't quibbling about a
few seconds here or there. The `large.txt` file is 150 MB.

![user input](images/userinput.png)
> ```sh
> make large.txt
> ```

![user input](images/userinput.png)
>```sh
> time java TopTen large.txt
>```

```
sed = 502500
ut = 392500
in = 377500
et = 352500
id = 317500
eu = 317500
eget = 302500
vel = 300000
a = 287500
sit = 282500

real	0m21.411s
user	0m30.780s
sys	0m1.224s
```

GraalVM is written in Java, rather than C++ like most other JIT compilers for
Java. We think this allows us to improve it more quickly than existing
compilers, with powerful new optimisations such as partial escape analysis that
aren't available in the standard JIT compilers for HotSpot. This can make your
Java programs run significantly faster.

To run without the GraalVM JIT compiler to compare, we can use the flag
`-XX:-UseJVMCICompiler`. JVMCI is the interface between GraalVM and the JVM. You
could also compare against your standard JVM as well.

![user input](images/userinput.png)
>```sh
> time java -XX:-UseJVMCICompiler TopTen large.txt
>```

```
sed = 502500
ut = 392500
in = 377500
et = 352500
id = 317500
eu = 317500
eget = 302500
vel = 300000
a = 287500
sit = 282500

real	0m32.080s
user	0m32.719s
sys	0m0.490s
```

This shows GraalVM running our Java program in around two-thirds of the
wall-clock (`real`) time it takes to run it with a standard HotSpot compiler. In
an area where we are used to treating single-digit percentage increases in
performance as significant, this is a big-deal.

You'll still get a result better than HotSpot if you use the Community
Edition, but it won't be quite as a good as the Enterprise Edition.

Twitter are one company [using GraalVM in production
today](https://www.youtube.com/watch?v=OSyvidFXL7M), and they say that for them
it is paying off in terms of real money saved. Twitter are using GraalVM to run
Scala applications - GraalVM works at the level of JVM bytecode so it works for
any JVM language.

This is the first way you can use GraalVM - simply as a drop-in better JIT
compiler for your existing Java applications.

## 2. Low-footprint, fast-startup Java

The Java platform is particularly strong for long-running processes and peak
performance, but short-running processes can suffer from longer startup time and
relatively high memory usage.

For example, if we run the same application with a much smaller input - around 1
KB instead of 150 MB, then it seems to take an unreasonably long time, and quite
a lot of memory at 70 MB, to run for such a small file. We use `-l` to print the
memory used as well as time used.

![user input](images/userinput.png)
>```sh
> make small.txt
>```

![user input](images/userinput.png)
>```sh
> /usr/bin/time -v java TopTen small.txt   # -l on Mac instead of -v
>```

```
sed = 6
sit = 6
amet = 6
mauris = 3
volutpat = 3
vitae = 3
dolor = 3
libero = 3
tempor = 2
suscipit = 2
	Command being timed: "java TopTen small.txt"
	User time (seconds): 0.39
	System time (seconds): 0.04
	Percent of CPU this job got: 168%
	Elapsed (wall clock) time (h:mm:ss or m:ss): 0:00.26
	Average shared text size (kbytes): 0
	Average unshared data size (kbytes): 0
	Average stack size (kbytes): 0
	Average total size (kbytes): 0
	Maximum resident set size (kbytes): 76116
...
```

GraalVM gives us a tool that solves this problem. We said that GraalVM is like a
compiler library and it can be used in many different ways. One of those is to
compile *ahead-of-time*, to a native executable image, instead of compiling
*just-in-time* at runtime. This is similar to how a conventional compiler like
`gcc` works.

![user input](images/userinput.png)
>```sh
> native-image --no-server TopTen
>```

```
[topten:37970]    classlist:   1,801.57 ms
[topten:37970]        (cap):   1,289.45 ms
[topten:37970]        setup:   3,087.67 ms
[topten:37970]   (typeflow):   6,704.85 ms
[topten:37970]    (objects):   6,448.88 ms
[topten:37970]   (features):     820.90 ms
[topten:37970]     analysis:  14,271.88 ms
[topten:37970]     (clinit):     257.25 ms
[topten:37970]     universe:     766.11 ms
[topten:37970]      (parse):   1,365.29 ms
[topten:37970]     (inline):   3,829.55 ms
[topten:37970]    (compile):  34,674.51 ms
[topten:37970]      compile:  41,412.71 ms
[topten:37970]        image:   2,741.41 ms
[topten:37970]        write:     619.13 ms
[topten:37970]      [total]:  64,891.52 ms
```

This command produces a native executable called `topten`. This executable isn't
a launcher for the JVM, it doesn't link to the JVM, and it doesn't bundle the
JVM in any way. `native-image` really does compile your Java code, and any Java
libraries you use, all the way down to simple machine code. For runtime
components like the garbage collector we are running our own new VM called the
SubstrateVM, which like GraalVM is also written in Java.

If we look at the libraries which `topten` uses you can see they are only
standard system libraries. We could also move just this one file to a system
which has never had a JVM installed and run it there to verify it doesn't use a
JVM or any other files. It's also pretty small - this executable is less than 8
MB.

![user input](images/userinput.png)
>```sh
> ldd topten    # otool -L topten on Mac
>```

```
	linux-vdso.so.1 =>  (0x00007ffe1555b000)
	libm.so.6 => /lib64/libm.so.6 (0x00007f6bda7c6000)
	libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f6bda5aa000)
	libdl.so.2 => /lib64/libdl.so.2 (0x00007f6bda3a6000)
	libz.so.1 => /lib64/libz.so.1 (0x00007f6bda190000)
	librt.so.1 => /lib64/librt.so.1 (0x00007f6bd9f88000)
	libcrypt.so.1 => /lib64/libcrypt.so.1 (0x00007f6bd9d51000)
	libc.so.6 => /lib64/libc.so.6 (0x00007f6bd9983000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f6bdaac8000)
	libfreebl3.so => /lib64/libfreebl3.so (0x00007f6bd9780000)
$ du -h topten 
7.2M  topten
```

If we run the executable we can see that it starts around an order of magnitude
faster, and uses around an order of magnitude less memory, than running the same
program on the JVM does. It's so fast that you don't notice the time taken when
using it at the command line - you don't feel that pause you always get when
running a short-running command with the JVM.

![user input](images/userinput.png)
>```sh
> /usr/bin/time -v ./topten small.txt
>```

```
sed = 6
sit = 6
amet = 6
mauris = 3
volutpat = 3
vitae = 3
dolor = 3
libero = 3
tempor = 2
suscipit = 2
	Command being timed: "./topten small.txt"
	User time (seconds): 0.00
	System time (seconds): 0.00
	Percent of CPU this job got: 50%
	Elapsed (wall clock) time (h:mm:ss or m:ss): 0:00.00
	Average shared text size (kbytes): 0
	Average unshared data size (kbytes): 0
	Average stack size (kbytes): 0
	Average total size (kbytes): 0
	Maximum resident set size (kbytes): 6192
...
```

The `native-image` tool has some
[restrictions](https://github.com/oracle/graal/blob/master/substratevm/LIMITATIONS.md)
such as all classes having to be available during compilation, and some
limitations around reflection. It has some additional advantages over basic
compilation as well in that static initializers are run during compilation, so
you can reduce the work done each time an application loads.

This is a second way that you can use GraalVM -- as a way to distribute and run
your existing Java programs with a low-footprint and fast-startup. It also frees
you from configuration issues such as finding the right jar files at runtime,
and allows you to have smaller Docker images.


## 3. Polyglot: Combine JavaScript, Java, and R

As well as Java, GraalVM includes new implementations of JavaScript, Ruby, R and
Python. These are written using a new language implementation framework called
*Truffle* that makes it possible to implement language interpreters that are
both simple and high performance. When you write a language interpreter using
Truffle, Truffle will automatically use GraalVM on your behalf to give you a JIT
compiler for your language. So GraalVM is not only a JIT compiler and
ahead-of-time native compiler for Java, it can also be a JIT compiler for
JavaScript, Ruby, R and Python.

The languages in GraalVM aim to be drop-in replacements for your existing
languages. For example we can install a Node.js module:

![user input](images/userinput.png)
>```sh
> npm install color
>```

```
...
+ color@3.1.1
added 6 packages from 6 contributors and audited 7 packages in 6.931s
```

We can write a little program using this module to convert an RGB HTML color to
HSL:

```javascript
var Color = require('color');

process.argv.slice(2).forEach(function (val) {
  console.log(Color(val).hsl().string());
});
```

Then we can run that in the usual way:

![user input](images/userinput.png)
>```sh
> node color.js '#42aaf4'
>```

```
hsl(204.89999999999998, 89%, 60.8%)
```

The languages in GraalVM work together - there's an API which lets you run code
from one language in another. This lets you write polyglot programs - programs
written in more than one language.

You might want to do this because you want to write the majority of your
application in one language, but there's a library in another language's
ecosystem that you'd like to use. For example, JavaScript doesn't have a great
solution for arbitrarily-large integers. I found several modules like
`big-integer` but these are all inefficient as they store components of the
number as JavaScript floating point numbers. Java's `BigInteger` class is more
efficient so let's use that instead to do some arbitrarily-large integer
arithmetic.

JavaScript also doesn't include any built-in support for drawing graphs, where R
does include excellent support for this. Let's use R's `svg` module to draw a
3D scatter plot of a trigonometric function.

In both cases we can use GraalVM's polyglot API, and we can just compose the
results from these other languages into JavaScript.

First, let's install the express npm package:

![user input](images/userinput.png)
>```sh
> npm install express
>```

Next, let's run the following program:

![user input](images/userinput.png)
```js
const express = require('express')
const app = express()

const BigInteger = Java.type('java.math.BigInteger')

app.get('/', function (req, res) {
  var text = 'Hello World from Graal.js!<br> '

  // Using Java standard library classes
  text += BigInteger.valueOf(10).pow(100)
          .add(BigInteger.valueOf(43)).toString() + '<br>'

  // Using R interoperability to create graphs
  text += Polyglot.eval('R',
    `svg();
     require(lattice);
     x <- 1:100
     y <- sin(x/10)
     z <- cos(x^1.3/(runif(1)*5+10))
     print(cloud(x~y*z, main="cloud plot"))
     grDevices:::svg.off()
    `);

  res.send(text)
})

app.listen(3000, function () {
  console.log('Example app listening on port 3000!')
})
```

![user input](images/userinput.png)
>```sh
> node --jvm --polyglot polyglot.js
>```

Open http://localhost:3000/ in your browser to see the result.

![polyglot.js](images/polyglot.png)

That's the third thing we can do with GraalVM - run programs written in multiple
languages and use modules from those languages together. We think of this as a
kind of commoditisation of languages and modules - you can use whichever
language you think is best for your problem at hand, and whichever library you
want, no matter which language it came from.

# Exercise 2: Microservices: Creating your first Micronaut GraalVM application

![micronaut](images/micronaut.png)

Next we'll learn how to create a Hello World Micronaut Graal application. To get started, clone the git repository:

![user input](images/userinput.png)
>```sh
> git clone https://github.com/micronaut-guides/micronaut-creating-first-graal-app.git
>```

Change directory to the `complete` subdirectory within the cloned repo:

>```sh
> cd micronaut-creating-first-graal-app/complete
>```

## 0. Creating native image inside Docker

With this approach you only need to build the fatjar and then use Docker to build the native image.

Build the Graal fatjar:

![user input](images/userinput.png)
>```sh
> ./gradlew assemble
>```

Then build a docker image from it:

![user input](images/userinput.png)
>```sh
> ./docker-build.sh
>```

The previous command will create the image micronaut-graal-app:latest. To execute it:

Execute the native image:

![user input](images/userinput.png)
>```sh
> docker run -p 3000:8080 --name=micronaut micronaut-graal-app &
>```

```
10:29:46.845 [main] INFO  io.micronaut.runtime.Micronaut - Startup completed in 12ms. Server Running: http://localhost:8080
```

We can see that the application starts in only 12ms in this example (actual time
will vary).

## 1. SetupSending a request

From another terminal, you can run a few cURL requests to test the application:

![user input](images/userinput.png)
>```sh
> time curl localhost:3000/conferences/random
>```

```
{"name":"Greach"}
real    0m0.016s
user    0m0.005s
sys     0m0.004s
```

Finally, kill the docker container:

![user input](images/userinput.png)
>```sh
> docker kill micronaut
>```

![user input](images/micronaut-startup.png)

![user input](images/micronaut-memory.png)


# Exercise 3: SpringBoot

This lab will focus on SpringBoot

First clone the sameple springboot application
>git clone https://github.com/spring-projects-experimental/spring-graal-native.git
>cd spring-graal-native
>./mvnw clean package
>cd spring-graal-native-samples/spring-petclinic-jpa/
>./compile.sh (takes a long time and require a lot of memory, it is not yet optimized)
>./petclinic (first run can be slow like 1s, next ones are less than 0.2s, again it is not optimized yet)

Go to http://localhost:8080/
Click on find owners menu
Click on find owners button to see the list of owners
Clock on veterinarians menu

### Conclusions

You have seen GraalVM in action, Microservices with GraalVM and also how a SpringBoot application works with GraalVM


## Appendix: VNC

If you'd like to connect to your VM graphically, follow these steps.

While connected to your VM via SSH, start the VNC server as follows:

![user input](images/userinput.png)
>```sh
>vncserver
>```

Set a password when prompted.

To connect graphically to the VNC server, you'll need a VNC client on your
laptop.  You can use whatever you have previously installed or you can use the
VNC Viewer for Chrome that is extremely easy to install.

https://chrome.google.com/webstore/detail/vnc%C2%AE-viewer-for-google-ch/iabmpiboiopbgfabjmgeedhcmjenhbla/related

![user input](images/userinput.png) Log into your VM using the provided IP
Address.  The VNC port is **5901** so the server address you'll need
to provide will look like **`n.n.n.n:5901`**

When prompted, enter the VNC password you set earlier.
