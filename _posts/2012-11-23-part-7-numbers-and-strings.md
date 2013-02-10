---
layout: post
title: "Part 7: Numbers and strings"
description: ""
category:
tags: []
---
{% include JB/setup %}


Although the notion might seem strange to a new programmer, it is indeed common in programming to treat numbers and strings as two completely different things. We must know that there is a fundamental difference between a *string* and a *number*. From Haskells point of view, these are entirly different beasts. A number is sort of an imaginary idea of a quantity, whereas a string is physical characters that can be displayed to the user.



Converting numbers to strings
-----------------------------

The arithmetic we know is useless if we can not employ it to perform something in a program. Let us start by printing a number to the user. As we know, a number can not immediately be displayed to the user. If we try to print a number, we will get an error message back. It will look something like this:

    Prelude> putStrLn 17

    <interactive>:2:10:
        No instance for (Num String)
          arising from the literal `17'
    ...
<div class="label">The error message that ensues when we try to print a number</div>

This is because `putStrLn` expects a *string*, but we're handing it a number. We need a way to turn the number into a string, and luckily, Haskell comes with a function to do precisely that. The function `show` converts the number `53` to the string `"53"`. Stay in the interpreter with me for just a minute more, and we will take `show` for a spin.

    Prelude> show 53
    "53"
    Prelude> show (-19.27)
    "-19.27"

The function `show` is very similar to `putStrLn` or `floor`, but instead of printing things or converting numbers to other numbers, it converts numbers to strings. (In fact, `show` can convert pretty much anything to a string.)

If you have not caught on by now, you put things into functions by writing

    〈function name〉 〈thing you want to put into function〉

which is simply the function name followed by the thing you want to put into the function, separated by a space. The thing you want to put into a function is called *a function argument*. The act of putting something into a function to receive a computed value is called *evaluating* a function. Evaluating is really just a fancy word for computing.


Using numbers in a program
--------------------------

Imagine we wish to display the number 44 to the user. We need to convert it to a string using `show`, and then the result of `show` is ready to print. A program to print a number to the user might look something like the following.

    module Main where

    main = do
      putStrLn "Hello, user!"
      putStr "I have a fine number for you: "
      putStrLn (show 44)
<div class="label">Printing a number to the user</div>

Just for the record, we can also pattern match on numbers, much like we did on strings.

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

If we instead want to receive numbers from the user, we encounter the opposite problem! The user will be entering a string, but we want it to be a number. Remember that strings and numbers are entirely different things, as far as Haskell cares. (And this is indeed true for almost every programming language.) What we need is some kind of "reverse `show`," that will take a string and give us back a number. Fortunately for us, there exists such a function. It is called `read`.

If you try to use `read` in <abbr>ghc</abbr>i, you will encounter an error.

    Prelude> read "43"

    <interactive>:31:1:
        Ambiguous type variable `a0' in the constraint:
          (Read a0) arising from a use of `read'
<div class="label">Ambiguous type variable! Haskell doesn't know what you want to convert "43" <em>to</em>.</div>

The problem is that read doesn't only convert from strings to numbers -- it converts from strings to a lot of different kinds of values. Haskell simply doesn't know what you want to convert *to*. If you use the value in a computation later on, this usually doesn't become a problem, because Haskell can figure out what you want to convert it to based on how you use it. For example, if we try to use `floor` on the value we get out, it looks like the following.

    Prelude> floor (read "3.1415926")
    3

This time around, Haskell knows that you want to convert `"3.1415926"` to a number, because `floor` in turn expects numbers. This is one of the magic things Haskell does that not all languages can do. (Remember, things in parentheses are computed first, so this is completely okay to write. When the computer computes it, it goes from `floor (read "3.1415926")` to `floor 3.1415926` to just `3`.)

We could use `read`, for example, to pattern match against a number the user enters.

    module Main where

    main = do
      putStrLn "Please enter the current hour."
      hour <- getLine

      case read hour of
        11 -> putStrLn "Aren't you getting hungry?"
        16 -> putStrLn "You'll be getting off of work soon!"
        22 -> putStrLn "You certainly have to be tired now."
        _  -> putStrLn "Alright."
<div class="label">Pattern matching on a number the user entered</div>

Interaction with this program might look something like the following.

    Please enter the current hour.
    16
    You'll be getting off of work soon!

In this case, `read` works because Haskell sees that the patterns you are trying to match against are numbers, so it assumes you want to convert `hour` to a number.

You might say that you do not want to display any text to the user if he or she enters an hour different from the ones listed. This is possible by computing the I/O computation `return ()`. Essentially, `return ()` means "perform some kind of I/O that does nothing the user will ever notice." It is a very confusing expression to indicate doing nothing, but the reasons for that are  academic and theoretical. We will avoid them for the time being.

If we want to do nothing for any other hour than 11, 16 and 22 in the previous example, we write the pattern matching like this:

    case read hour of
      11 -> putStrLn "Aren't you getting hungry?"
      16 -> putStrLn "You'll be getting off of work soon!"
      22 -> putStrLn "You certainly have to be tired now."
      _  -> return ()
<div class="label">Doing nothing for a certain pattern</div>

If you are coming from other programming languages, keep in mind that `return` *does not mean the same thing in Haskell as it does in other languages*. I can not stress this enough. Try not to have any presumptions on how things work in Haskell while learning it. It will be difficult adjusting to the Haskell mindset, but it will be worth it for the enlightening experience when you finally can say, "Ohh, I *get it*." It will change your perspective on programming forever.

You can verify for yourself that `return ()` does nothing with the following simple snippet.

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

There are different ideas on how to handle errors like this, and nobody knows which way is right. Regardless of how you choose to handle errors, the important thing is to **never ever sweep errors under the rug**. It is very common for new programmers to suppress any errors they get but do not understand. This is very dangerous, because it means your program contains a real error and you no longer have any way of knowing which. Only handle errors you know how to handle and let the rest crash your program. Make sure you understand all the errors you get. Ask someone if you need to.

The traditional approach to handling errors is to write lots of error-handling code around areas that can fail, and make sure as few failures as possible can escape all that error-handling code. Another approach, called *fail-fast*, is to only write code for the correct input, and let the program blow up for invalid inputs. Or at least let parts of the program blow up, and have other parts restart the parts that just blew up.

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

For the time being, though, we will not handle any errors that occur when the user enters invalid input. The reason for this is that it results in messier code and the work is not quite worth it just yet. We can, for the time being, safely assume that you will be the only user of your code and that you can choose not to enter invalid input. You will learn to handle invalid input in due time.


Exercises
---------

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
    <div class="label">An involved program with lots of print statements.</div>

    While this might seem like a useless exercise, it is there to train you in thinking critically about data and realising the differences small things make. It should also be a relatively easy exercise if you have spent enough time playing with the things you have learned so far.

 *  Create a simple calculator program. Make the program ask the user if they want to add, subtract or multiply, and then ask them for numbers and show them the result. Interaction with the program could look like the following.

        Do you want to (a)dd, (s)ubtract or (m)ultiply?
        m
        Enter the first number.
        3
        Enter the second number.
        4
        The result is 12
    <div class="label">Example interaction with a simple calculator.</div>

    Or, if the user chooses to add,

        Do you want to (a)dd, (s)ubtract or (m)ultiply?
        a
        Enter the first number.
        2
        Enter the second number.
        9
        The result is 11
    <div class="label">Example interaction with a simple calculator.</div>

    Concentrate on which values are strings and which are numbers to get this one right. You will first have to convert the strings from the user into numbers, then add those two numbers, before converting the result back to a string to print to the user!

 *  Make a program that asks the user for their age, and make it tell them when they were born, given that the current year is whatever is the current year when you wrote the program. For example, if I wrote this program now, it would say something like

        How old are you?
        23

        Ah! You were born in 1989!
    <div class="label">This program was evidently written during 2012, but works as intended, at least!</div>

