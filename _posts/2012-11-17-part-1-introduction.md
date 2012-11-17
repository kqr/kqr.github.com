---
layout: post
title: "Part 1: Introduction"
description: ""
category: 
tags: []
---
{% include JB/setup %}


Programming is an art form. The way some people are stunned by a Monet or Turner, I am stunned by a piece of beautiful code. The way some people get angry when they hear someone play their favourite song wrong on a guitar, I get angry when I see wrong code. Art is about provoking feelings in the beholder, and code can have that effect on programmers who are devoted to their skill.

Programming is also an intellectual exercise, much like composing music is a creative exercise. When the musician gets that "Ah! Yes! This is precisely what I need to make the song better" feeling, a programmer somewhere else in the world exclaims to themself or their friends, "Ah! Yes! This is precisely what I need to make the code better."

The difference between good code and bad code is huge. Good code should

 *  Be easy to read,
 *  Express the programmers intent clearly,
 *  Be easy to fix if something goes wrong,
 *  Be easy to reuse when conditions change, and
 *  Be stable and not suddenly go very wrong.

You will find that in many ways, good code resembles good scaffolding. It should not obstruct anything, it should be modular, it should allow for transportation and, most of all, it should provide stability and safety for the people who use it. Lives depend on good scaffolding, and lives depend on good code.

All of these requirements are difficult to achieve, and most often, programmers miss one or two of them. The purpose if this book is to teach you programming and writing code that meets as many as possible of those requirements.

Programming should of course also be a fun pastime. That is the most important part. It is a hobby where you can decide on your own how challenging you want it to be.



Programming Languages
---------------------

Just as languages like English, Chinese or Spanish are used to communicate between people, programmers need a language to communicate with the computer. These languages are called *programming languages* and there have been many different programming languages invented throughout the history of computing. Many were short-lived, many had a thriving user base but are now extinct, and comparatively few remain today. Most of the remaining languages are fairly new, but there are also some old ones that are just so good they refuse to die.

The first programming language that was used to communicate with computers as we know them today was the language we now call *machine code*. Machine code is a language expressed in numbers, which the <abbr>cpu</abbr> -- *the central processing unit*, the computational core of a computer -- was designed to understand.

Eventually, programmers got tired of expressing themselves in the language of their computers, so they gave names to numbers that occurred often, and constructed an automatic translator that would translate those names into the correct numbers that the <abbr>cpu</abbr> could understand. For example, the sequence "43 92 34" could be written as "ADD 92 43" and get translated by the computer itself to the language that it understands.

Bringing the humans away from the gritty details of the internals of the computer is called *abstracting*. Humans are excellent at inventing abstractions for themselves to make things easier to handle. The concept of abstraction is so important to programming that I wish to go out on a tangent about it before I do anything else.


Abstraction
-----------

Imagine a car. Cars are vastly complex mechanical systems with cylinders, fuel pumps, gearboxes, clutches, differentials, batteries, cooling, oil and several more parts that many people have not even heard of. Yet anyone can drive a car. When driving a car, you are not exposed to any of the complicated internals. You see essentially a steering wheel, two or three pedals and a gear stick. You do not even have to turn on the brake lights by yourself. The car does that for you when you push the brake pedal down.

The car analogy tries to capture the nature of abstraction. Abstraction is how people make scary things less scary. The scary car becomes less scary when you are presented with only pedals and a steering wheel. We do the same with programming. We hide internals and provide simpler and better ways to interact with those internals.

The major difference between different programming languages is the varying level of abstraction they provide, to make communicating what we want with the computer easier. Some languages provides almost *no* abstractions at all, and are very similar to the number sequences of machine code. Other languages provides a lot of abstractions and would not be recognised as programming by someone in the 1940's. Today, we prefer not to speak machine code, because the language of computers is a difficult one. We prefer a language that provides us with as much abstraction as possible.

Do I Need a Language?
-------------------

Programming as a hobby does not require a language. A language is only needed when you wish to actually communicate with the computer. Programming is a pure logical matter, and the ideas can be formed without a language to form them in. However, as an aspiring programmer, you will benefit greatly from having a language to express your ideas in. For that purpose, I have chosen to use Haskell in this book.

Haskell
-------

Haskell was invented in the early 1990's by a committee out of a conference known as the <abbr>fpca</abbr>. The purpose of the committee was to make an open standard of around a dozen of similar languages that coexisted during that time.

In the beginning, Haskell was mostly confined to research, but since the mid-1990's, several important features were added to the language and it started enjoying adoption outside of computer science faculties. With a new standard implemented in 1998, Haskell became a fully-fledged general-purpose programming language. At the time of writing, the latest official Haskell standard was released in 2010.

Haskell is Unique
-----------------

I have chosen to use Haskell for this book mainly because it gives you very powerful tools to think about programming as *performing computations*, rather than something else. Performing computations is really everything programming is about.

Haskell also enjoys a unique position among programming languages today. Although it has yet failed to achieve widespread adoption, Haskell is safe for the changes that will come in the immediate future, and learning Haskell is a worthwhile investment for what is to come. Even if you do not use Haskell later on, many of the concepts which Haskell pioneer are gradually transferred to other languages, and you will have immense use of your Haskell knowledge.

Most of all, Haskell is free and open, and it is a lot of fun and an excellent language to teach the art of computation in because it is, alongside all this, designed for education purposes.

Features of Haskell
---------------------

(This section is a little technical. If you feel like most of the words are flying right over your head, skim it for interesting tidbits but do not concern yourself too much with the details.)

Haskell is a *declarative* programming language. Most other programming languages are, by contrast, imperative. This means that a Haskell programmer will concern himself with *what* computation should be performed, and not *how* it is performed, as would a programmer of an imperative language would. A declarative *what* approach makes it easier to analyse code to make sure it is correct.

Haskell does not allow destructive updates; in Haskell, data cannot be accidentally (or intentionally) changed. New data can be created from old data, but old data can never be changed, and old data can never be removed. This provides safety against some errors in the code that might otherwise be hard to find if they could modify program data.

Haskell is a *pure functional* language. This is partly a consequence of the previous constraint. Functions, the basic building blocks of Haskell, do not depend on any external variable. A function will never change its behaviour based on chance, a particular file not existing, the phase of the moon or anything else. If the function works in one context, it will work in *all other* contexts. To a new programmer, this might sound obvious, but it is actually the case in most of the mainstream programming languages that functions (or procedures or methods, as they are called there) can work in one context but be horribly broken in another.

Similarly, a function in Haskell will never change any external variable. A function will never accidentally create thousands of files or launch the entirety of the forgotten Soviet nuclear arsenal. At least not without the programmer being *very* well aware that might happen. Haskell functions are completely isolated blocks of computation and can not affect the rest of the world in any way. While this isolation might seem like a disadvantage, there are ways to overcome the problems it creates, and it provides the programmer with excellent guaratees to make analysis of the code easier.

Haskell is also a *lazily evaluated* language, which simply means that no computations will be performed until they are needed. Most other languages are eager in their computation strategy and will perform computations as soon as they can, even if the result of that computation is not needed at all. Haskell only computes things when they are needed.

Although these are lots of restrictions, and it might feel uncomfortable to work under restrictions, they are all useful restrictions. They do not interfer with normal work, but they prevent lots of mistakes programmers often make. All of these points make Haskell a great language, and it is one of the few languages I feel are future-proof.



The Haskell Platform
--------------------

To be able to talk to a computer using Haskell, the computer must first "know" Haskell itself. There are two ways to achieve this. A Haskell program can be translated into machine code the computer understands, which is then saved to a file that can be executed later by the computer. This is called the *compilation* of a program. The second alternative is for another program to read the Haskell code, and instead of translating it to machine code, the program tries to get the computer to execute the correct actions directly. This is called the *interpretation* of a program. Haskell supports both these alternatives, but we will mostly be using the latter.

At the time of writing, the easiest way to get started with Haskell is to install [The Haskell Platform](http://haskell.org/platform). The Haskell Platform provides you with <abbr>ghc</abbr>, the compiler that provides the *de facto* standard for Haskell aside from the official 2010 standard. The installation procedure is a little different for every system; consult a local expert for details.

In addition to the <abbr>ghc</abbr> compiler, The Haskell Platform will provide you with an accompanying interpreter, <abbr>ghc</abbr>i.


Testing the interpreter
-----------------------

Assuming The Haskell Platform is successfully installed, run `GHCi`. Although you *can* compile your Haskell code to machine code, I will mostly be using the interpreter throughout this book. Upon starting the interpreter, you should be presented with a version number, some extra lines about packages loading, and then the prompt

    Prelude>

This is the interpreter, where you will be spending most of your time examining and testing your code. `Prelude` is simply the name for a few built-in features that are always loaded, and the prompt reflects that. Try typing `4 + 3` and press return. The interpreter should respond with `7`.

    Prelude> 4 + 3
    7

You have now made sure your interpreter works properly, and you have learned what programming is. Congratulations! A new world is about to unfold beneath your feet.

