---
layout: post
title:  "CMake: What You Need to Know - Part 2"
---
<img src="{{ site.url }}/assets/jeff-web.jpg" 
     alt="Jeff Bell" 
     style="width: 200px; height: 200px; padding-bottom: 25px" />  
This post is the second part of a few posts that explain some basic usages of
[CMake][cmake] that I have found to be clean and effective. This part will go
over creating a multi-directory project and including libraries. If you haven't 
read [part one][cmake-wyntk-p1], I would encourage you to do so as this is a 
continuation of that first post.

# Recap

In my [last post][cmake-wyntk-p1] I talked about how we could set up a simple
cmake project. I mentioned how CMake does a lot of the work for you, but hides
most of the things that happen. 

This can be both good and bad. For example,
think of your favorite IDE. Most IDE's allow you to press a green play button
which will automatically compile and run your program. This is great and has
allowed for huge advancement
this **The IDE Illusion**. 


[cmake]:          https://cmake.org
[cmake-tutorial]: https://cmake.org/cmake-tutorial/
[cmake-commands]: https://cmake.org/cmake/help/v3.8/manual/cmake-commands.7.html
[clion]:          https://www.jetbrains.com/clion/

[cmake-wyntk]:  https://github.com/jhbell/cmake-wyntk
[part-1]:       {{ site.baseurl }}{% post_url blog/2017-06-22-cmake-what-you-need-to-know-part-1 %}
[last-post]:    {{ site.baseurl }}{% post_url blog/2017-06-11-welcome-to-my-blog %}
