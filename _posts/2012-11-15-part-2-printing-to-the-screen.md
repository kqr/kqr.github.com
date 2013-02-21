---
layout: post
title: "Part 2: Printing to the screen"
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

Printing something to the user or receiving input from the user is collectively referred to as *performing I/O*. (I/O is short for Input and Output.) Steps one and three consists of performing I/O, while the middle step could be performed without intervention from the user at all. When programming in Haskell, we like to separate the steps that require I/O from the ones that does not. This is an important distinction that is unique to Haskell. Why this is beneficial will be evident later. This chapter is concerned with very basic mastery of the third step. We will examine the first step in the next chapter.


Hello, world!
-------------

The traditional introduction to any programming language is the "Hello, world" program. The "Hello, world" program performs only one thing: it prints out a greeting to the user. Reproduce the following program in its entirety in a new text file on your computer.

    module Main where

    main = putStrLn "Hello, world!"
<div class="label">The "Hello, world" program in Haskell</div>

Name the file something like `hello_world.hs`. Haskell source code traditionally uses the `.hs` file extension. To run this code, you need to open up `GHCi` in the same directory as the file you just saved, and then in the interpreter, type `:load hello_world.hs`. This will load your code into the interpreter and prepare it for running. When it comes back to the prompt, just type `main` and press return to start your first program. The exchange will look something like this:

<pre>Prelude&gt; :load hello_world.hs
[1 of 1] Compiling Main             ( hello_world.hs, interpreted )
Ok, modules loaded: Main.
*Main&gt; main
Hello, world!</pre>
<div class="label">Loading the "Hello, world" program into GHCi</div>

It is important that you copy this program, and all the other example programs, into your computer manually, character by character. Like when learning any language, you need to type the words several times before you know their spelling. If you pass a French exam by putting the answers sheet in the photocopier, you will not learn a single word French -- although you might become an expert on the photocopier. In the same fashion, if you want to learn Haskell properly, you will have to type the example programs literally into your computer every time. It may seem tedious at first, but as soon as you get to know Haskell better, it will become a breeze.

Type in the program character by character and run it. Verify that it prints "Hello, world!" to the user.

When the "Hello, world" program is done, it will return you to the interpreter console where you can type in further commands. If you make changes to `hello_world.hs` and save them, you will need to load the file again to test your changes.

If you get any error messages, go back and check your file for misspellings. An exact reproduction of the file should not cause any errors. If it still doesn't seem to work, find a Haskell community, show them your code and error messages, and ask them what is wrong with your code.

Even if you *don't* get any error messages, I would recommend you try removing parts of the program at random to see what kind of error messages you get. Take a note of them, because if you know what an error message means you will save yourself a lot of frustration when you get the same error message but don't know what you did wrong.


The "Hello, world" program
--------------------------

This is a line-by-line breakdown on how the program works.

Any complete program in Haskell must start with the line

    module Main where

Haskell programs are composed of *modules* -- bunches of related code kept together. Here, you state that your code belongs to a module named `Main`. A complete Haskell program needs to consist of at least one module, and that module has to be the `Main` module. Any other modules the program contains may be named more freely, but the first module needs to be the `Main` module. If your program was much, much longer, you could have wanted to split up the code into several modules. The general form of a module declaration is

    module 〈name〉 where

where `〈name〉` is the name of the module you wish your code to belong to. I will use `〈〉` (right and left angle brackets) in this book to signify that the programmer is supposed to insert content of his own choosing; the brackets are not part of the code and should not be in the final product.

The next line is a function definition:

    main = putStrLn "Hello, world!"

Functions are the things that perform computations in Haskell. You might wish to imagine functions like black boxes with slots on opposing sides. On one side, you find zero or more slots for inserting data. On the other side, you find a single slot where data can come out. The data that comes out is somehow computed from the data that you put in.

Sometimes you know how this computation is performed, and sometimes you do not. Often, you do not *need* to know how the computation is performed, unless you want to design the box yourself. Consider a function that computes the square a number. You put in the number 4, and out comes the number 16. You put in the number 3, and out comes the number 9. There are many possible ways to compute the square of a number, and you might not know the particular method this function uses, but that does not matter as long as it produces the correct results.

In this case, you define the `main` function. *Defining* a function means to state what computation the function is supposed to perform. It is, in essence, "designing the internals of the black box." When a Haskell program is run, it will try to perform the computation defined by the `main` function. The `main` function may in turn try to perform the computations of other functions, but the only function that is *guaranteed* to be computed as soon as you run the program is the `main` function.

Note that Haskell is case sensitive, and as such, it does not treat `main` and `Main` as the same thing, and neither of them are the same names as `MAIN` or `mAiN`. So far, I have introduced two *different* names, which do *not* refer to the same thing. `Main` is the name of a module, whereas `main` is the name of a function.

The general form of a function definition is

    〈function name〉 = 〈expression〉

An *expression* is a description of a *single* value to compute. For example, `4 + 5` is an expression that describes the sum of four and five and it will compute the value `9`. Similarly, `square 3` is an expression that describes the square of three, and will also compute the value `9`. `reverse "hello"` is an expression that describes the word "hello" reversed, and it will compute the value `"olleh"`.

You might react to the fact that our `main` function does not seem to compute any value; it just prints text to the screen! The observation is partly correct. While `putStrLn` does not compute a value that means anything to us, it actually computes a value, too. *Every* expression in Haskell computes a value. Since the value doesn't mean anything to us in this case, it is cleverly hidden by <abbr>ghc</abbr>i so as to not clutter our screen.

The `=` (equals sign) is meant to communicate that computing the value of the function *is the same thing as* computing the value of the expression. In our case, computing the `main` function is the same thing as computing the expression

    putStrLn "Hello, world!"

which, in turn, is computing *another* function! `putStrLn` is a function that receives a particular kind of data -- text enclosed in `""` (double quotes) -- which it prints out to the user. Constructing more complex computations from smaller "sub-computations" is a very common pattern in Haskell. In the next example, when we print two lines of text, we will see this pattern more clearly.


Goodbye, world
--------------

Save the following code in a file and load it in the interpreter. As with before, manually reproduce this program exactly. Do not copy and paste.

    module Main where

    main = do
      putStrLn "Hello, world!"
      putStrLn "Goodbye, world."
<div class="label">The "Goodbye, world" program</div>

You will notice two changes from the previous program. The most important one is the new `do` construction. The `do` construction starts a sequence of I/O computations. You did not need the `do` construction in the previous program because you only performed *one* I/O computation. That makes sense, because as you have learned, every expression computes a single value, and you can't say that that computing the `main` function is the same thing as computing *two* expressions. You would simply not know what value `main` should equate to. It could be either! When programming Haskell, we dislike ambiguity.

The `do` construction makes a *single* expression out of *many* I/O computations. Technical aside: It does this by performing all I/O computations, but then throwing away all the intermediary computed values and keeping only the value of the last computation. Of course, since we do not care for the computed values right now, it does not matter to us which value is kept, only that the main function equates to a single value so the code will run.

Pay attention to where the second `putStrLn` is placed: I/O computations need to be aligned horizontally with each other, to let Haskell know that they are part of the same sequence. They are also *indented* slightly more than the surrounding code to show Haskell that they belong to the `do` construct on the line above.


Including special characters
----------------------------

If you try to print the `"` (double quote) character, you will get an error message. This is because Haskell thinks you are done printing, when in fact you wanted to print the actual quote. To print a double quote, you will need to print `\"`. The `\` (backslash) serves as a way to say, "Treat the following character as part of the text to print." For example, you can do this.

    module Main where

    main = do
      putStrLn "As it is said in \"The Art of War\" by Sun Tzu,"
      putStrLn "\"Move swift as the wind, and"
      putStrLn "closely-formed as the wood."
      putStrLn "Attack like the fire and"
      putStrLn "be still as the mountain.\""
<div class="label">A program which prints double quotes</div>

By running the following program (or examining it closely) you will see that there are four double quotes embedded in the text that is printed. These will be printed as they are. If you want to print a `\` (backslash) you will similarly have to precede it with another backslash. For example, you can print `"C:\\Windows\\system32"` and it will only show a single backslash between the words.

Printing things on the same line
--------------------------------

There is a function closely related to `putStrLn` that you might want to know, too. The two messages in the previous program were printed on separate lines. Sometimes you might want to print two things on the same line. For that, you have the function `putStr` at your whim. Copy the next program manually and test it.

    module Main where

    main = do
      putStr "Hello, "
      putStrLn "world!"
      putStr "Goo"
      putStr "dbye,"
      putStrLn " world."
<div class="label">Printing more than one thing to a single line</div>

While `putStrLn` makes the next printing happen on a new line, anything after `putStr` will be printed to the same line as the previous output. Take careful note as to where a new line begins in the output of this program. A new line is started *after* every `putStrLn`.


To `do` or not to `do`
----------------------

What's really important here (and one thing you'll get wrong often!) is when to use `do` and when not to. The rule is simple: When you want to chain several I/O computations together, you need to use `do`. When you want only a single I/O computation, you shouldn't use `do`. However, there is little visual distinction between the cases, so it might take a while to internalise the rule and apply it consistently.


Exercises
---------

 *  Modify the "Hello, world" program so that it prints out something completely different. Use your imagination!

 *  Modify the "Hello, world" program so that it prints the same thing, but with longer code (i.e. more lines of code.) Hint: You may need more than one I/O computation.

 *  Write a completely new program from scratch which prints a Haiku poem. Try to write it from memory without looking at the programs you wrote earlier.

    The Western variation of the Haiku consists of three lines, where the first and last have five syllables, and the middle one has seven. Come up with your own poem or try to reproduce the following output.

    <pre>Coding is like art;
    Nobody understands you,
    And neither will you.</pre>
    <div class="label">An example of a Western Haiku poem</div>
