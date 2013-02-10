---
layout: post
title: "Part 10: Compilation and comments"
description: ""
category:
tags: []
---
{% include JB/setup %}

I spent the last chapter explaining the difference between pure expressions and expressions with side effects. I promised three important concepts though, so two remain. I'll devote this really short chapter to them.



Compilation
-----------

I keep calling the code we write "programs," and yet we haven't run them on their own! We simply load them into <abbr>ghc</abbr>i and run them from there. I am not cheating you. The programs we write are indeed capable of running on their own, as soon as you convert them to the right machine code format.

To do so, instead of starting `ghci`, just run `ghc 〈haskell source file〉` in your console. The output may look something like the following.

    $ ghc hello.hs
    [1 of 1] Compiling Main             ( hello.hs, hello.o )
    Linking hello ...

And then you should have an executable program with the same name as your Haskell source file. It is that easy!

The reason I choose to work in <abbr>ghc</abbr>i is that it makes it really convenient to experiment with expressions and values as I am programming. Contrary to popular belief, I don't know everything. A lot of things I figure out by playing around with <abbr>ghc</abbr>i as I go. You should do the same. Not all programming languages have support for an interactive prompt like that, so it is a privilege worth appreciating to have it.



Comments
--------

One thing that's really important in programming is that your code should be readable. Other programmers should be able to read your code and go, "Ah, yes, I undestand how it works." Sometimes, the meaning of a piece of code is obvious, but at times it can be really convoluted. It would then be nice if you could put a little remark in the code for the next programmer to read, that explains what the code does.

These little remarks are called "comments" in programming lingo. There are two ways to create comments in Haskell. The most common form of comment is started with `--` (two dashes) and continues for the rest of the line. Here is an example from a previous chapter with some comments in it.

    module Main where

    -- This is a simple program that asks for an username
    -- and a password, and if they are correct, the program
    -- will disclose a secret message.
    main = do
      putStrLn "Username:"
      username <- getLine
      putStrLn "Password:"
      password <- getLine
      putStrLn "Authenticating..."

      -- pattern matching on the user name and password
      -- both in one fell swoop
      case (username ++ "/" ++ password) of
        "steve/secret123" -> do
            -- the credentials were correct!
            putStrLn "Username and password correct."
            putStrLn "Secret message: Moondust collected!"
        _ -> putStrLn "Incorrect credentials."

      putStrLn "End of transmission."
<div class="label">An example of a program with comments</div>

All the lines starting with `--` will be completely ignored by Haskell. They are *only* there for the programmers to read. They are thrown out of the program when the computer reads it. These particular comments were perhaps not very useful, but it is a good habit to put comments in bits of the program that you find particularly tricky. Even if only you read your code, coming back to code without comments half a year later and trying to figure out what it does is sometimes a nightmare.

I said there was another way to make comments. You can also start comments in Haskell with `{-` and then end them with `-}`. This is useful for making a comment only in a part of a line, or making a huge block comment.

    module Main where

    {- This is the hello world program, with a block comment inside of it.
       block comments may sometimes be useful, and they may or may not
       extend over several lines, but generally, they are uncommon compared
       to the normal double dash comment. -}

    main = putStrLn {- a comment inside of a line -} "Hello, world!"



Exercises
---------

 *  Compile one or more of the programs you have already written, and figure out how to run them independently of <abbr>ghc</abbr>i in your operating system!

 *  Add comments to tricky sections of programs you have written earlier, where you explain in detail what the section does and how it works. You will thank yourself a month from now!
