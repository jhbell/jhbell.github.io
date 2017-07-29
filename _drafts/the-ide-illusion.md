---
layout: post
title:  "The IDE Illusion"
---
<img src="{{ site.url }}/assets/jeff-web.jpg" 
     alt="Jeff Bell" 
     style="width: 200px; height: 200px; padding-bottom: 25px" />  
In a [previous post][cmake-wyntk-p1] I talked about how we could set up a 
simple C++ project using CMake. I mentioned how CMake does a lot of the work 
for you, but hides most of the things that happen. 

This can be both good and bad. For example, think of your favorite IDE. Most 
IDE's allow you to press a green play button which will automatically compile 
and run your program. This is an amazing feature and has allowed for huge 
advancements in software development. At the same time, however, when you
leave it to the IDE to run all of your code, you have have no idea what goes
on when you hit that play button.

I like to refer to this phenomena as **The IDE Illusion**. Let's take a look
at some of the pros and cons of using this level of abstraction when building
a project.

# Pros

- Easy
- Clean
- Efficient

To be clear, I'm not trying to bash the "Run" button model. It's an extremely
beautifully designed concept, and the positive characteristics are obvious.

[cmake]:          https://cmake.org
[cmake-tutorial]: https://cmake.org/cmake-tutorial/
[cmake-commands]: https://cmake.org/cmake/help/v3.8/manual/cmake-commands.7.html
[clion]:          https://www.jetbrains.com/clion/

[cmake-wyntk]:  https://github.com/jhbell/cmake-wyntk
[part-1]:       {{ site.baseurl }}{% post_url 2017-06-22-cmake-what-you-need-to-know-part-1 %}
[last-post]:    {{ site.baseurl }}{% post_url 2017-06-11-welcome-to-my-blog %}
