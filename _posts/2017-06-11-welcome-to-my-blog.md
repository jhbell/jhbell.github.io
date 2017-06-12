---
layout: post
title:  "Welcome to My Blog!"
date:   2017-06-11 16:03:21 -0500
categories: blog tutorial
---
<img src="{{ site.url }}/assets/jeff-web.jpg" 
     alt="Jeff Bell" 
     style="width: 200px; height: 200px; padding-bottom: 25px" />  
Welcome! I made this blog so I could post some of the things I learn on my 
journey through the field of computer science. I have been doing some work
outside of school lately, requiring me to solve some problems that there aren't 
many resources on the web about. This has led me to want to want to have a 
place where I can write about some of the things I find.

# How I Made My Blog

I thought I'd start today by talking about how I got this blog set up. If you
can't tell from the style of the blog, I made this using [Jekyll][jekyll-home],
a Ruby-based static website generator. The convenient part about using Jekyll
is that it integrates very easily with [GitHub Pages][gh-pages].

I already host my [personal website][jhbell] using a GitHub Pages
[respository][jhbell-repo]. I wanted to be able to add a blog to my website,
without laying to waste all of the hard work I put into creating my website
last summer. Whether or not I decided to use Jekyll became contingent on
whether or not I could integrate it with my website, rather than trash
everything I have.

Luckily, GitHub Pages works in a very convenient manner. Whenever you create
a Pages site for a repository, it will automatically add it to your user
website (i.e. jhbell.github.io). This functionality ended up being exactly
what I needed to keep my website, and add a blog.

All I had to do was add a [blog repository][blog-repo] and turn on Pages
from the repository settings. Now, this page is conveniently hosted at
jeffreyhbell.com/blog/. To set up Jekyll, I used GitHub's 
[tutorial][ghp-tutorial] on setting up a Jekyll site locally, and then 
creating a new GitHub repository for the site.

# What's next?

I've been working on organizing a fairly large [CMake][cmake] project recently. 
I was able to integrate Google Test with the project after quite a bit of work. 
The problem right now is that I have to have the complete gtest library stored 
in the project (or symlinked), and it's a little clunky.

I think that having a [Docker][docker] image to contain the environment and 
dependencies will prove to be really useful. So, I am going to be looking into 
how to get all of that set up. I may post a tutorial on the simple way that I 
included gtest into my CMake project, since that in itself took a while for 
me to figure out. Until then, happy hacking!

[blog-repo]:    https://github.com/jhbell/blog
[gh-pages]:     https://pages.github.com/
[ghp-tutorial]: https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#step-3-optional-generate-jekyll-site-files
[jekyll-home]:  https://jekyllrb.com
[jhbell]:       http://jeffreyhbell.com
[jhbell-repo]:  https://github.com/jhbell/jhbell.github.io
[cmake]:        http://cmake.org
[docker]:       https://www.docker.com/
