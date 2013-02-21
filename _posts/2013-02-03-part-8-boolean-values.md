---
layout: post
title: "Part 8: Boolean values"
description: ""
category:
tags: []
---
{% include JB/setup %}


So far, we have only been able to use pattern matching to do exact comparisons. What if we want to know if a number is smaller than another, or if a name comes before another in the phone book, or a combination of the two? We need to introduce something more.


Boolean values
-------------
We have already seen numbers and strings, both of which are two different kinds of values. There is a third kind of value, the *boolean* values. Numbers indicate some kind of quantity, strings indicate sequences of letters, and boolean values indicate truth or falsehood about something. There are only two boolean values, and in Haskell we call them `True` and `False`.

Just like there are functions that work with strings and numbers, there are functions that work with boolean values. But first, we will need to learn why boolean values are useful!


Testing for equality
--------------------

There is an equality operator in Haskell, which takes two values and tests whether they are the same. Fire up <abbr>ghc</abbr>i and try evaluating `5 == 5`, and then `3 == 12`. You should see something like the following.

    Prelude> 5 == 5
    True
    Prelude> 3 == 12
    False
<div class="label">Two numeric comparisons by Haskell</div>

We see that just like `+` is an operator that takes two numbers and returns a new number, and just like `++` is an operator that takes two strings and returns a string, `==` is an operator that takes numbers and returns a *boolean value*! The equality operator (as `==` is called) tests whether two things are equal, and then returns a boolean value to indicate that.

If we experiment a little further, we will find that the equality operator also works on strings. For example, the following works fine.

    Prelude> "carl" == "carl"
    True
    Prelude> "carl" == "barks"
    False

However, it does not work to compare a string to a number.

<pre>Prelude&gt; "3" == 3

&lt;interactive&gt;:9:8:
    No instance for (Num [Char])
      arising from the literal `3'
    ...</pre>
<div class="label">An error that occurs when you try to compare a number to a string... or a string to a number... or whichever way around it is.</div>

The same problem will arise if you try to compare something which is explicitly an integer to a fractional number, say by trying something like

<pre>Prelude&gt; (floor 3.14) &gt; 2.2

&lt;interactive&gt;:25:16:
    Ambiguous type variable `a0' in the constraints:
      (Fractional a0)
        arising from the literal `2.2' at &lt;interactive&gt;:25:16-18
      (Integral a0) arising from a use of `floor' at &lt;interactive&gt;:25:2-6
      (Ord a0) arising from a use of `&gt;' at &lt;interactive&gt;:25:14</pre>

We will therefore have to keep in mind that it seems like you can compare any values, but they have to be the same kind of values. Only compare numbers with numbers; only compare strings with strings; only compare boolean values wit...

What did I say? Compare boolean values? Sure! Why not, really? Saying that you can compare "any values" is a bit of a stretch, but you sure can compare boolean values!

    Prelude> True == True
    True

One kind of value you can't compare is the one you get from I/O computations. You can't do very much with that value, and definitely not compare it to anything. If you still are curious, you could try to compute something like `putStr "hai" == putStr "hai"`. You will get an error.

All of this is not very useful yet, but stay with me. I have a few other kinds of comparisons to introduce first.


Testing for inequality
----------------------

Similar to the equality operator is the inequality operator, or `/=` as we write it in Haskell.

    Prelude> 5 /= 5
    False
    Prelude> 3 /= 12
    True
    Prelude> "carl" /= "carl"
    False
    Prelude> "carl" /= "barks"
    True
    Prelude> True /= True
    False
<div class="label">Examples of inequality.</div>

The purpose of the inequality operator is to do the exact opposite thing of the equality operator. If you compare (hehe) the previous section to this one, it will be clear as day.

If you are coming from other programming languages, you might find the `/=` symbol chosen for Haskell a bit odd. The reason for choosing this symbol, of course, is that it resembles the mathematical symbol &#8800; that is used for the same purpose.

As an historical aside, I can mention that many of the design decisions about Haskell are rooted in mathematics. This is because many problems of programming have already been solved in mathematics for a few decades at least, and while mainstream languages are slow to realise this, Haskell developers are not. Do not let this put you off; despite what ignorant people say, Haskell is a highly practical language, and you don't need to know a single bit about the mathematical influences to be a proficient Haskell programmer.



Greater than and smaller than (or equal to)
-------------------------------------------

There are also four operators to test whether or not values are greater or less than others. They are summarised as follows.

 *  `>` tests whether the left value is greater than the right one (the crocodile opens his mouth to devour the big value)
 *  `>=` tests whether the left value is greater than *or equal to* the right one (the crocodile opens his mouth to devour the big value)
 *  `<` tests whether the left value is less than the right one (the crocodile opens his mouth to devour the big value)
 *  `<=` tests whether the left value is less than *or equal to* the right one (the crocodile has turned into an arrow from eating so many values. I believe my rule for memorisation has broken down.)

 Play around with them all to understand how they work. They can be used to compare numbers to each other, strings to each other, and oddly enough, boolean values to each other. I'm not sure what good the last one is, but it's there.

 Note that for strings, there are several ways you could imagine the comparison being done. One option would be to compare the lengths of the strings, and see which is the shorter one. That's not how Haskell does it, though. Haskell compares them according to their *lexicographic order*, which is just a fancy word for "phone book ordering." For example,

    Prelude> "Aabelson" < "Abraham"
    True

because Aabelson would be before Abraham in the phone book.


Chaining comparisons
--------------------

We saw with numbers that we could add several things at once by doing `1 + 2 + 3 + 4`. Is it possible to do this with comparisons? Another way to ask the question would be: In mathematics, it is common to state relations between numbers on the form of `4 < 9 < 16`, which is supposed to be read as "four is less than nine, which in turn is less than sixteen." Is this possible in Haskell?

The answer is no. Unfortunately. You can try it for yourself, but you'll be out of luck. It would be really neat, but it would violate a few principles of keeping things simple.

It is really uncommon for programming languages to support that way of chaining comparisons, for the same reasons as Haskell doesn't. In a later chapter, however, we will learn how Haskell (and indeed most programming languages) solves the problem!


Pattern matching on boolean values
----------------------------------

How are boolean values useful in a program? As you might realise, we can pattern match on them, just like we could on numbers and strings. We can imagine the following program, which asks the user for two names and then compares them.

    module Main where

    main = do
      putStrLn "Enter a name"
      name_1 <- getLine
      putStrLn "Enter another name, please"
      name_2 <- getLine

      case name_1 < name_2 of
        True  -> putStrLn (name_1 ++ " comes before " ++ name_2 ++ " in the phone book!")
        False -> putStrLn (name_1 ++ " comes after " ++ name_2 ++ " in the phone book!")
<div class="label">A program pattern matching on boolean values</div>

With this clever trick, we have made it possible to do fairly advanced decision making by our program. We evolved from a very simple kind of decision making that pattern matching allows, and went right into the heavy-duty territory of being able to make decisions based on almost any requirement.

Just be sure that when you ask the user for numbers, you convert strings to numbers with `read` first. For example, you might want to make the following program.

    module Main where

    main = do
      putStrLn "How old are you?"
      age <- getLine

      case read age < 47 of
        True  -> putStrLn "You are younger than me!"
        False -> putStrLn "You are older than me."

Here it is worth mentioning that `read age < 47` works because `read age` automatically gets computed first. Generally, functions get computed before operators.

If you combine operators, on the other hand, (say, you want to do `4 + 3 < 6`) there is no easy way of telling whether or not you need parentheses, other than just trying both variants to see which produces correct results. Generally, things behave like you would expect them to (in our example, addition is performed before the lesser than test), but do not take that for granted. One of the legitimate complaints about Haskell is precisely this -- you have no real way of learning what gets computed first when you combine operators. (The only way is to ask someone who knows, try all variants for yourself or check the source code for the operator.)


Exercises
---------

 *  Which of the following are valid boolean values?

     *  `No`
     *  `Yes`
     *  `Maybe`
     *  `False`

 *  Try to figure out what the following expressions evaluate to (what boolean value they compute.)

     *  `3 == 5`
     *  `-1 > 1`
     *  `"aaaaa" < "aaaaaa"`
     *  `"-1" > "1"`
     *  `False /= False`

 *  We have learned that Haskell uses phone book ordering to compare strings. Use the comparison operators to try to figure out what ordering Haskell follows to compare boolean values!

 *  Use pattern matching to write a program that asks the user for their age, and output remarks for some kind of age milestones. The interaction with the user might look like

    <pre>How old are you?
    34
    You probably knew how to read when you were 10
    You were probably allowed to drive when you were 18
    You probably had worked by 28
    You've passed 30!</pre>

    Hint: You can chain several `case` expressions after each other, as long as you line them up horizontally.

    You might be tempted to do something like `real_age <- read age` to avoid having to type `read age` all the time. Try to fight the temptation. In the next chapter, you will get to know why it doesn't work, and you can improve your program then.
