---
layout: post
title: "Part 9: Side effects"
description: ""
category:
tags: []
---
{% include JB/setup %}


This chapter and the next is a little different. There will not be much code, but I will explain two really important concepts. Have a little patience, and I'll try to make this short.


Side effects and purity
-----------------------

I would like to elaborate briefly on so-called "side effects." An expression like `14 + 7` is very clean in the sense that it only computes a value and does nothing else. Every time you compute `14 + 7` you can be certain that the same, reliable thing will happen. This is what is called a "pure" expression. Any expression that only computes a value and does nothing else is a pure expression.

There is another kind of expression though, and you have used it a lot. Remember that `putStr "hello"` is also an expression? It does compute a value (a value we still are not going to care for) but it also produces text on the screen. The result, as far as the user is concerned, is different each time it is computed. The first time the result will be the text "hello" on the screen. The second time around, the result will be "hellohello" on the screen. This kind of expression is called "impure" and we say that it has "side effects."

Remember that there is a distinction in Haskell between "pure" expressions, that simply compute a value, and expressions with "side effects," that can change the world in some way. All IO computations have side effects.

A `putStr` computation is still a fairly predictable expression. One that is a lot worse is `getLine`. If your program is not run by a very simple-minded user, the line of text you get from the user will be different almost every time `getLine` is computed. Contrast this to something like `4 + 7` where the result will be the same all the time. Haskell sees expressions that generate different things each time they are run as something bad, and strives to isolate the unpredictable parts of a program as much as possible. This is why someone coming from other programming languages might think that `getLine` behaves a little different from what they are used to.

`getLine` does compute a value that is complete gibberish to us, just like `putStr`. We are not interested in the value that `getLine` computes, we are only interested in the line of user input that happened as a side effect. The line of user input is not a calculated value but a string originating from the keyboard or another input source. To get the user input from `getLine`, we need to bind a name to it with the left arrow (`<-`.) We *must* do that to be able to use the line the user typed. If we do not do that, the line the user typed will be thrown into cyberspace and we are left with an unusable gibberish value from `getLine`.

This way of comparing two input strings is correct.

    do
      putStrLn "Enter two lines"
      string_a <- getLine
      string_b <- getLine

      case string_a == string_b of
         ...

The following way of comparing two input strings is correct from a computer grammar standpoint (you are comparing two computed values, which is alright), but incorrect from a logical standpoint, and <abbr>ghc</abbr>i will indeed give you an error if you try to do this comparison.

    do
      putStrLn "Enter two lines"

      case getLine == getLine of
         ...

The problem that the interpreter screams at you for is that it doesn't make sense to compare the gibberish values that `getLine` computes. However, it does make sense to compare the lines of user input, and those have to be extracted by binding a name to them.



Names for pure values
---------------------

The difference between pure and impure expressions is also why the following code is wrong on the fifth line of the `main` function.

    module Main where

    main = do
      putStrLn "Enter a number"
      number <- getLine

      half <- read number / 2

      putStrLn ("Half the number is " ++ show half ++ "!")

The reason is that you can *only* use `<-` to get information from I/O computations (which have side effects). The left arrow `<-` is for extracting the side effect part of an I/O computation. If you want to create a name (a variable -- names for values are called variables, even though they never vary) for a pure value, there is another way to write it. The following is okay *inside of a do construction*.

    module Main where

    main = do
      putStrLn "Enter a number"
      number <- getLine

      let half = read number / 2

      putStrLn ("Half the number is " ++ show half ++ "!")

The general way to create variables from pure expressions inside a do construction is

    let 〈name〉 = 〈expression〉

This works exactly like `<-` only it works for pure expressions like `5 * 3` instead of I/O computations like `getLine`.

Just remember the following rule for creating variables inside a do construction.

    〈name〉 <- 〈I/O computation〉

    let 〈name〉 = 〈pure expression〉



Exercises
---------

 *  Are the following expressions pure or do they have side effects?

     *  `5 + 3`
     *  `putStrLn (show 4)`
     *  `getLine`
     *  `age / 2 + 7`
     *  `putStr name`
     *  `"So your name is " ++ name ++ ". Is that right?"`
     *  `putStrLn ("Hello, " ++ name ++ ".")`
     *  `floor length`

    Hint: You can check the answers for yourself with <abbr>ghc</abbr>i. In <abbr>ghc</abbr>i there is the command `:type` to check what kind of expression something is. It is used like in the following example.

        Prelude> :type putStrLn "yes!"
        putStrLn "yes!" :: IO ()

    If you see `IO` somewhere after `::` (double colon) you know the expression has side effects. If you don't see it, you know it is pure. Only one caveat: If you have any undefined variables (like `age` in the exercises) <abbr>ghc</abbr>i will complain. I recommend just replacing them with some probable value of theirs.

 *  Try to use the expressions from the previous exercise in a program somehow, to solidify your understanding of what makes an expression pure and impure.
