---
layout: post
title:  "Using CMake with Gcov"
---
<img src="{{ site.url }}/assets/jeff-web.jpg" 
     alt="Jeff Bell" 
     style="width: 200px; height: 200px; padding-bottom: 25px" />  
I recently tried using [CMake][cmake] to add gcov to the testing script. As I 
learned, this is much more difficult than using a Makefile. This post covers 
what I came up with.

# What is Gcov?
`gcov` is a tool to check test coverage. `gcov` records a run of your program and 
will measure the lines of code that are executed. This allows you to see how 
well your tests cover the code you have written. For a more detailed
description on `gcov`, checkout this [introduction][gcov-intro] on the GNU
website.

# What is CMake?
If you haven't seen my [previous post][part-1] on an introduction to CMake,
check it out. In a nutshell, CMake is a tool/language for cross-platform
software builds in various languages. CMake attempts to remove some of the
uncertainties that come with using Makefiles. For example, if you wanted
to locate a library on the system for linking etc. CMake has a single-line 
command to do it. Read more about it [here][cmake].

# Rationale
When using a coverage tool alongside a testing framework, it is very easy to
see how much of your code is executed when you run your tests. This allows
you to see if there are holes in your tests and, to a further extent, where
the holes are.

The goal of this project was to use CMake to build a simple program and run 
a few tests. Then, create a target where we can say `make gcov` to run `gcov` 
on our program and output the coverage data.

# Putting It All Together
Let's take a look at how this example project works. If you'd like to follow 
along, you can check out the project source [here][cmake-gcov].

To show that `gcov` is working, I created the simple `Adder` class. An `Adder`
contains a value that you can add numbers to using the `add()` method. You
can also print the current value of the `Adder` using the `print_value()`
method and reset the value to zero using the `clear()` method. Once again, to
see the full project source, check it out on [GitHub][cmake-gcov].

I created the following test program for this class:

```cpp
// RunAdder.cpp
#include <iostream>
#include "Adder.h"

int main() {
    Adder adder;
    adder.print_value(std::cout);
    adder.add(5);
    adder.print_value(std::cout);
    adder.add(5);
    adder.print_value(std::cout);

    return 0;
}
```

When run, this program outputs the following:

```
Current value: 0
Current value: 5
Current value: 10
```

Lastly, my `CMakeLists.txt` file looks like this:

```cmake
cmake_minimum_required(VERSION 3.5)
project(CMake_GCov CXX)

# Set the compiler options
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_FLAGS "-g -O0 -Wall -fprofile-arcs -ftest-coverage")
set(CMAKE_CXX_OUTPUT_EXTENSION_REPLACE ON)

# Create OBJECT_DIR variable
set(OBJECT_DIR ${CMAKE_BINARY_DIR}/CMakeFiles/RunAdder.dir)
message("-- Object files will be output to: ${OBJECT_DIR}")

# Set the sources
set(SOURCES
    RunAdder.cpp
    Adder.cpp
   )

# Create the executable
add_executable(RunAdder ${SOURCES})

# Create the gcov target. Run coverage tests with 'make gcov'
add_custom_target(gcov
    COMMAND mkdir -p coverage
    COMMAND ${CMAKE_MAKE_PROGRAM} test
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    )
add_custom_command(TARGET gcov
    COMMAND echo "=================== GCOV ===================="
    COMMAND gcov -b ${CMAKE_SOURCE_DIR}/*.cpp -o ${OBJECT_DIR}
    | grep -A 5 "Adder.cpp" > CoverageSummary.tmp
    COMMAND cat CoverageSummary.tmp
    COMMAND echo "-- Coverage files have been output to ${CMAKE_BINARY_DIR}/coverage"
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/coverage
    )
add_dependencies(gcov RunAdder)
# Make sure to clean up the coverage folder
set_property(DIRECTORY APPEND PROPERTY ADDITIONAL_MAKE_CLEAN_FILES coverage)

# Create the gcov-clean target. This cleans the build as well as generated 
# .gcda and .gcno files.
add_custom_target(scrub
COMMAND ${CMAKE_MAKE_PROGRAM} clean
COMMAND rm -f ${OBJECT_DIR}/*.gcno
COMMAND rm -f ${OBJECT_DIR}/*.gcda
WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
)

# Testing
enable_testing()

add_test(output_test ${CMAKE_CURRENT_BINARY_DIR}/RunAdder)
set_tests_properties(output_test PROPERTIES PASS_REGULAR_EXPRESSION "0;5;10")
```

Here are some things to note about the above CMakeLists.txt. The first is that
the `gcov` target is what will do everything we need to show us results
on our code coverage. The COMMAND to run gcov is possible thanks to the
`-fprofile-arcs -ftest-coverage` compile flags. Also note that the `scrub`
target will clean up the generated .gcno and .gcda files. Unfortunately, I
couldn't find a much better way to clean up these files since they are buried
in the CMakeFiles directory.

All .gcov files and results are output into a `coverage` directory located in
the project binary directory. I decided to go with this solution because it 
allowed for easy cleanup by adding just the `coverage` directory to the list of
additional make clean files.

Lastly, a call to `make gcov` will automatically build the project, and run
any tests added using the `add_test` command. For this project, I created a 
very simple test that checked to see the correct three numbers are output:
0, 5 and 10. As long as these three numbers are output, the test will pass.

```cmake
set_tests_properties(output_test PROPERTIES PASS_REGULAR_EXPRESSION "0;5;10")
```

# Building and Testing

As usual, I can perform an out-of-source build using the following commands

```bash
$ mkdir build
$ cd build
$ cmake ..
```

Now that the `Makefile` has been generated, we can run coverage tests by simply
saying `make gcov`. When I run `make gcov`, I get something like the following
as output:

```
[ 33%] Building CXX object CMakeFiles/RunAdder.dir/RunAdder.o
[ 66%] Building CXX object CMakeFiles/RunAdder.dir/Adder.o
[100%] Linking CXX executable RunAdder
[100%] Built target RunAdder
Running tests...
Test project cmake-gcov/build
Start 1: output_test
1/1 Test #1: output_test ......................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.01 sec
=================== GCOV ====================
File 'cmake-gcov/Adder.cpp'
Lines executed:71.43% of 7
No branches
No calls
cmake-gcov/Adder.cpp:creating 'Adder.cpp.gcov'

File 'cmake-gcov/RunAdder.cpp'
Lines executed:100.00% of 7
No branches
No calls
cmake-gcov/RunAdder.cpp:creating 'RunAdder.cpp.gcov'

-- Coverage files have been output to cmake-gcov/build/coverage
[100%] Built target gcov
```

Notice that only about 71% of the lines of Adder.cpp are executed. This is
because the `clear()` method is never used in our test. If we were to add a
call to `clear()` in `RunAdder.cpp`, this value goes back up to 100%. If you 
don't trust me on that, try it yourself!

If I want to run a new coverage test, I can run:

```bash
$ make scrub
$ make gcov
```

`make scrub` MUST be run before running `make gcov` again. If `make scrub` is
not run, the coverage results will be incorrect due to the .gcno and .gcda
files being updated, rather than regenerated. This can be seen easily by
running `make gcov` twice and looking at `coverage/Adder.cpp.gcov` after each
run. The total number of runs will continue to increase for each line of code.
These numbers will not be reset until you run `make scrub` to clean the .gcda 
and .gcno files.

# Closing Remarks

And there you have it, folks! They said it couldn't be done, but here you are
using CMake and gcov at the same time. This project took way longer than I was
expecting, especially considering how few lines I ended up with.

The largest problem I was unable to find a solution for was how to delete the 
.gcno and .gcda files automatically when running the gcov target. I couldn't 
find a way to clean up the old ones automatically before building the project. 
Just about any way I tried it, the project would be built, and the newly 
generated profiling files would be deleted. I was unable to clean before
building. I suspect part of this comes from using the `add_dependencies()`
CMake command to build the project before analyzing the coverage.

The amount of time I had to spend on tiny details like this not working 
make me want to run away from CMake and never look back. Overall, I am happy
with how this turned out, but I wish that I could make it just a touch more
elegant. Integrating tools like this really shows off how much more powerful
and straight-forward GNU Makefiles are.

In the future, I intend to integrate this with googletest to create a CMake
project template. This would make it super easy to start a new C++ project
that already has testing and coverage capabilites. Until then, happy hacking!

[cmake]:      https://cmake.org
[gcov-intro]: https://gcc.gnu.org/onlinedocs/gcc/Gcov-Intro.html
[cmake-gcov]: https://github.com/jhbell/cmake-gcov
[part-1]:     {{ site.baseurl }}{% post_url 2017-06-22-cmake-what-you-need-to-know-part-1 %}
[last-post]:  {{ site.baseurl }}{% post_url 2017-06-11-welcome-to-my-blog %}
