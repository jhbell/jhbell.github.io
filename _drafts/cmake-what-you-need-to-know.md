---
layout: post
title:  "CMake: What You Need to Know - Part 1"
---
<img src="{{ site.url }}/assets/jeff-web.jpg" 
     alt="Jeff Bell" 
     style="width: 200px; height: 200px; padding-bottom: 25px" />  
In my [last post][last-post] I talked about some recent discoveries I've had
with using [CMake][cmake]. I decided today I would take the time to talk a
little about what I learned. This will be the first part of a few posts that
will walk through one way I have found to use CMake in a logical, and helpful
manner.

# Rationale

There are plenty of people that talk about CMake, but when I was learning how
to use it I struggled to find a *free*, cohesive source on building projects 
with CMake. This post is my attempt to create some helpful information on 
getting started with CMake, and some clean ways I have discovered to use it.

# What is CMake?

Well, according to their [website][cmake]:

> CMake is an open-source, cross-platform family of tools designed to build, 
> test and package software. CMake is used to control the software compilation 
> process using simple platform and compiler independent configuration files, 
> and generate native makefiles and workspaces that can be used in the compiler
> environment of your choice. The suite of CMake tools were created by Kitware
> in response to the need for a powerful, cross-platform build environment for
> open-source projects such as ITK and VTK.

Great, so what does that mean to the average user and why would you want to
use CMake?

* CMake is a Makefile generator (or Makefile equivalent).
* CMake is platform-independent.
* CMake _should_ make your life easier.

**CMake is a makefile generator.** When you use CMake, it will automatically
generate a Makefile (or Makefile equivalent on non \*NIX systems) which you
can use to compile your program. *But why would I do that when I can just write
a Makefile?*

**CMake is platform-independent.** The reason you would want to use CMake to 
generate your Makefiles is that you no-longer have to define specifics in your 
Makefile for different compilers and or operating systems. You can write out
the things you would like to do, and let CMake handle all of the 
platform-dependent nonsense.

**CMake *should* make your life easier.** I say *should* because there's a lot
of hoopla surrounding CMake and how to use it. The 
[CMake Tutorial][cmake-tutorial] helps with a few specific cases, but doesn't
do anything to teach you how to actually use the software. Once you figure out
how to do what you want, however, CMake is a very valuable tool. CMake can
make building and, even testing, a breeze and has some pretty sweet features.

# Basic CMake Usage

The complete source code for the following HelloSimple program can be found in 
my GitHub repository: [http://github.com/jhbell/cmake-wyntk][cmake-wyntk]

CMake is run using files called `CMakeLists.txt`. Inside of these files is
where you will specify the configuration options that you would normally use
in a Makefile. Furthermore, CMake allows for the use of multiple 
`CMakeLists.txt` files to allow for better organization. Here is an example of 
_the simplest CMake project you can make_.

```
HelloSimple
|-- CMakeLists.txt
|-- Hello.cpp
|-- Hello.h
|-- RunHello.cpp
```

Imagine that `RunHello.cpp` uses `Hello.h` and `Hello.cpp` to print "Hello,
world!" to standard output. We have written the code necessary to do so, and
now we are ready to compile our program.

We will start by creating our `CMakeLists.txt` file containing the following
lines:

```cmake
cmake_minimum_required(VERSION 3.5)
project(HelloSimple CXX)
set(CMAKE_CXX_STANDARD 14)
add_executable(RunHello RunHello.cpp Hello.cpp)
```

Notice that each line looks like a call to a function. These "functions" can
all be found in the [CMake documentation][cmake-commands], and are referred to
as "commands".  Let's take a look at what each one of these commands means, 
starting with the first line:

```cmake
cmake_minimum_required(VERSION 3.5)
```

As you can imagine, here we define the *minimum required version of CMake* we
need to build this project as version 3.5. Again, as you would think, you can 
change the version number (here 3.5) to be whatever version you need to support 
the CMake commands you will be using.

```cmake
project(HelloSimple CXX)
```

This command specifies the *name* of the project, and the *language* of the
project. I decided to go with the project name of `HelloSimple`. Since I am
using C++, I wanted to specify that the language as being C++ by putting
CXX as the next parameter.

```cmake
set(CMAKE_CXX_STANDARD 14)
```

The set command will set a *variable* to a given *value*. In this case, we are
setting one of CMake's known variables called `CMAKE_CXX_STANDARD`. This
variable controls the C++ standard that should be used when compiling, and is
equivalent to using the `-std=c++14` flag on g++. There are other ways to
specify the C++ standard, but this one I have found to be the most concise.
The values 98, 11, or 14 can be used.

```cmake
add_executable(RunHello RunHello.cpp Hello.cpp)
```

The last line specifies the *name of the executable* to be created (-o flag on
gcc/g++) and the *list of source files to compile*. Here we are naming the
executable RunHello, and compiling the .cpp files `RunHello.cpp` and 
`Hello.cpp` for the HelloSimple program.

# Building and Running with CMake

Building your CMake project is now quite simple. Again, there are various ways
to accomplish this task, but the following is what I have found to be easiest
to remember.

1. *Make a new directory* inside of your project folder. I, personally, like
using the name `build` for this. Once this is done, the directory structure
from before should look like this:

```
HelloSimple
|-- build/              <= New!
|-- CMakeLists.txt
|-- Hello.cpp
|-- Hello.h
|-- RunHello.cpp
```

2. *Change directories* into the newly created directory.

3. *Execute the command `cmake ..`.* The CMake command line tool simply
requires a path to the project source, or a path to a previous build. We
obviously don't have a previous build, so we want to pass in the root directory
for our project containing our highest level `CMakeLists.txt` file. This just
so happens to be `..`.

4. *Run make* in the build directory. The `cmake` command generates a 
`Makefile` into your current directory. You will also notice a file called 
`CMakeCache.txt` and a directory called `CMakeFiles`. The `CMakeCache.txt` file 
is an editable file containing some defaults for your program's build process.
You can open this file to learn more about it. The `CMakeFiles` directory 
contains all of the sorcery done by CMake along with the binaries for
each source file.

5. *Run your program* as you normally would after a make. In this example, 
I simply have to type `./RunHello` since RunHello is the name I gave to my
executable using the `add_exectuable()` command above.

From here on out, to recompile your program, you simply need to run `make`
again. Only after making changes to the CMakeLists.txt file do you need to
run the `cmake` command again.

In summary, the commands to execute this build process from the HelloSimple
directory are:

```
mkdir build
cd build
cmake ..
make
./RunHello
```

# Closing Remarks

As you can see, there is a lot to learn about CMake. Similar to using an IDE,
the more you let the software do, the less you know about what is actually
going on. If you want a platform independent build process, look no further, 
this is about as simple as you can get. CMake is growing in popularity and is
even the basis for JetBrains' [CLion][clion] IDE. If you already have a ton of 
Makefiles that do exactly what you want, however, maybe switching your entire 
project over to CMake isn't necessary.

In some future posts, I will go over creating more elaborate
project setups with CMake where we will begin to see some of the features that
makes CMake much easier to use than Makefiles. As we have already seen, we can
use CMake to generate a fully equipped Makefile with just four commands. The
future posts will go into more complex directory structures, including
libraries, and testing with GoogleTest. Until then, happy hacking!


[cmake]:          https://cmake.org
[cmake-tutorial]: https://cmake.org/cmake-tutorial/
[cmake-commands]: https://cmake.org/cmake/help/v3.8/manual/cmake-commands.7.html
[clion]:          https://www.jetbrains.com/clion/

[cmake-wyntk]:  https://github.com/jhbell/cmake-wyntk
[last-post]:    {{ site.baseurl }}{% post_url 2017-06-11-welcome-to-my-blog %}
