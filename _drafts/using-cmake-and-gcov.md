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
Add related links and reasoning behind wanting to use Gcov in build process

# Putting It All Together
Explain the project

# Issues
- cleaning


[cmake]:      https://cmake.org
[gcov-intro]: https://gcc.gnu.org/onlinedocs/gcc/Gcov-Intro.html
[cmake-gcov]: https://github.com/jhbell/cmake-gcov
[part-1]:       {{ site.baseurl }}{% post_url 2017-06-22-cmake-what-you-need-to-know-part-1 %}
[last-post]:    {{ site.baseurl }}{% post_url 2017-06-11-welcome-to-my-blog %}
