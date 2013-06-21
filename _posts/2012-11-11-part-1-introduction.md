---
layout: post
title: "Part 1: Introduction"
description: ""
category:
tags: []
---
{% include JB/setup %}


Programming is an art form. The way some people are stunned by a Monet or
Turner, I am stunned by a piece of beautiful code. The way some people get
angry when they hear someone play their favourite song wrong on a guitar, I get
angry when I see wrong code. Art is about provoking feelings in the observer,
and code can have that effect on programmers who are devoted to their skill.

Programming is also an intellectual exercise, much like composing music is a
creative exercise. When the musician gets that "Ah! Yes! This is precisely what
I need to make the song better" feeling, a programmer somewhere else in the
world exclaims to themself or their friends, "Ah! Yes! This is precisely what I
need to make the code better."

The difference between good code and bad code is huge. Good code should

 *  Be easy to read,
 *  Express the programmers intent clearly,
 *  Be easy to fix if something goes wrong,
 *  Be easy to reuse when conditions change, and
 *  Be stable and not suddenly go very wrong.

You will find that in many ways, good code resembles good scaffolding. It
should not obstruct anything, it should be modular, it should allow for
transportation and, most of all, it should provide stability and safety for the
people who use it. Lives depend on good scaffolding, and lives depend on good
code.

All of these requirements are difficult to achieve, and most often, programmers
miss one or two of them. The purpose of this book is to teach you programming
and to write code that meets as many as possible of those requirements.

Programming should of course also be a fun pastime. That is the most important
part. It is a hobby where you can decide on your own how challenging you want
it to be.



Programming languages
---------------------

Just as languages like English, Chinese or Spanish are used to communicate
between people, programmers need a language to communicate with the computer.
There are many such languages, called *programming languages*. Many programming
languages were short-lived; many had a thriving user base but are now extinct;
and comparatively few remain today.  Most of the remaining languages are fairly
new, but there are also some old ones that are just so good they refuse to die.

The first programming language that was used to communicate with computers is
the language we now call *machine code*. Machine code is a language expressed
in numbers, which the <abbr>cpu</abbr> -- *the central processing unit*, the
computational core of a computer -- is designed to understand.

![A computer reading machine code](/images/p01_machine-code.jpg)
<div class="ilabel">They really are designed like this, even today!</div>

Machine code is a language for computers, and as such it is really difficult
for humans to comprehend. Eventually, programmers got tired of expressing
themselves in the language of their computers, so they gave names to numbers
that occurred often, and constructed an automatic translator that would
translate those names into the correct numbers that the <abbr>cpu</abbr> could
understand. For example, the sequence "43 92 34" could be written as "ADD 92
43" and get translated by the computer itself to the language that it
understands.

Bringing the humans away from the gritty details of the internals of the
computer is called *abstracting*. Humans are excellent at inventing
abstractions for themselves to make things easier to handle. The concept of
abstraction is so important to programming that I wish to go out on a tangent
about it before I do anything else.


Abstraction
-----------

Imagine a car. Cars are vastly complex mechanical systems with cylinders, fuel
pumps, gearboxes, clutches, differentials, batteries, cooling, oil and several
more parts that many people have not even heard of. Yet anyone can drive a car.
When driving a car, you are not exposed to any of the complicated internals.
You see essentially a steering wheel, two or three pedals and a gear stick. You
do not even have to turn on the brake lights by yourself. The car does that for
you when you push the brake pedal down.

This car analogy tries to capture the nature of abstraction. Abstraction is how
people make scary things less scary. The scary car becomes less scary when you
are presented with only pedals and a steering wheel. We do the same with
programming. We hide internals and provide simpler and better ways to interact
with those internals.

The major difference between different programming languages is the varying
level of abstraction they provide. Lots of abstraction makes communicating what
we want with the computer easier. Some languages provide almost *no*
abstractions at all, and are very similar to the number sequences of machine
code. We call these *low-level languages*. Other languages provide a lot of
abstractions and would not be recognised as something used for programming by
someone in the 1940's. Not surprisingly, these are called *high-level
languages*. Generally, programmers avoid low-level languages these days,
because they are difficult.

<!--
![Punched cards vs Haskell code](/images/p01_punch_card.jpg)
<div class="ilabel">Can you tell which one is from the 60's and which one is
from today?</div>
-->

There is one problem with high-level languages, though. The general principle
is that as you go to a higher level language, the development time will
drastically decrease, but so will the speed of the program when it is run. In a
high-level language, you can develop a program in a matter of days, but it will
require much more computing power as the same program written over the
course of a few months in a low-level language. This of course only applies
when all else is equal. By the time someone has written a faster program in a
low-level language, someone else might have come up with a more clever way to
do the same thing in a higher-level language, thus turning out on top again. In
this day and age when hardware is cheap, it is generally considered that the
cost of programmer time is more valuable than the cost of computer time, and
high-level languages provide the following benefits.

 *  **High-level code is easier to read and understand**. This is fairly
    obvious, because it is closer to human thought processes and further from
    the metal of the computer. The programmer community have in large realised
    that code is read way more often than it is written, so writing programs
    that are easy to read is a very important thing to do.

 *  **In a high-level language, it is easier to express exactly what you
    mean**, rather than what steps the computer should perform to reach what
    you mean. In a sense, your code "speaks for itself" about what happens to a
    larger degree. Low-level code is often cryptic and you have to sort of
    "pretend to be the computer" to figure out how a particular piece of code
    works.

 *  **Finding faults is often easier in high-level code**, because you deal
    with concepts that are more familiar and intuitive than many of the
    concepts of low-level code. Say you have written a program to answer the
    question, "Is there a book in my bookshelf where the first name of the
    author is five letters long and starts with E?" Finding faults is made easy
    in high-level code due to the fact that the program can be broken down into
    two or three smaller pieces, each of which can be searched for faults
    individually. In low-level code, it is not uncommon for such a program to
    be divided into 10 or 20 smaller pieces that interact in various ways and
    can not be searched for faults individually.

As you probably noticed, we have touched on the points that described good code
in the start of this chapter. High-level languages turn out to be a great tool
to help us achieving the goals of good code.


Do I need a language?
-------------------

Programming as a hobby does not require a language. Programming is a pure
logical matter, and the ideas related to programming can be formed without a
language to form them in.  However, as an aspiring programmer, you will benefit
greatly from having a language to express your ideas in. For that purpose, I
have chosen to use Haskell in this book.

Haskell
-------

Haskell was invented in the early 1990's by a committee out of a conference
known as the <abbr>fpca</abbr>. The purpose of the committee was to make an
open standard of around a dozen of similar languages that coexisted during that
time.

In the beginning, Haskell was mostly confined to research, but since the
mid-1990's, several important features were added to the language and it
started enjoying adoption outside of computer science faculties. With a new
standard implemented in 1998, Haskell became a full-fledged general-purpose
programming language.

<!--
![A Haskell logo running from an ivory tower](/images/p01_ivory-tower.jpg)
<div class="ilabel">Haskell has escaped the ivory tower and the claws of
academia.</div>
-->

At the time of writing, the latest official Haskell standard was released in
2010.

Haskell is Unique
-----------------

If you just want to get into programming, you may want to skip down to "The
Haskell Platform," where I tell you how to install Haskell and test that it
works. The following two sections just provide a little detail about what makes
Haskell different from other programming languages, and why it is used in this
book.

Programming, at its core, is simply *performing computations* and then
combining them and their results to form a larger picture. I have chosen to use
Haskell for this book mainly because it is a very high-level language that
gives you very powerful tools to do exactly that; to think of programming as
performing computations. Many other languages, both low and high in level,
focus on another view of programming -- they say that programming is about
fiddling around with small parts at a time to sooner or later arrive at a
larger picture.

Haskell also enjoys a unique position among programming languages today.
Although it has yet failed to achieve the widespread adoption of e.g. Java or
Python, the user base of Haskell is growing larger by the minute. I believe the
reason for the constant growth is that Haskell is ready for the changes in
hardware and software that will come in the future; when other programming
languages become obsolete, Haskell will be extended and continue to live on.
This extension phenomenon has been observed with a few programming languages
before, and Haskell has everything it needs to continue being relevant for
years to come.  Already, Haskell is pioneering some of the concepts which are
only slowly creeping into other programming languages, so learning Haskell is
definitely a worthwhile investment even if you do not use Haskell itself later
on.

There are loads of libraries (other peoples code) for Haskell to download and
use in your programs. There are libraries for <abbr>http</abbr>, general
parsing, testing, compression, regular expressions, logging, markup, web
services, databases, parallellism, statistics, hashing and many, many other
common tasks.

The Haskell community is warm and welcoming. Although many like to use academic
words and strange terms for things, they are more than happy to explain
anything to you. The Haskell community is one of the nicest programming
language communities I have had experience with. If you are confused about
something -- *don't be afraid to ask!* The community will explain it to you as
thoroughly as you need.

Most of all, Haskell is free and open, and it is a lot of fun and an excellent
language to teach the art of computation in because it is, alongside all this,
designed for educational purposes.


Features of Haskell
---------------------

There are two main approches to programming. One is called *declarative*
programming, and programming declaratively means to view programs as
combinations of *independent* computations. Consider the following declarative
way to describe a process.

 1. Assume that `pinkify` means to paint something pink;
 2. Assume that `cars` is a name that refers to all the cars in this
    dealership;
 3. To perform an `action` on all items of a `collection`, you first perform
    `action` on the first item, then the next and so on until you reach the
    end;
 4. Show me what all `cars` would look like if you would perform `pinkify` on
    them.

First, we define two names, one for an action (painting something pink) and one
for a collection of objects (cars in this case.) Then we define what it means
to perform an action on multiple items. Then we perform the action on the
entire dealership and take a look. The actual computation is performed by
combining three independent sub-computations.

<!--
    Picture of lots of cars in various colours, but partly viewed through
    a pink "lens" that makes them look pink.
    Text below: "This is the declarative way to style cars."
-->

The opposite of declarative programming is called *imperative* programming.
Imperative programming is less about combining independent sub-computations,
and more about making a list of actions that the computer must perform to reach
the desired goal. This is how the same process description might look like when
you are programming imperatively:

 1. Assume that `cars` is a name that refers to all the cars in this
    dealership;
 2. Assume that `current` is a name that refers to the car we are currently
    looking at;
 3. Set `current` to the first car in `cars` (i.e. look at the first car);
 4. Paint the `current` car pink;
 5. Set `current` to the next car in `cars`;
 6. Repeat from step 4 until `current` refers to the last car in `cars`;
 7. Now look at all the cars.

This is accomplishing the same thing, but with much more guidance from the
programmer. We have to explicitly tell the program how to perform an action on
all cars, and we cannot perform a generic action on all cars -- we have to
repeat this definition for every action we want to perform. Worst of all,
though, this second version *actually paints all the cars pink*. If you decide
it was an ugly colour, you have to manually save somewhere which colour each of
the cars previously were so you can restore it.

<!--
    Picture of a guy painstakingly painting a single car pink.
    Text below: "The traditional imperative way of styling cars is tedious."
-->

Haskell is declarative, and as such, programs tend to get fairly small and are
quick to write. The code is also easier to understand and fix when it goes
wrong.

In the previous example, we saw how the declarative version never actually
painted over any previous cars. This is common for declarative languages and it
is likewise true for Haskell. In fact, it is *not possible* to overwrite data
in Haskell. You can create new data from old data, but you can never change or
remove old data. Old data gets automatically removed when it is no longer used,
but not until then. This provides safety against some errors in the code that
might otherwise be hard to find if they by mistake could modify program data to
effectively hide themselves.

Partly as a consequence of the previous constraint, Haskell also tries its
damnedest to prevent computations from interfering with each other. Each
computation in Haskell is completely isolated and does not depend on any other
computation. A computation will only be able to affect "the outside world" when
the programmer is made aware of that risk. This is compared to pretty much all
other programming languages where some magic line of "outside world" code may
be hidden anywhere in a large project. It simply can not happen in Haskell. The
programmer has full control of which parts of the code that may affect "the
outside world."

Haskell also never performs a computation unless it is absolutely necessary.
All other mainstream languages try to perform a computation as soon as they
can. Haskell only computes as much as it has to. In many cases, computations do
not occur at all because the result is not needed. This, as it turns out, is a
double-edged sword. If it can be mastered, it is very effective and beautiful.

Haskell also keeps track of what kind of values you use. In Haskell, you can
not even start a program that has the remote possibility of trying to drive a
space shuttle like you would a motorcycle. To Haskell, a space shuttle and a
motorcycle are two completely different things and if it is somehow possible
for the program to mix them up, the program is considered unsafe. When writing
normal code, of course, there should not be any possibility to mix motorcycles
and space shuttles up, so this is a good thing.

Many of these features makes it easier for us to build safe programs which run
in somewhat controlled environments. I would describe Haskell as a very sharp
and pointy language, and if you do not care for what you are doing, you will
eventually run into those pointy bits and hurt yourself. If you do care for
what you are doing, though, you can use the pointy bits to fend off evil.


The Haskell Platform
--------------------

To be able to talk to a computer using Haskell, the computer must first "know"
Haskell itself. There are two ways to achieve this. A Haskell program can be
translated into machine code the computer understands, which is then saved to a
file that can be executed later by the computer. This is called the
*compilation* of a program. The second alternative is for another program to
read the Haskell code, and instead of translating it to machine code, the
program tries to get the computer to execute the correct actions directly. This
is called the *interpretation* of a program. Haskell supports both these
alternatives, but we will mostly be using the latter.

At the time of writing, the easiest way to get started with Haskell is to
install [The Haskell Platform](http://haskell.org/platform). The Haskell
Platform provides you with <abbr>ghc</abbr>, the compiler that provides the *de
facto* standard for Haskell aside from the official 2010 standard. The
installation procedure is a little different for every system; consult a local
expert for details.

In addition to the <abbr>ghc</abbr> compiler, The Haskell Platform will provide
you with an accompanying interpreter, <abbr>ghc</abbr>i.


Testing the interpreter
-----------------------

Assuming The Haskell Platform is successfully installed, run `ghci`. Although
you *can* compile your Haskell code to machine code, I will mostly be using the
interpreter throughout this book. Upon starting the interpreter, you should be
presented with a version number, some extra lines about packages loading, and
then the prompt

    Prelude>

This is the interpreter, where you will be spending most of your time examining
and testing your code. `Prelude` is simply the name for a few built-in features
that are always loaded, and the prompt reflects that. Try typing `4 + 3` and
then press return. The interpreter should respond with `7`.

    Prelude> 4 + 3
    7

You have now made sure your interpreter works properly, and you have learned
what programming is. Congratulations! A new world is about to unfold beneath
your feet.

