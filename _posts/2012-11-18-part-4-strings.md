---
layout: post
title: "Part 4: Strings"
description: ""
category:
tags: []
---
{% include JB/setup %}


So far, I have been talking about "lines of text" or "user typed text" without really going into what "text" means, from a computer perspective. It might not be as obvious as it seems.

In Haskell and many other programming languages, the basic kind of text is the *string*. A string is a sequence of characters enclosed in `""` (double quotes.) A string may contain any number of characters, including none at all. The following are all valid strings:

 *  `"Hello, world"`
 *  `"abc"`
 *  `"This is still a valid string."`
 *  `""` <!-- I have no idea what's going on here. Without this comment this line doesn't get parsed into a li. -->
 *  `"x"`
 *  `"($*_&(*@!$"`

If you want an actual double quote in a string, you can not simply type it, because Haskell will think that you want to end the string. What you have to do is prefix the double quote with `\` (a backslash.) I touched on this in part 2 about printing to the screen, but I will repeat it here. It might look something like

    "This is a string with \"double quotes\" in it."

Because the inner double quotes are prefixed with backslashes, they will not end the string. Remember to prefix any double quotes inside a string; forgetting to do so is a common mistake among novices. Similarly, you have to prefix a backslash with another backslash to include it in the string.

When your program reads a line of input from the user, that line will be supplied to your program as a string. The function `putStr` has earned its name because it *put*s a *str*ing to the screen. You can either specify that string literally in the source code, as you did in the beginning, or you can specify a named string -- input from the user.

Appending strings
-----------------

To familiarise yourself with strings, open up the interpreter without loading any program. Toying around in the interpreter without writing a program is a great way to test something quickly. Try typing in

    "Hello, " ++ "world!"

and pressing return. The interpreter responds with a single string.

    Prelude> "Hello, " ++ "world!"
    "Hello, world!"

When you type an expression in the interpreter, it will try to compute the value described by the expression and then display it back to you. In this case, the expression was computed to a single string, and it was printed back to you. The value that gets printed back to you is *not* something that would get printed if the expression was part of a normal program; it is only when you type expressions directly into <abbr>ghc</abbr>i that their results will be printed.

> It is important to note the difference between what we are doing now and what we are doing when we say `putStrLn` something. Currently, we are simply computing things in <abbr>ghc</abbr>i, and <abbr>ghc</abbr>i is nice and shows us what the result is. When we do `putStrLn`, we are actually printing things in our program, and the things we are printing will appear regardless of whether we run the program in <abbr>ghc</abbr>i or on its own.
>
> When we are toying around with <abbr>ghc</abbr>i like this, nothing really gets printed, it just shows us what the result is.

The previous expression constructed a single string from two strings. Just like you may use `+` to add two numbers in mathematics, Haskell uses `++` to add two strings together, end to end. The `++` operator is usually called the *list append* operator. To append two things means to put them together, end to end. I will refer to `++` as the *string* append operator, because that is what it does for us right now. And just like you can add several numbers together in mathematics, you can do the same with strings in Haskell.

    Prelude> "Hello, " ++ "world!" ++ " How " ++ "are you?"
    "Hello, world! How are you?"


Appending strings in a real program
-----------------------------------

Next, we will try out our newfound knowledge in a real program.

    module Main where

    main = do
      putStrLn "What do you prefer to drink?"
      beverage <- getLine
      putStrLn ("Ah, so you like " ++ beverage ++ ".")
      putStrLn (beverage ++ " is an interesting choice.")
<div class="label">Appending strings in action</div>

This programs combines user input with appending strings, to avoid superfluous `putStr`s in the program. It is worth pointing out that the parentheses around the strings *are necessary* to show Haskell that you want to pass the whole appended string to `putStrLn`, and not just the first part of it. Generally, when you want one computation to be treated as a unit and want it computed before everything else, you might need to wrap it in parentheses.

The exchange with the user from this program will look something like the following

<pre>What do you prefer to drink?
Highland Park 12
Ah, so you like Highland Park 12.
Highland Park 12 is an interesting choice.</pre>



Exercises
---------

 *  Modify the programs from the last two exercises of the previous chapter by appending strings so that they become shorter. Remember to put parentheses around the things you want to print, so that Haskell knows you want to print the entire thing and not just the first bit.

