---
layout: post
title: "Part 10: Compilation"
description: ""
category:
tags: []
---
{% include JB/setup %}



I keep calling the code we write "programs," and yet we haven't run them on their own! We simply load them into <abbr>ghc</abbr>i and run them from there. I am not cheating you. The programs we write are indeed capable of running on their own, as soon as you convert them to the right machine code format.

To do so, instead of starting `ghci`, just run `ghc 〈haskell source file〉` in your console. The output may look something like the following.

    $ ghc hello.hs
    [1 of 1] Compiling Main             ( hello.hs, hello.o )
    Linking hello ...

And then you should have an executable program with the same name as your Haskell source file. It is that easy!

The reason I choose to work in <abbr>ghc</abbr>i is that it makes it really convenient to experiment with expressions and values as I am programming. Contrary to popular belief, I don't know everything. A lot of things I figure out by playing around with <abbr>ghc</abbr>i as I go. You should do the same. Not all programming languages have support for an interactive prompt like that, so it is a privilege worth appreciating to have it.


Exercises
---------

 *  Compile one or more of the programs you have already written, and figure out how to run them independently of <abbr>ghc</abbr>i in your operating system!
