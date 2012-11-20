---
layout: post
title: "Part 4: Numbers"
description: ""
category: 
tags: []
---
{% include JB/setup %}


This part is not really as much about numbers as it is about basic operations on numbers. When you have read this part, you should be fluent in printing, adding, subtracting, multiplying and doing other things to numbers in Haskell. Believe it or not -- but numbers is actually a pretty complicated topic in Haskell, so I have chosen to not delve too deeply into the mysteries of numbers in this part. You will merely get to know what you need to know to do basic things to numbers.

In Haskell you write numbers pretty much like you would in mathematics. All the following are considered valid numbers.

 *  `-983828489419289182`
 *  `-4134`
 *  `-3.14`
 *  `0.0000000000000000000000000000000000001`
 *  `0.57`
 *  `4`
 *  `9.22`
 *  `37`
 *  `290349052352660002312.458290024`
 *  `42389023423498294385913492458939623591`

If you want to write really, really large or small numbers, Haskell supports *scientific notation*, which means that the following are also valid numbers.

 *  `4.23e29`
 *  `1.14e-32`

Be careful, however. If you try to enter ridiculously huge or ridiculously tiny numbers, you will notice that <span style="textsc">GHCi</span> just responds with `Infinity` or `0.0`. This is because of limitations of your computer, and not any sort of theoretical limit.

In either case, scientific notation for numbers is rarely used in code, because we are mostly working with numbers between a millionth and a million, all of which are fairly convenient to write out in the normal form.


Basic arithmetic
----------------

If you have followed the book diligently, you have already tried performing addition. In the introduction, I asked you to open up the interpreter and try to compute `3 + 4`. The result should be

    Prelude> 3 + 4
    7

as expected. Verify that for yourself. We are going to hang out in the interpreter now for a while, so keep it open.

As you have witnessed, addition is performed much like appending strings, but with a single `+` (plus sign) instead of two. You could also add several things at once, like in the following example.

    Prelude> 3 + 4 + 12 + 9
    28

If you are wondering whether this goes left-to-right or right-to-left: addition also goes from right to left in Haskell. If we want to change that, we need parentheses as usual. The following is computed left-to-right.

    Prelude> ((3 + 4) + 12) + 9
    28

Subtraction is performed similarly, but with a `-` (minus sign) instead.

    Prelude> 7 - 2
    5

As you might imagine, multiplication and division is similar too.

    Prelude> 5 * 3
    15
    Prelude> 20/7
    2.857142857142857

There is however one caveat when doing simple arithmetic like this. The curious reader have already encountered it. You can not seem to use a negative number in an expression -- if you try, you will get a nasty-looking error message.

    Prelude> 9 + -8
    
    <interactive>:12:1:
       Precedence parsing error
           cannot mix `+' [infixl 6] and prefix `-' [infixl 6] in the same infix expression

The way to solve this is to wrap the negative numbers in parentheses. Entering the following works fine, because then `(-8)` is computed to a negative number before the addition occurs.

    Prelude> 9 + (-8)
    1

You may of course mix different arithmetic operators to compute more complex expressions. You will notice that operators follow the precedence rules we are used to from mathematics, i.e. `3 + 5 * 2` is equal to `13`, while `(3 + 5) * 2` is equal to `16`. Naturally, keeping track of your parentheses is important as it makes a huge difference to the computed value.

    Prelude> (3 + 4 * 12) - 7 * (-1) / 4
    52.75
    Prelude> (3 + 4 * 12 - 7) * (-1) / 4
    -11.0


Fractional numbers
------------------

Another thing you may have noticed is that there seems to be some sort of difference between the numbers `3 * 2` and `18 / 3`. One is displayed as `6` while the other is displayed as `6.0`. This is where things get messy with numbers in Haskell, so I will just say that, yes, there is a difference between `6` and `6.0`. The first one is an integer while the latter is a fractional number. If you need to convert from a fractional number to an integer, there are three ways of doing so. Haskell provides you with three functions, one for each way.

 1. You want to just throw away the decimal places:

        Prelude> floor 2.753
        2

 2. You want to round to the closest integer. Note: `.5` rounds down:

        Prelude> round 2.621
        3

 3. You want to round to the closest higher integer:

        Prelude> ceiling 1.314
        2

Of course, with an expression like `18 / 3`, you will want to wrap the division in parentheses before giving it to one of the three functions above.

    Prelude> floor (18 / 3)
    6

If you do not wrap the division in parentheses, Haskell will parse your expression as

    (floor 18) / 3

and throw an error message. The error message might seem strange, and that is true. This is once again a place where we run into the mess that is numbers in Haskell. Do you know the story of what the doctor replied when his patient came in complaining that, "It hurts when I do this?" The doctor replied dryly, "Then stop doing that." For the time being, I am going to ask you to not divide with the thing you get out of any of the three functions that convert fractional numbers to integers.


Converting numbers to strings
-----------------------------

The arithmetic is useless if we can not employ it to perform something in a program. Let us start by printing a number to the user. First of all, we must know that there is a fundamental difference between a *string* and a *number*. From Haskells point of view, these are entirly different beasts. A number is sort of an imaginary idea of a quantity, whereas a string is physical characters that can be displayed to the user.

A number, therefore, can not immediately be displayed to the user. Every number does have a "string representation," though. You could imagine converting the number `53` to the string `"53"`. There is a function in Haskell to do precisely that, and it is called `show`. Stay in the interpreter with me for just a minute more, and we will take it for a spin.

    Prelude> show 53
    "53"
    Prelude> show (-19.27)
    "-19.27"

The function `show` is very similar to `putStrLn` or `floor`, but instead of printing things or converting numbers to other numbers, it converts numbers to strings. (In fact, `show` can convert pretty much anything to a string.)

If you have not caught on by now, you put things into functions by writing

    〈function name〉 〈thing you want to put into function〉

which is simply the function name followed by the thing you want to put into the function, separated by a space. The thing you want to put into a function is called *a function argument*. The act of putting something into a function in hope of receiving a computed value is called *evaluating* a function. Evaluation is really just a fancy word for computing.


Using numbers in a program
--------------------------

Imagine we wish to display the number 44 to the user. We need to convert it to a string using `show`, but the result of `show` is ready to print. A program to print a number to the user might look something like the following.

    module Main where

    main = do
      putStrLn "Hello, user!"
      putStr "I have a fine number for you: "
      putStrLn (show 44)
<div class="label">Printing a number to the user</div>

We can also pattern match on numbers, much like we did on strings.

    module Main where

    main =
      case 42 of
        0 -> putStrLn "That's a low number"
        1 -> putStrLn "That's barely any better"
        10 -> putStrLn "Now it's starting to look like something"
        42 -> putStrLn "Now, that's it!"
        50 -> putStrLn "Too high."
        _ -> putStrLn "What kind of a number is that?"
<div class="label">Pattern matching on a number</div>

which will produce the output

    Now, that's it!


Converting strings to numbers
-----------------------------

If we want to go the other way, and taking numbers from the user, we encounter the opposite problem! The user will be entering a string, but we want it to be a number! Remember that strings and numbers are entirely different things, as far as Haskell cares. (And this is indeed true for almost every programming langauge.) What we need is some kind of "reverse `show`," that will take a string and give us back a number. Fortunately for us, there exists such a function! It is called `read`. We could use it, for example, to pattern match against a number the user enters.

    module Main where

    main = do
      putStrLn "Please enter the current hour."
      hour <- getLine

      case (read hour) of
        11 -> putStrLn "Aren't you getting hungry?"
        16 -> putStrLn "You'll be getting off of work soon!"
        22 -> putStrLn "You certainly have to be tired now."
        _  -> putStrLn "Alright."
<div class="label">Pattern matching on a number the user entered</div>

Interaction with this program might look something like the following.

    Please enter the current hour.
    16
    You'll be getting off of work soon!

You might say that you do not want to display any text to the user if he or she enters an hour different from the ones listed. This is possible by computing the expression `return ()`. Essentially, `return ()` means "do nothing." It is a very confusing expression to indicate doing nothing, but the reasons for that are  academic and theoretical, so we will avoid them for the time being.

If we want to do nothing for any other hour than 11, 16 and 22 in the previous example, we write the pattern matching like this:

    case (read hour) of
      11 -> putStrLn "Aren't you getting hungry?"
      16 -> putStrLn "You'll be getting off of work soon!"
      22 -> putStrLn "You certainly have to be tired now."
      _  -> return ()
<div class="label">Doing nothing for a certain pattern</div>

If you are coming from other programming languages, keep in mind that `return` *does not mean the same thing in Haskell as it does in other languages*. I can not stress this enough. Try not to have any presumptions on how things work in Haskell while learning it. It will be difficult adjusting to the Haskell mindset, but it will be worth it for the enlightening experience you get when you finally get it. It will change your perspective on programming forever. You can verify for yourself that the return does nothing with the following simple snippet.

    module Main where

    main = do
      putStrLn "Hello, dear programmer of inferior languages"
      return ()
      putStrLn "Do you get this message too?"

As you can see from the output, `return ()` really does mean "do nothing" and nothing else.


Invalid input
-------------

One thing you might have noticed is that the program in the last example crashes if the user tries to enter something that is not a number. Try entering "fish" as the hour. The interpreter will respond with the cryptic error message

    *** Exception: Prelude.read: no parse

which simply means, "I couldn't parse `"fish"` into a number and I'm really confused now and I don't know what to do. I'll just blow myself up."

There are different ideas on how to handle errors like this, and nobody knows which way is right. The traditional approach is to write lots of error-handling code around areas that can fail, and make sure as few failures as possible can escape all that error-handling code. Another approach, called *fail-fast*, is to only write code for the correct input, and let the program blow up for invalid inputs. Or at least let parts of the program blow up, and have other parts restart the parts that just blew up.

The benefits and drawbacks of both approaches are best summarised in a table.


<table>
<tr>
    <td>&nbsp;</td>
    <td>Benefits</td>
    <td>Drawbacks</td>
</tr>
<tr>
    <td>Traditional approach</td>
    <td><ul>
        <li>Some errors can be handled very cleanly directly where they occur</li>
        <li>The programmer does not have to worry about where to handle errors</li>
    </ul></td>
    <td><ul>
        <li>Error-handling code is known for being the ugliest kind of code there is</li>
        <li>Some errors can not be handled neatly where they occur</li>
    </ul></td>
</tr>
<tr>
    <td>Letting things blow up</td>
    <td><ul>
        <li>The code becomes extremely tidy when it only handles the correct case, i.e. valid input</li>
        <li>Some errors are so bad the computation can not continue anyway</li>
    </ul></td>
    <td><ul>
        <li>The programmer needs to handle errors eventually, the question instead becomes "where?"</li>
        <li>Some errors are best handled without blowing up parts of the program</li>
    </ul></td>
</tr>
</table>

Commonly in code, a compromise of both approaches is used: some errors are handled directly where it is possible to do so, others will result in parts of the program blowing up only to let the rest of the program clean up afterwards.

Regardless of how you choose to handle errors, the important thing is to **never sweep errors under the rug**. It is very common for new programmers to suppress any errors they get but do not understand. This is very dangerous, because it means your program contains a real error and you no longer have any way of knowing which. Only handle errors you know how to handle and let the rest crash your program. Make sure you understand all the errors you get. Ask someone if you need to.

For the time being, though, we will not handle any errors that occur when the user enters invalid input. The reason for this is that it results in messier code and the work is not quite worth it just yet. We can, for the time being, safely assume that you will be the only user of your code and that you can choose not to enter invalid input. You will learn to handle invalid input in due time.


Exercises
---------

Naturally, you can verify your answers for yourself by writing short Haskell programs. I would suggest you do so. Any chance you get to practise writing Haskell programs is a golden moment.

 *  Which of the following are valid *numbers*? (Not counting expressions that get computed into numbers.)
 
     *  `52.000000000000000000000`
     *  `-0`
     *  `3/4`
     *  `0.7e5`
     *  `3 + 2i`
     *  `5i`

 *  What would be the result of computing the following expressions? Try to also predict if the values will be integers or fractional numbers.

     *  `17 - 4 * 2`
     *  `96 * 10 / 12`
     *  `5 - (4 + 3) * (-1) - 17`
     *  `2 * 3.14`

 *  Can you correct the following expressions so they are no longer invalid?

     *  `round 3.6 / 0.2`
     *  `4 * -3 + 12`
     *  `ceiling 5.29 / 9 * -1`

 *  For each of the following lines, consider: Is this line valid? Why/why not?

     *  `putStrLn 42`
     *  `"So, you are " ++ 37 ++ " years old. Interesting."`
     *  `"4" * ("3" + "5")`

 *  Correct the lines from the previous exercise while keeping all the values intact -- you are only allowed to add functions. Hint: Numbers and strings are not the same thing -- `show` and `read` are at your service.

 *  Predict what the following program prints out to the user.

        module Main where

        main = do
          putStrLn "Hello! This is the start of the program!"
          putStrLn "Are you ready for a ride?"

          case "special string " of
               "baboon" -> do putStrLn "A monkey, huh?"
                              putStrLn "Well, that's unexpected."
                              return ()
                              putStrLn "I don't think I like that!"

               "special string" -> putStrLn "This is much better."

               _ -> do return ()
                       putStrLn "Now I'm just kidding with you, am I not?"
                       putStrLn "Naah, I can't be."

          putStrLn "But I have to admit, this is confusing even to me."

          return ()
          putStrLn "So are we done yet?"
          putStrLn "Sure."

