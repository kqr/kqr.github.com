---
layout: post
title: "Part 11: Defining your own functions"
description: ""
category:
tags: []
---
{% include JB/setup %}



The previous chapters were pretty short, and now it is time for a meaty chapter. Since this is such a big topic, I have divided it into two chapters. This and the next chapter deals with the basics of defining your own functions.

We have used several functions before that comes with Haskell, for example `putStrLn` and `floor`. It probably doesn't come as a surprise that you can also define your own functions. As I have described, functions may be viewed as black boxes with slots for putting in data on one side, and a single slot for retrieving data on the other side. The black box somehow computes a value from the values put into it, and puts that value out on the other side.

The inputs to a function are called its *arguments*, and the outputs may be called a lot of different things, but to avoid confusion, we will simply say that the function *computes* a value, or *evaluates* to a value, or is *equal* to a value. In the case of

    Prelude> max 3.9 4.7
    4.7

we say that we evaluate the `max` function with the arguments `3.9` and `4.7`, and it computes `4.7`.

> In the English language, there are about a hundred different ways to say "evaluate a function" and every programming language community seems to have their own way of referring to it. Hopefully, we will only have to deal with a few.
>
> If you are coming from other programming languages, you are probably used to say "return value" when you mean the value you get back from a function. You could call it that in Haskell as well, but it might confuse you because of the `return ()` oddity. I would recommend just "computed value" or something along those lines.


Why functions?
--------------

Imagine you are running a rental car company. You charge your customers a base price for each car, and then some more depending on how much they have driven the car. The base price is $300, and then there's an extra 50 cents per mile. The way to calculate the final price would then be

    300 + 〈miles driven〉 * 0.5

We can imagine a program that lists how much you need to pay if you plan to drive different distances. It might look something like this:

    module Main where

    main = do
      putStrLn "This you need to pay:"
      putStrLn ("  10 miles: $" ++ show (300 +   10 * 0.5) ++ ".")
      putStrLn ("  50 miles: $" ++ show (300 +   50 * 0.5) ++ ".")
      putStrLn (" 100 miles: $" ++ show (300 +  100 * 0.5) ++ ".")
      putStrLn (" 500 miles: $" ++ show (300 +  500 * 0.5) ++ ".")
      putStrLn ("1000 miles: $" ++ show (300 + 1000 * 0.5) ++ ".")
      putStrLn ("2000 miles: $" ++ show (300 + 2000 * 0.5) ++ ".")
<div class="label">Repetitive code to calculate the price of a rental car</div>

While this is a really handy program, it suffers from one problem: *code repetition*. Just like we humans don't like to repeat ourselves when we speak to each other, we programmers don't like to repeat ourselves when we speak to the computer. In this program, we have written `300 + 〈something〉 * 0.5` so many times, and the only difference is really the "something" bit of it.

It feels like there should be some way to "teach" the computer how to calculate the price for a rental car, if you just tell it how far it is driven. This is where functions come in. When we are tired of writing the same computation over and over, we can tell the computer just *once* how to do the computation, and then we don't need to write it anymore.



The `price` function
--------------------

Compare the previous program to the following one.

    module Main where

    main = do
      putStrLn "This you need to pay:"
      putStrLn ("  10 miles: $" ++ show (price 10) ++ ".")
      putStrLn ("  50 miles: $" ++ show (price 50) ++ ".")
      putStrLn (" 100 miles: $" ++ show (price 100) ++ ".")
      putStrLn (" 500 miles: $" ++ show (price 500) ++ ".")
      putStrLn ("1000 miles: $" ++ show (price 1000) ++ ".")
      putStrLn ("2000 miles: $" ++ show (price 2000) ++ ".")

    price miles = 300 + miles * 0.5
<div class="label">Calculating the price of a rental car with a function</div>

It's not an enourmous improvement (we will learn how to do those much later), but it's a little better.

One of the neater things with this latter program is that the `main` function becomes much easier to read. When we see "`price 100`" we know it means quite literally "the `price` at `100` miles."

The magic of the program lies in the last line, though. This is where we teach the computer how to compute the price once you give it the number of miles you're interested in. A function definition reads as

    〈function name〉 〈argument name〉 = 〈expression〉

You might recognise this from the second chapter, where I showed you a simplified version of it. The function name may be almost anything, but try to keep to letters in the English alphabet. The argument name follows the same rules as the function name. The function will compute the value that is described by the expression.

When you evaluate a function, the argument name will refer to the argument you put into the function. In our case, if we try to evaluate `price 22`, then `miles` will refer to the value of 22. I'll try to put this into a concrete step-by-step description.

 1. We start out with

        price 22

 2. We refer to the definition of `price`, which says that

        price miles = 300 + miles * 0.5

 3. We can replace `miles` with `22` everywhere in the definition.

        price 22 = 300 + 22 * 0.5

 4. Then we can see that

        price 22 = 311.0

 5. And therefore, every instance of "`price 22`" in the code is going to be replaced with `311.0` by Haskell when the code is run.

This is essentially how programs are run: Every time Haskell needs a value but finds a function, it tries to figure out what value it computes, and then replaces the function with that value. It is a simple principle, but very, very powerful.

> To programmers coming from other languages: I am being very deliberate when I am saying "needs a value." Haskell *only* performs computations when it *needs* the value. Other languages tend to perform computations as soon as they can; Haskell tends to avoid performing computations for as long as it can.
>
> This may have unintuitive results if you are used to other languages' way of doing things, but as we will see later, it provides us with powerful mechanisms that are missing in other languages.

Note that I put the `price` function as far to the left as I could in the code. It is aligned with the `module` and `main` declarations. To be able to use the function everywhere in the file, you have to align the definition with the `module` and `main` declarations.


Name visibility
---------------

It is worth mentioning the concept of "visibility" of a name, because it is a difficult concept that is important for all kinds of programming. Every definition is associated with a "visibility." This means that every name in your program -- whether it is the name of a function or a variable -- has a limited "visibility." The visibility of the name decides where the computer is able to see (and use) the definition of a variable or function.

> Note: What I call "visibility" is more often referred to as "scope" in classical programming literature, but I believe "visibility" is a much more descriptive term.

Consider the following program.

    module Main where

    main = do
      putStrLn "How far do you want to go? (Enter in miles.)"
      input <- getLine
      let distance = read input

      putStrLn ("That will cost you " ++ show (price distance) ++ " bucks.")

    price miles = 300 + miles * 0.5
<div class="label">Calculating the price based on user input</div>

We have five definitions in this program. Try to figure out for yourself which five names we define in this program. You should be able to figure it out. Then continue reading.

 1. `main` -- the function where computation of our program starts,
 2. `input` -- the name we give to the line the user enters,
 3. `distance` -- the name we give to the *number* the user has entered,
 4. `price` -- the name of the function that calculates the price for a rental car,
 5. `miles` -- the name of the argument to the `price` function.

These five names are also associated with a visibility, which decides where they are usable in the program. Let's explore the visibility of these five names.

We already know that the functions `main` and `price` are visible throughout the entire file. Everything you define at the "top level" (as far to the left as possible) will be visible in the entire file.

Any names defined inside of a `do` construction are only visible from the place they are defined, and down to the end of the `do` construction. In our case, we can use the `input` string on line 6, 7 and 8. We can use the `distance` number on line 7 and 8. Note that this means that we can *not* refer to `input` or `distance` in the `price` function, because that is far outside of the `do` construction. (Recall that everything that is lined up vertically just below the `do` line belongs to the `do` construction.)

The last definition is the `miles` argument to the `price` function. Function arguments are only useable inside of the function they belong to. This means that we can only use the `price` definition on line 10.




Evaluating functions in the interpreter
---------------------------------------

You don't *have* to create a complete program with a `main` function. If you are only interested in a function (such as our `price` function here) you can evaluate it directly in the <abbr>ghc</abbr>i. Just like the interpreter allows you to type `4 + 3`,

<pre>Prelude&gt; 4 + 3
7</pre>

you can (after you have loaded your Haskell source file of course) run your own functions directly in the interpreter. For example, after loading `carrental.hs` I can use `price` in the interpreter, and it looks like the following.

<pre>*Main&gt; price 132
366.0
*Main&gt; price 77
338.5</pre>

The interpreter computes what the function is equal to, and displays it back to you.

I've said it before and I say it again: <abbr>ghc</abbr>i is really valuable especially for stuff like this. You can try your functions as soon as you have written them! Make use of this facility of Haskell, and remember one proverb:

> Test early, test often.

Are you not sure if you are supposed to have parentheses or not? Test both in <abbr>gch</abbr>i. I have too many times seen people who are new to programming who write entire programs consisting of anywhere up to a hundred lines, and *not test them a single time*. Make sure to test your program almost after every change you make. Just reload the file and compute some function in <abbr>ghc</abbr>i to see that it behaves like you expect it to.

Besides, problems with your code are often much easier to fix if you find them right after you introduced them.



Convert any expression to a function
------------------------------------

As we have seen, a function is supposed to be equal to an expression. This, in effect, means that we can turn any expression into a function. Remember the original program we wrote in the beginning of this chapter? It contained a lot of I/O computations on the form of

    putStrLn ("10 miles: $" ++ show (price 10) ++ ".")
    putStrLn ("50 miles: $" ++ show (price 50) ++ ".")
    putStrLn ("100 miles: $" ++ show (price 100) ++ ".")

and so on. These are really the same, only the numbers change. We can turn these to functions. As an example, I will use the third one here. It is essentially the same thing as

    putStrLn (show 100 ++ " miles: $" ++ show (price 100) ++ ".")

We put the entire expression into a function that takes an argument. This allows us to compute the I/O expression while not having the same `100` miles fixed. (I indented the expression slightly to indicate that it belongs to the `printPrice` function.)

    printPrice mi =
      putStrLn (show mi ++ " miles: $" ++ show (price mi) ++ ".")

If we want to imagine what happens when we run this function, we can evaluate it by hand. We do this simply by replacing expressions with the values we know they represent. This is how `printPrice 20` is computed.

    -- We know the 'mi' variable has the value '20.0', so we
    -- replace each occurrence of the variable 'mi' with '20.0'.
    printPrice 20 =
      putStrLn (show 20.0 ++ " miles: $" ++ show (price 20.0) ++ ".")

    -- We know that 'price 20.0' is equal to '300 + 20.0 * 0.5',
    -- so we replace it with that.
    printPrice 20 =
      putStrLn (show 20.0 ++ " miles: $" ++ show (300 + 20.0 * 0.5) ++ ".")

    -- We can calculate '300 + 20 * 0.5' to '310.0', so we replace
    -- it with that.
    printPrice 20 =
      putStrLn (show 20.0 ++ " miles: $" ++ show 310.0 ++ ".")

    -- 'show' on a number is simply turning it to a string.
    printPrice 20 =
      putStrLn ("20.0" ++ " miles: $" ++ "310.0" ++ ".")

    -- And then all that remains is appending the strings.
    printPrice 20 =
      putStrLn "20.0 miles: $310.0."

And then we're done. We have shown that evaluating `printPrice 20` is the same thing as evaluating `putStrLn "20.0 miles: $310.0."` so that is what Haskell is going to do.

Being able to say something complicated like the function we started out with, and then having it reduce to the simple `putStrLn` is at the core of programming. You connect simple pieces to form complex systems, which in turn reduce to something simple again, but it can reduce in endless variations.

We may of course just test this function in <abbr>ghc</abbr>i too. It looks like this:

<pre>*Main&gt; printPrice 57
57.0 miles: $328.5.
*Main&gt; printPrice 312
312.0 miles: $456.0.
*Main&gt; printPrice 12
12.0 miles: $306.0.</pre>

This function makes it possible to rewrite our first example as

    module Main where

    main = do
      putStrLn "This you need to pay:"
      printPrice 10
      printPrice 50
      printPrice 100
      printPrice 500
      printPrice 1000
      printPrice 2000

    printPrice mi =
       putStrLn (show mi ++ " miles: $" ++ show (price mi) ++ ".")

    price miles = 300 + miles * 0.5

As you can see, a lot of the *code repetition* is gone. There's a little left, but we'll have to leave that for another time, when we know how to deal with lists of things (in this case, lists of distances.)



Brief note about purity
-----------------------

The two functions we have created so far have been different in a very integral way. I won't be going into detail just yet, but if we try to look at what type of functions <abbr>ghc</abbr>i thinks they are, we see that

    *Main> :type price
    price :: Fractional a => a -> a
    *Main> :type printPrice
    printPrice :: (Fractional a, Show a) => a -> IO ()

If we try to see through the soup of characters, we see that one says `IO` and the other doesn't. If you did the exercises in chapter 9 about pure expressions and expressions with side effects, you know that `IO` in the type means that the function has side effects. In other words:

 *  `price` is a *pure* function.
 *  `printPrice` is a function *with side effects*.

How does this affect your program? It means that when you try to evaluate either of these functions, you have to think an extra time to see if they "just compute a value" or if they do something else too. Treat `printPrice` like you would treat `putStrLn`, and treat `price` like you would `sqrt` or some other maths function.

We will get to know more about this in the next chapter.



Exercises
---------

There are no exercises for this chapter yet.

