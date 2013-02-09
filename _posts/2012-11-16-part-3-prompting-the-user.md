---
layout: post
title: "Part 3: Prompting the user"
description: ""
category:
tags: []
---
{% include JB/setup %}


As I outlined in the previous chapter, the flow of a computer program consists of three steps.

 1. Read input from the user,
 2. Compute something based on the input, and
 3. Display the result of the computation back to the user.

We learned a little about the third step in the previous chapter, and it is time to learn about the first step now. When we have knowledge of them both, we are ready to tackle the most important part of a program, the middle step of computing something.


Input
-----

The programs you now can write will print the same thing every time they are run. Sometimes programs needs to be more adaptive. A program might need to respond differently based on what input it receives from the user. This next program does precisely that. Type it out character for character and run it in the interpreter.

    module Main where

    main = do
      putStrLn "What is your favourite type of candy?"
      candy <- getLine
      putStr candy
      putStrLn " is a good kind, yeah."
      putStrLn "You have fine taste, sir!"
      putStr "Regards, "
      putStrLn "Your Computer."
<div class="label">Reading input from the user</div>

Two new things happen in this program. The one that probably stands out the most is the third line of the `main` function. `getLine` is actually a function that receives a line of input from the user. The left arrow `<-` (lesser than, dash) *binds* a name to that particular line of input. In this program, I choose the name `candy` to illustrate that the line contains the users favourite type of candy. You can bind almost any name to user input, as long as it starts with a lower case letter. The general form for binding a name to user input is

    〈name〉 <- 〈function that receives user input〉

When you have bound a name for the line the user typed, you can use the name elsewhere in the `main` function to refer to the line the user typed. In this particular program, I use it on the very next line to print out the candy type the user picked. Instead of writing a candy type in the code, enclosed in `""`, I referred to the line of input that the user typed. This makes the program a lot more dynamic, and as you probably can imagine, it opens up a lot of possibilities.

You could construct a simple cheerleading program, by simply asking the user for his teams name and then outputting it a few times in a row, like in the next example.

    module Main where

    main = do
      putStrLn "What is the name of your team?"
      team <- getLine
      putStr "Go "
      putStrLn team
      putStrLn team
      putStrLn team
      putStrLn team
<div class="label">A cheerleading program</div>

As usual, reproduce the program exactly and run it in the interpreter. This program contains nothing new to you. We bind the name `team` to the line the user typed, and then use that name to print out the team name several times.

When you have bound a name to some line of user input, using that name in your code is exactly equivalent to writing the line of user input in the code instead of the name. The reason to use names instead of writing e.g. `"Red Sox!"` directly in the code is, of course, that you don't know what the user is going to write.


Multiple inputs
---------------

Reading more than one line of input from the user is just as simple.

    module Main where

    main = do
      putStrLn "What is your name?"
      name <- getLine
      putStr "So you're "
      putStr name
      putStrLn ". Cool."
      putStrLn "How old are you?"
      age <- getLine
      putStr age
      putStrLn " years old. I see."
      putStr "Well, it's been nice meeting you, "
      putStr name
      putStrLn "!"
<div class="label">Multiple inputs</div>

This program asks for both the users name and their age. Every line of input needs a unique name bound to it; once again, when programming Haskell, we dislike ambiguity. This means that you can not use the same name for different lines of input from the user.

While it may be tempting to try to print several things at once with `putStrLn`, you will find it difficult if you try. `putStr` and `putStrLn` are both designed to print only one thing each, and in the next chapter, we will begin investigating how we may mitigate that.




Exercises
---------

*  Write a program that asks the user for their favourite colour, their favourite animal and their lucky number. Use the input to print a description of a whacky animal like "A *blue* *cat* with *17* legs." Try to come up with something more to ask the user about, and integrate it in your description.

 *  Write a program that asks the user for their first name, last name, street address, postal code, postal town and then prints the answers formatted for writing on an envelope. Refer to the following for an example of the interaction with the user.

        What is your first name?
        Chris
        And your last name?
        Johnson
        What is your street address?
        Fisher street 12
        May I have your postal code please?
        198 47
        And the postal town?
        Clearborough
        ----
        On envelopes, write:

        Chris Johnson
        Fisher street 12
        198 47 Clearborough
    <div class="label">An example interaction for the envelope address printer. You might want to change the questions into something that makes sense for your location.</div>

