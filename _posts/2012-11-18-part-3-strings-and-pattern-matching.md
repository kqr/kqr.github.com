
---
layout: post
title: "Part 3: Strings and case expressions"
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

If you want an actual double quote in a string, you can not simply type it, because Haskell will think that you want to end the string. What you have to do is prefix the double quote with `\` (a backslash.) It might look something like

    "This is a string with \"double quotes\" in it."

Because the inner double quotes are prefixed with backslashes, they will not end the string. Remember to prefix any double quotes inside a string; forgetting to do so is a common mistake among novices.

When your program reads a line of input from the user, that line will be supplied to your program as a string. The function `putStr` has earned its name because it *put*s a *str*ing to the screen. You can either specify that string literally in the source code, as you did in the beginning, or you can specify a named string -- input from the user.

String concatenation
--------------------

To familiarise yourself with strings, open up the interpreter without loading any program. Toying around in the interpreter without writing a program is a great way to test something quickly. Try typing in

    "Hello, " ++ "world!"

and pressing return. The interpreter responds with a single string.

    Prelude> "Hello, " ++ "world!"
    "Hello, world!"

When you type an expression in the interpreter, it will try to compute the value described by the expression and then display it back to you. In this case, the expression was computed to a single string, and it was printed back to you. The value that gets printed back to you is *not* something that would get printed if the expression was part of a normal program; it is only when you type expressions directly into <span class="textsc">GHCi</span> that their results will be printed.

You constructed a single string from two strings. Just like you may use `+` to add two numbers in mathematics, Haskell uses `++` to add two strings together, end to end. The `++` operator is actually a function, and it is usually called the *list append* operator. To append two things means to put them together, end to end. I will refer to `++` as the *string* append operator, because that is what it does for us right now. And just like you can add several numbers together in mathematics, you can do the same with strings in Haskell.

    Prelude> "Hello, " ++ "world!" ++ " How " ++ "are you?"
    "Hello, world! How are you?"

You might be curious as to whether the string in the last example is computed left-to-right or right-to-left. Even though it doesn't matter to the result, it might be interesting to know that `++` works right-to-left. The string concatenation operator starts computing things from the right. If you want to change that behaviour, you can use parentheses, just like in maths. The following will compute left-to-right instead.

    Prelude> (("Hello, " ++ "world!") ++ " How ") ++ "are you?"
    "Hello, world! How are you?"

Anything in parentheses will be computed first by Haskell. In this case, the result is the same, but in the next example it does make a difference. Next, we will try out our newfound knowledge in a real program.

    module Main where
    
    main = do
      putStrLn "What do you prefer to drink?"
      beverage <- getLine
      putStrLn ("Ah, so you like " ++ beverage ++ ".")
      putStrLn (beverage ++ " is an interesting choice.")
<div class="label">Appending strings in action</div>

This programs combines user input with appending strings, to avoid superfluous `putStr`s in the program. It is worth pointing out that the parentheses around the strings *are* necessary to show Haskell that you want to pass the whole concatenated string to `putStrLn`, and not just the first part of it. Generally, when you want one computation to be treated as a unit and want it computed before everything else, you might need to wrap it in parentheses.

The attentive reader will also notice that the code in the last example has a line break on the first line of the `main` function, just after the `do`. You are allowed to start on new lines quite liberally, as long as you *indent* the code suitingly. In this case, the next line has to start further in than the `main` -- I achieved this with two spaces, some people like four. All the I/O computations which were part of the same sequence of I/O computations are still aligned horizontally.

The exchange with the user from this program will look something like the following

    What do you prefer to drink?
    Lagavulin 16yo
    Ah, so you like Lagavulin 16yo.
    Lagavulin 16yo is an interesting choice.


Pattern matching on strings
---------------------------

So far, all your programs have not made any decisions. They have been good programs, and they have accomplished what they were meant to, but you might like to express more complicated ideas. You might want to change the behaviour of the program radically based on user input.

Haskell has support for something we call *pattern matching*, which is a really powerful tool to implement decision making in a program. Pattern matching in Haskell might look something like what you see in the following example. When you load this program into the interpreter, it will give you a warning about overlapping patterns. Ignore that for the time being.

    module Main where
    
    main =
      case "hello" of
        "goodbye" -> putStrLn "Are you leaving already?"
        "hello"   -> putStrLn "Well hello!"
        "ice"     -> putStrLn "Burr, that's cold."
        "hello"   -> putStrLn "Not again!"
<div class="label">An example of pattern matching</div>

If you run this program, you will only get the output

    Well hello!

None of the other I/O computations were performed. The general form of pattern matching is

    case 〈expression〉 of
      〈pattern 1〉 -> 〈expression 1〉
      〈pattern 2〉 -> 〈expression 2〉
      〈pattern 3〉 -> 〈expression 3〉
      ...

Haskell tries to match the value computed by `〈expression〉` against the listed patterns. It will start at the first pattern, and see whether it is equal to the value computed by `〈expression〉`. If it is, the expression to the right of the arrow will be computed, and no more patterns will be tried. If it is not, the expression to the right of the pattern will *not* be computed, and the next pattern will be tried. This goes on until it runs out of patterns to try, at which point the program crashes.

The right arrows `->` (dash, greater than) do *not* need to be aligned. Sometimes it is nice to align them, and sometimes it makes the code harder to read. Whether or not to do it is completely up to the discretion of the programmer.

You might wonder why the pattern matching stops at the first match. The reason for this is clear if you remember that every expression can only compute a single value. The `case` pattern matching is an expression too, and as such, it can only compute a *single* value. This value has been arbitrarily chosen to be the first matching value. This is also the reason that the code in the previous example did not output two lines, even though two patterns does technically match.

The interpreter actually warned us that the last expression never would get computed. That's what "overlapping patterns" means. The interpreter detected that the last pattern in the previous example will never be matched against, because every expression that matches against the last pattern will also match against the second pattern, and only one computation will be performed, ignoring further patterns. This is one of the ways that I find Haskell to be a strong and safe language; it warns the programmer about very many eventual gotchas.

The `main` function in the last example also lacks a `do` construction. Because the `case` expression is a single expression in itself, you do not need the `do` keyword to turn it into a single expression.

The catch-all pattern
---------------------

If you try changing the value in the last example to see if the corresponding I/O computation is performed instead, you will sooner or later run into a problem. If the value can not be matched to any of the patterns, the program will crash and the interpreter will spit out an error message. This is not nice, so you may want a way to "catch" all the non-matching values. For this, Haskell provides `_` (the underscore.) The following program will not crash for any inputs.

    module Main where

    main =
      case "hrllo" of
        "goodbye" -> putStrLn "Are you leaving already?"
        "hello"   -> putStrLn "Well hello!"
        "ice"     -> putStrLn "Burr, that's cold."
        "hello"   -> putStrLn "Not again!"
        _         -> putStrLn "Value not recognised."
<div class="label">The catch-all pattern</div>

The underscore pattern has the unique property that it matches against *every* value. (Needless to say, it does not make sense to put the underscore pattern anywhere else than last, because the pattern matching will always stop at the first underscore.)

In this example, the value I want to match is a misspelling of the word "hello." It will not match against any pattern other than the underscore, so the output of the program will be

    Value not recognised.


Pattern matching on user input
------------------------------

Now, matching against strings in the code is kind of useless, because you know the result of the pattern matching before even running the program. What you can do is match on user input instead. Remember how a user entered line was just a string like any other? This next example implements a simple guessing game which matches on user input.

    module Main where

    main = do
      putStrLn "Guess what animal I'm thinking of..."
      guess <- getLine

      case guess of
        "rhino" -> putStrLn "OH MY GOD YOU DID IT!"
        _ -> putStrLn "Nope. Tough luck."
<div class="label">A simple guessing game</div>

Here, the name `guess` is bound to the users input, and then pattern matched against the string `"rhino"` and the catch-all pattern `_`. If the user typed "rhino," the pattern matching will succeed on the `"rhino"` pattern and the success message will be printed. If the user typed anything else, the pattern matching will succeeed on the catch-all pattern and print the failure message.

It is important to note that *patterns are not the same things as expressions*. You can for example not do the following, because the user input is an expression and not a pattern.

    do greeting <- getLine
       case "hello" of
         greeting -> putStrLn "Hello to you too!"
<div class="label">The interpreter will complain here! greeting is an expression and not a pattern!</div>

The other way around is okay, though, because then you are matching an expression (the user input) against a pattern (the string.)

    do greeting <- getLine
       case greeting of
         "hello" -> putStrLn "Hello to you too!"
<div class="label">This is how you should write it. The expression greeting is now matched against the pattern "hello".</div>


A naïve password mechanism
--------------------------

This mechanism can be used to provide a naïve username and password mechanism. The user has to provide the correct username and password for the program to disclose a secret message. A possible way to write this program is detailed below. The username is *steve* and the password is *secret123*.

    module Main where
    
    main = do
      putStrLn "Username:"
      username <- getLine
      putStrLn "Password:"
      password <- getLine
      putStrLn "Authenticating..."
    
      case (username ++ "/" ++ password) of
        "steve/secret123" -> do
            putStrLn "Username and password correct."
            putStrLn "Secret message: Moondust collected!"
        _ -> putStrLn "Incorrect credentials."
    
      putStrLn "End of transmission."
<div class="label">A naïve authenticating program</div>

In this program, the pattern matching is done against the string

    username ++ "/" ++ password

If you remember how string concatenation works, you realise that this is the username and password provided by the user, separated with / (a slash.) If the user for example entered the username "jones" and the password "apple29" the string on which the program will pattern match will be

    "jones" ++ "/" ++ "apple29"

which in turn is the same thing as

    "jones/apple29"

When wondering what a particular program or expression does, it is often enough to replace the names with their respective values (as I did here) to shed some light on the situation. The complete pattern matching part of the previous example is as follows.

    case username ++ "/" ++ password of
      "steve/secret123" -> do
          putStrLn "Username and password correct."
          putStrLn "Secret message: Moondust collected!"
      _ -> putStrLn "Incorrect credentials."

In this case, if the user has entered the username "steve" and the password "secret123," the appropriate computation is performed. Take careful note of the `do` keyword. Because the computation described is a sequence of I/O computations, the `do` keyword is necessary. There is however no `do` keyword on the catch-all pattern here, because it results in only one I/O computation.

In the beginning, correct use of the `do` keyword will be *very* difficult. Just remember that a single I/O computation is an expression, and the `do` keyword takes *multiple* I/O computations and turns them into a *single* expression. Anywhere a single expression is expected and you want to perform multiple I/O computations, use the `do` keyword.

If you already understand this example fully, you are off to a great start. It covers *everything* you have learned so far. Understanding even the difficult parts is vital, because they will be used time and time again in the chapters to come. If you feel like you do not understand everything in this example, please go back and review this chapter and the previous chapters. Read the most difficult parts again and figure out how they work. Ask a knowledgeable friend or someone on the Internet to help you out. Try to figure out exactly what you are having trouble with -- being able to ask concise questions is a highly useful skill even for seasoned programmers.



Exercises
---------

 *  Modify the programs from the last two exercises of the previous chapter using string concatenation so that they become shorter.

 *  Write a program that asks the user where they live, and if it is the same place as you, output something along the lines of "Zürich, huh? Great place to live." If the user enters someplace you do not like, output something like "Oh no, not Dakota!" instead. Otherwise, just let the program acknowledge where the user lives.

 *  Extend the program of the previous exercise so that it compliments the location of the user not only if it is the right city, but also if it is the right country, continent or planet. The exchange can look like

         Where do you live?
         Zürich
         Zürich, huh? Great place to live.

    or an exchange with *the same program* (only the input has changed) could look like

         Where do you live?
         Europe
         Europe, huh? Great place to live.

 *  Write a program that reads three lines from the user, and outputs something special if *all three lines are empty*. This will require a trick on your part to successfully detect whether all the inputs are empty or not. Hint: Use the string append operator.

 *  (Warning: difficult exercise.) The `case` pattern matching *is an expression* in itself, and can therefore be used *anywhere where an expression is expected*. This means that you can "nest" pattern matches (put them inside of each other), and therefore perform a second pattern matching only if the first one succeeds or fails.

    Modify the naïve authenticating program so that it aborts early if the username is wrong, but continues to check the password if the username is right. If the user types the wrong username, the interaction could look like

        Username:
        john
        Password:
        poodle12
        Authenticating...
        Wrong username!
        End of transmission.

    Otherwise, if the password is wrong, it could look like

        Username:
        steve
        Password:
        poodle12
        Authenticating...
        Wrong password!
        End of transmission.

    If both the username and the password is right, the interaction can look just like it did in the example.

 *  (Warning: difficult exercise.) Modify the program in the previous exercise so that it only asks for the password if the username was correct.

