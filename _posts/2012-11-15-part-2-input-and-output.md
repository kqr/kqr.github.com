---
layout: post
title: "Part 2: Input and Output"
description: ""
category: 
tags: []
---
{% include JB/setup %}


Generally, the flow of a computer program consists of three steps.

 1. Read input from the user,
 2. Compute something based on the input, and
 3. Display the result of the computation back to the user.

These three steps might be executed once (to calculate something particular), or they might repeat indefinitely (for example in a real-time video game, where the three steps are executed multiple times each second.)

Printing something to the user or receiving input from the user is collectively referred to as *performing I/O*. (I/O is short for Input and Output.) Steps one and three consists of performing I/O, while the middle step could be performed without intervention from the user at all. When programming in Haskell, we like to separate the steps that require I/O from the ones that does not. This is an important distinction that is unique to Haskell. Why this is beneficial will be evident later. This chapter is concerned with very basic mastery of the first and last step.


Hello, world!
-------------

The traditional introduction to any programming language is the "Hello, world" program. The "Hello, world" program performs only one thing: it prints out a greeting to the user. Reproduce the following program in its entirety in a new text file on your computer.

    module Main where
    
    main = putStrLn "Hello, world!"
<div class="label">The "Hello, world" program in Haskell</div>

Name the file something like `hello_world.hs`. Haskell source code traditionally uses the `.hs` file extension. To run this code, you need to open up `GHCi` in the same directory as the file you just saved, and then in the interpreter, type `:load hello_world.hs`. This will load your code into the interpreter and prepare it for running. Then just type `main` and press return to start your first program. The exchange will look something like this:

    Prelude> :load hello_world.hs
    [1 of 1] Compiling Main             ( hello_world.hs, interpreted )
    Ok, modules loaded: Main.
    *Main> main
    Hello, world!
<div class="label">Loading the "Hello, world" program into GHCi</div>

When the "Hello, world" program is done, it will return you to the interpreter console where you can type in further commands. If you make changes to `hello_world.hs` and save them, you will need to load the file again to test your changes. If you get any error messages, go back and check your file for misspellings. If it still doesn't seem to work, consult a local expert.

It is important that you copy this program, and all the other example programs, into your computer manually, character by character. Like when learning any language, you need to type the words several times before you know their spelling. If you pass a French exam by putting the answers sheet in the photocopier, you will not learn a single word French -- although you might become an expert on the photocopier. In the same fashion, if you want to learn Haskell properly, you will have to type the example programs literally into your computer every time. It may seem tedious at first, but as soon as you get to know Haskell better, it will become a breeze.

Type in the program character by character and run it. Verify that it prints "Hello, world!" to the user.


The "Hello, world" program
--------------------------

This is a line-by-line breakdown on how the program works.

Any program in Haskell must start with the line

    module Main where

Haskell programs are composed of *modules* -- bunches of related code kept together. Here, you state that your code belongs to a module named `Main`. A complete Haskell program needs to consist of at least one module, and that module has to be the `Main` module. Any other modules the program contains may be named more freely, but the first module needs to be the `Main` module. If your program was much, much longer, you could have wanted to split up the code into several modules. The general form of a module declaration is

    module 〈name〉 where

where `〈name〉` is the name of the module you wish your code to belong to. I will use `〈〉` (right and left angle brackets) in this book to signify that the programmer is supposed to insert content of his own choosing; the brackets are not part of the code and should not be in the final product.

The next line is a function definition:

    main = putStrLn "Hello, world!"

Functions are the things that perform computations in Haskell. You might wish to imagine functions like black boxes with slots on opposing sides. On one side, you find zero or more slots for inserting data. On the other side, you find a single slot where data can come out. The data that comes out is somehow computed from the data that you put in.

Sometimes you know how this computation is performed, and sometimes you do not. Often, you do not *need* to know how the computation is performed, unless you want to design the box yourself. Consider a function that computes the square a number. You put in the number 4, and out comes the number 16. You put in the number 3, and out comes the number 9. There are many possible ways to compute the square of a number, and you might not know the particular method this function uses, but that does not matter as long as it produces the correct results.

In this case, you define the `main` function. *Defining* a function means to state what computation the function is supposed to perform. It is, in essence, "designing the black box." When a Haskell program is run, it will try to perform the computation defined by the `main` function. The `main` function may in turn try to perform the computations of other functions, but the only function that is *guaranteed* to be computed is the `main` function.

Note that Haskell is case sensitive, and as such, it does not treat `main` the same as `Main`, and neither of them are the same names as `MAIN` or `mAiN`. So far, I have introduced two *different* names, which do *not* refer to the same thing. `Main` is a module, whereas `main` is a function.

The general form of a function definition is

    〈function name〉 = 〈expression〉

An *expression* is a description of a *single* value to compute. You might react to the fact that our `main` function does not seem to compute any value; it just prints text to the screen! The observation is partly correct. While `putStrLn` does not compute a value that means anything to us, it actually computes a value, too. *Every* expression in Haskell computes a value.

The `=` (equals sign) is meant to communicate that computing the value of the function *is the same thing as* computing the value of the expression. In our case, computing the `main` function is the same thing as computing the expression

    putStrLn "Hello, world!"

which, in turn, is computing *another* function! `putStrLn` receives a particular kind of data -- text enclosed in `""` (double quotes) -- which it prints out to the user. Constructing more complex computations from smaller "sub-computations" is a very common pattern in Haskell. In the next example, when you print two lines of text, you will see this pattern more clearly.


Goodbye, world
--------------

Save the following code in a file and load it in the interpreter. As with before, manually reproduce this program exactly. Do not copy and paste.

    module Main where
    
    main = do putStrLn "Hello, world!"
              putStrLn "Goodbye, world."
<div class="label">The "Goodbye, world" program</div>

You will notice two changes from the previous program. The most important one is the new `do` construction. The `do` construction starts a sequence of I/O computations. You did not need the `do` construction in the previous program because you only performed *one* I/O computation. That makes sense, because as you have learned, every expression computes a single value. If you then would say that computing the `main` function is the same thing as computing the values of the *two* expressions

    putStrLn "Hello, world!"

and

    putStrLn "Goodbye, world."

then you can not know what value `main` should equate to. It could be either. When programming Haskell, we dislike ambiguity. The `do` construction makes a *single* expression out of *many* I/O computations. It does this by performing all I/O computations, but keeping only the value of the last computation. This means that your `main` function will equate to the value of the *last* `putStrLn`. Of course, since we do not care for the computed values right now, it does not matter to us which value is kept, only that the main function equates to a single value so the code will run.

Pay attention to where the second `putStrLn` is placed: I/O computations need to be aligned horizontally with each other, to let Haskell know that they are part of the same sequence.


Printing things on the same line
--------------------------------

There is a function closely related to `putStrLn` that you might want to know, too. The two messages in the previous program were printed on separate lines. Sometimes you might want to print two things on the same line. For that, you have the function `putStr` at your whim. Copy the next program manually and test it.

    module Main where
    
    main = do putStr "Hello, "
              putStrLn "world!"
              putStr "Goo"
              putStr "dbye,"
              putStrLn " world."
<div class="label">Printing more than one thing to a single line</div>

While `putStrLn` makes the next printing happen on a new line, anything after `putStr` will be printed to the same line as the previous output. Take careful note as to where a new line begins in the output of this program. A new line is started *after* every `putStrLn`.


Input
-----

You have now learned to write programs which print the same thing every time they are run. Sometimes programs needs to be more adaptive. A program might need to respond differently based on what input it receives from the user. This next program does precisely that. Type it out character for character and run it in the interpreter.

    module Main where
    
    main = do putStrLn "What is your favourite type of candy?"
              candy <- getLine
              putStr candy
              putStrLn " is a good kind, yeah."
              putStrLn "You have fine taste, sir!"
              putStr "Regards, "
              putStrLn "Your Computer."
<div class="label">Reading input from the user</div>

Two new things happen in this program. The one that probably stands out the most is the second line of the `main` function. `getLine` is actually a function that receives a line of input from the user. The left arrow `<-` (lesser than, dash) gives a name to that particular line of input. In this program, I choose the name `candy` to illustrate that the line contains the users favourite type of candy. You can bind almost any name to user input, as long as it starts with a lower case letter. The general form for binding a name to user input is

    〈name〉 <- 〈function that receives user input〉

When you have bound a name for the line the user typed, you can use the name elsewhere in the `main` function to refer to the line the user typed. In this particular program, I use it on the very next line to print out the candy type the user picked. Instead of writing a candy type in the code, enclosed in `""`, I referred to the line of input that the user typed. This makes the program a lot more dynamic, and as you probably can imagine, it opens up a lot of possibilities.

You could construct a simple cheerleading program, by simply asking the user for his teams name and then outputting it a few times in a row, like in the next example.

    module Main where
    
    main = do putStrLn "What is the name of your team?"
              team <- getLine
              putStr "Go "
              putStrLn team
              putStrLn team
              putStrLn team
              putStrLn team
<div class="label">A cheerleading program</div>

As usual, reproduce the program exactly and run it in the interpreter. This program contains nothing new to you. I bind the name `team` to the line the user typed, and then use that name to print out the team name several times.

When you have bound a name to some line of user input, using that name in your code is exactly equivalent to writing the line of user input in the code instead of the name. The reason to use names instead of writing e.g. `"Red Sox!"` directly in the code is, of course, that you don't know what the user is going to write.


Multiple inputs
---------------

Reading more than one line of input from the user is just as simple.

    module Main where
    
    main = do putStrLn "What is your name?"
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

 *  Modify the "Hello, world" program so that it prints out something completely different. Use your imagination!

 *  Modify the "Hello, world" program so that it prints the same thing, but with longer code (i.e. more lines of code.) Hint: You may need more than one I/O computation.

 *  Write a completely new program from scratch which prints a Haiku poem. Try to write it from memory without looking at the programs you wrote earlier.

    The Western variation of the Haiku consists of three lines, where the first and last have five syllables, and the middle one has seven. Come up with your own poem or try to reproduce the following output.

        Coding is like art;
        Nobody understands you,
        And neither will you.
    <div class="label">An example of a Western Haiku poem</div>

*  Write a program that asks the user for their favourite colour, their favourite animal and their lucky number. Using the input to print a description of a whacky animal like "A *blue* *cat* with *17* legs." Try to come up with something more to ask the user about, and integrate it in your description.

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

