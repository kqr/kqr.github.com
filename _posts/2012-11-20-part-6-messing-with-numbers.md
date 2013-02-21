---
layout: post
title: "Part 6: Messing with numbers"
description: ""
category:
tags: []
---
{% include JB/setup %}


This part is not really as much about numbers as it is about basic operations on numbers. When you have read this part, you should be fluent in printing, adding, subtracting, multiplying and doing other things to numbers in Haskell. Believe it or not -- but numbers is actually a pretty complicated topic in Haskell, so I have chosen to not delve too deeply into the mysteries of numbers in this part. You will merely get to know what you need to know to do basic things to numbers.

In Haskell you write numbers pretty much like you would in mathematics. All of the following are considered valid numbers.

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

Be careful, however. If you try to enter ridiculously huge or ridiculously tiny numbers, you will notice that <abbr>ghc</abbr>i just responds with `Infinity` or `0.0`. This is because of limitations of your computer, and not any sort of theoretical limit. The technical term for this is that Haskell does not support unlimited precision for fractional numbers. It does however support unlimited precision for integers!

In either case, scientific notation for numbers is rarely used in code, because we are mostly working with numbers between a millionth and a million, all of which are fairly convenient to write out in the normal form.


Basic arithmetic
----------------

If you have followed the book diligently, you have already tried performing addition. In the introduction, You were asked to open up the interpreter and try to compute `3 + 4`. The result should be

    Prelude> 3 + 4
    7

as expected. Verify that for yourself. We are going to hang out in the interpreter for a while now, so keep it open.

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
    Prelude> 20 / 7
    2.857142857142857

There is however one caveat when doing simple arithmetic like this. The curious reader has already encountered it. You seem to be unable to use a negative number in an expression -- if you try, you will get a nasty-looking error message.

<pre>Prelude&gt; 9 + -8

&lt;interactive&gt;:12:1:
   Precedence parsing error
       cannot mix `+' [infixl 6] and prefix `-' [infixl 6] in the same infix expression</pre>

The way to solve this is to wrap the negative numbers in parentheses. Entering the following works fine, because then `(-8)` is computed to a negative number before the addition occurs.

    Prelude> 9 + (-8)
    1

You may of course mix different arithmetic operators to compute more complex expressions. You will notice that operators follow the rules for applying operations that we are used to from mathematics, i.e. `3 + 5 * 2` is equal to `13`, while `(3 + 5) * 2` is equal to `16`. Naturally, keeping track of your parentheses is important as it makes a huge difference to the computed value.

    Prelude> (3 + 4 * 12) - 7 * (-1) / 4
    52.75
    Prelude> (3 + 4 * 12 - 7) * (-1) / 4
    -11.0

> In schools in the US, children learn a mnemonic for remembering the order of operations. If you remember "<abbr>PEMDAS</abbr>" and that it means
>
> 1. Parentheses
> 2. Exponents
> 3. Multiplication and division
> 4. Addition and subtraction
>
> you'll be fine.


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

and throw an error message. The error message might seem uncalled for, and that is true. This is once again a place where we run into the mess that is numbers in Haskell.

There is a funny story about a snarky doctor with his patient, and it goes something like this. A patient comes into the doctors office and says, "It hurts when I do this." The doctor replies dryly, "Then stop doing that." For the time being, I am going to be the snarky doctor and ask you to not divide with the thing you get out of any of the three functions that convert fractional numbers to integers.

> The technical reason for why it behaves like that is that you cannot use `/` for division with integers in Haskell, only fractional numbers. Something like `18` will automatically get converted to the fractional number `18.0`, but if you type `floor 18`, Haskell will see that you explicitly mean 18 as an integer, and therefore it will refuse converting and the division will fail.


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
