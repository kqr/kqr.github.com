---
layout: post
title: "Part 6: Messing with numbers"
description: ""
category:
tags: []
---
{% include JB/setup %}


This part is not really as much about numbers as it is about basic operations
on numbers. When you have read this part, you should be fluent in printing,
adding, subtracting, multiplying and doing other things to numbers in Haskell.
Believe it or not -- but numbers is actually a pretty complicated topic in
Haskell, so I have chosen to not delve too deeply into the mysteries of numbers
in this part. You will merely get to know what you need to know to do basic
things to numbers.

In Haskell you write numbers pretty much like you would in mathematics. All of
the following are considered valid numbers.

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

If you want to write really, really large or small numbers, Haskell supports
*scientific notation*, which means that the following are also valid numbers.

 *  `4.23e29`
 *  `1.14e-32`

Be careful, however. If you try to enter ridiculously huge or ridiculously tiny
numbers, you will notice that <abbr>ghc</abbr>i just responds with `Infinity`
or `0.0`. This is because of limitations of your computer, and not any sort of
theoretical limit. The technical term for this is that Haskell does not support
unlimited precision for fractional numbers. It does however support unlimited
precision for integers!

In either case, scientific notation for numbers is rarely used in code, because
we are mostly working with numbers between a millionth and a million, all of
which are fairly convenient to write out in the normal form.


Basic arithmetic
----------------

If you have followed the book diligently, you have already tried performing
addition. In the introduction, You were asked to open up the interpreter and
try to compute `3 + 4`. The result should be

    Prelude> 3 + 4
    7

as expected. Verify that for yourself. We are going to hang out in the
interpreter for a while now, so keep it open.

As you have witnessed, addition is performed much like appending strings, but
with a single `+` (plus sign) instead of two. You could also add several things
at once, like in the following example.

    Prelude> 3 + 4 + 12 + 9
    28

If you are wondering whether this goes left-to-right or right-to-left: addition
also goes from right to left in Haskell. If we want to change that, we need
parentheses as usual. The following is computed left-to-right.

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

There is however one caveat when doing simple arithmetic like this. The curious
reader has already encountered it. You seem to be unable to use a negative
number in an expression -- if you try, you will get a nasty-looking error
message.

<pre>Prelude&gt; 9 + -8

&lt;interactive&gt;:12:1:
   Precedence parsing error
       cannot mix `+' [infixl 6] and prefix `-' [infixl 6] in the same infix expression</pre>

The way to solve this is to wrap the negative numbers in parentheses. Entering
the following works fine, because then `(-8)` is computed to a negative number
before the addition occurs.

    Prelude> 9 + (-8)
    1

You may of course mix different arithmetic operators to compute more complex
expressions. You will notice that operators follow the rules for applying
operations that we are used to from mathematics, i.e. `3 + 5 * 2` is equal to
`13`, while `(3 + 5) * 2` is equal to `16`. Naturally, keeping track of your
parentheses is important as it makes a huge difference to the computed value.

    Prelude> (3 + 4 * 12) - 7 * (-1) / 4
    52.75
    Prelude> (3 + 4 * 12 - 7) * (-1) / 4
    -11.0

> In schools in the US, children learn a mnemonic for remembering the order of
> operations. If you remember "<abbr>PEMDAS</abbr>" and that it means
>
> 1. Parentheses
> 2. Exponents
> 3. Multiplication and division
> 4. Addition and subtraction
>
> you'll be fine. That's the order in which things are computed.


Fractional numbers
------------------

Another thing you may have noticed is that there seems to be some sort of
difference between the numbers `3 * 2` and `18 / 3`. One is displayed as `6`
while the other is displayed as `6.0`. This is where things get messy with
numbers in Haskell, so I will just say that, yes, there is a difference between
`6` and `6.0`. The first one is an integer while the latter is a fractional
number. If you need to convert from a fractional number to an integer, there
are three ways of doing so. Haskell provides you with three functions, one for
each way.

 1. You want to just throw away the decimal places:

        Prelude> floor 2.753
        2

 2. You want to round to the closest integer. Note: `.5` rounds down:

        Prelude> round 2.621
        3

 3. You want to round to the closest higher integer:

        Prelude> ceiling 1.314
        2

Of course, with an expression like `18 / 3`, you will want to wrap the division
in parentheses before giving it to one of the three functions above.

    Prelude> floor (18 / 3)
    6

If you do not wrap the division in parentheses, Haskell will parse your
expression as

    (floor 18) / 3

and throw an error message. The error message might seem uncalled for, and that
is true. This is once again a place where we run into the mess that is numbers
in Haskell.

There is a funny story about a snarky doctor with his patient, and it goes
something like this. A patient comes into the doctors office and says, "It
hurts when I do this." The doctor replies dryly, "Then stop doing that." For
the time being, I am going to be the snarky doctor and ask you to not divide
with the thing you get out of any of the three functions that convert
fractional numbers to integers.

> The technical reason for why it behaves like that is that you cannot use `/`
> for division with integers in Haskell, only fractional numbers. Something
> like `18` will automatically get converted to the fractional number `18.0`,
> but if you type `floor 18`, Haskell will see that you explicitly mean 18 as
> an integer, and therefore it will refuse converting and the division will
> fail. We will explore this further later on.

![A bunch of fractional numbers freezing out an integer](/images/p06_not-welcome.jpg)
<div class="ilabel">Haskell integers don't blend well with fractional numbers.</div>


More math functions
-------------------

To get it out of the way, here are a few other math functions you may be
interested in.

Computing the power of one number to another is done via the `**` (double
asterisk) operator in Haskell. You may for example calculate two to the seventh
with

    Prelude> 2**7
    128.0

This also works for non-integers and negative numbers. Don't forget the
parentheses around negative numbers!

    Prelude> 3.14**0.5
    1.772004514666935
    Prelude> (-1.5)**(-4)
    0.19753086419753085

There are two other operators that compute powers, but they are not as general
as this one. I mention them for completeness' sake, but I recommend you stick
with this one. The other two are `^^` (double caret) which only works with
integers in the exponent, and `^` (caret) which only works with positive
integers in the exponent.

If you want to find the absolute value of a number, i.e. its size without
regards for whether it's positive or negative, you can use the `abs` function.
Basically, if you give the `abs` function a positive number, it will give you
the same number right back. If you give it a negative number, it will give you
a positive number back.

    Prelude> abs 37
    37
    Prelude> abs (-29)
    29

> If you have not caught on by now, you put things into functions by writing
>
>     〈function name〉 〈thing you want to put into function〉
>
> which is simply the function name followed by the thing you want to put into
> the function, separated by a space. The thing you want to put into a function
> is called *a function argument*. The act of putting something into a function
> to receive a computed value is called *evaluating* a function. Evaluating is
> really just a fancy word for computing.
>
> Anything on this form is an expression. `abs (-29)` is an expression that
> describes the value `29`. In other words, every function call is an
> expression.

The `abs` function is great if you don't care for whether or not the number is
positive or negative. On the other hand, if that is the *only* thing you care
about, you might like the `signum` function. `signum` equals `1` for positive
numbers, and `-1` for negative numbers.

    Prelude> signum 4
    1
    Prelude> signum (-3)
    -1

You might guess what the `sqrt` function does -- it computes the square root of
a number.

    Prelude> sqrt 16
    4.0
    Prelude> sqrt 27
    5.196152422706632

To find the maximum or minimum of two numbers, you can use the functions `max`
or `min`. They take as arguments *two* numbers, and equals the larger and
smaller of them, respectively. They may be used like this.

    Prelude> max 3 7
    7
    Prelude> min 9 12
    9
    Prelude> max (-2) 0
    0

![A black box with a "max" label finding the max of two numbers](/images/p06_max.jpg)
<div class="ilabel">Let me introduce you to our buddy max!</div>

If you need to find the largest of several numbers, you'll need to be clever
and take out that programming brain of yours. For example, to find the largest
of the four numbers 8, 3, 12 and 5, we need to first find the largest number of
8 and 3.

    Prelude> max 8 3
    8

Then we can use that information to find the largest of 8, 3 and 12.

    Prelude> max (max 8 3) 12
    12

Do you see how this works? It compares 12 to the largest number of 8 and 3, so
in effect, the whole expression will equal the largest number of 8, 3 and 12.
We can figure this out on our own by replacing expressions with the values they
describe:

    max (max 8 3) 12

is equal to

    max 8 12

which is equal to

    12

Then, to find the largest of all four numbers, we just continue using this trick.

    Prelude> max (max (max 8 3) 12) 5
    12

You can work this one out for yourself with the replacement trick, if you still
don't really understand how it works.

Finally, there are the (in)famous trigonometric functions. If you need to find
out the sine, cosine or tangent of a number, Haskell saves your day! As you can
see, the trigonometric functions in Haskell work with angles measured in
radians and not degrees.

    Prelude> sin 1.39
    0.9837008148112766
    Prelude> cos 0.56
    0.8472551110134161
    Prelude> tan 1.57
    1255.7655915007897

There are more functions that work with numbers, and I've only skimmed the
surface of what's there, but these should last you long.


Exercises
---------

Naturally, you can verify your answers for yourself by writing short Haskell
programs. I would suggest you do so. Any chance you get to practise writing
Haskell programs is a golden moment.

 *  Which of the following are valid *numbers*? (Not counting expressions that
    get computed into numbers.)

     *  `52.000000000000000000000`
     *  `-0`
     *  `3/4`
     *  `0.7e5`
     *  `3 + 2i`
     *  `5i`

 *  What would be the result of computing the following expressions? Try to
    also predict if the values will be integers or fractional numbers.

     *  `17 - 4 * 2`
     *  `96 * 10 / 12`
     *  `5 - (4 + 3) * (-1) - 17`
     *  `2 * 3.14`

 *  Can you correct the following expressions so they are no longer invalid?

     *  `round 3.6 / 0.2`
     *  `4 * -3 + 12`
     *  `ceiling 5.29 / 9 * -1`

 *  The `cos` function has a really cool property. If you repeatedly take the
    cosine of a number, you get closer and closer to a fixed number *x*,
    regardless of which number you started with.

        Prelude> cos 1.2
        0.3623
        Prelude> cos 0.3623
        0.9350
        Prelude> cos 0.9350
        0.5938
        Prelude> cos 0.5938
        0.8288
    <div class="label">Getting closer and closer to a fixed number.</div>

    Can you figure out approximately what the mysterious number *x* is? Can you
    figure out what it is with only a single calculation? Hint: You can use
    parentheses like we did with `max` to combine several cosine computations
    into one calculation.
