+++
title = "Bringing Lambda Calculus To Life"
author = ["Brandon C. Irizarry"]
date = 2025-10-09T00:00:00-04:00
publishDate = 2025-10-09T00:00:00-04:00
lastmod = 2025-10-09T22:29:57-04:00
tags = ["lambda-calculus", "functional-programming"]
draft = false
summary = "Bringing Lambda Calculus to Life"
+++

On July 25th 2025, I introduced the first commit of a new personal
project. I'm currently signed up for Boot.dev, an online boot camp for
learning backend web development. Recently Boot.dev hosted a
[hackathon](https://blog.boot.dev/news/hackathon-2025/). I had always wanted to get involved in a hackathon, and
(with some initial hesitation due to time-budgeting constraints)
voluteered my participation when surveyed on Discord.


## The Book {#the-book}

For a while, I've owned a copy of Greg Michaelson's _An
Introduction to Functional Programming Through Lambda
Calculus_[^fn:1]. I remember reading through it with gusto,
marvelling at how every step followed incrementally from the
previous step, which all start from three simple rules:

1.  Names
    -   Variables. Theoretically, these could be global or local,
        though the text focuses only on the latter kind.

2.  Abstractions
    -   Function definitions. Every function has exactly one
        parameter; to get multiple parameters, you have to nest
        function definitions inside each other into a kind of chain
        (what is sometimes known as "currying".)[^fn:2]

3.  Applications
    -   Function calls. These consist of a left part (which may or may
        not be an abstraction) and a right part (the application
        _argument_.)

And so, all expressions (I prefer to call them _lambda terms_, or
_terms_ for short) are constructed from this basis. Once
constructed, the engine that derives a useful value from an
expression is known as "beta reduction", of which there are at
least three varieties, which are in essence algorithms for
evaluating an application term. What follows is a quick and dirty
summary of the distinction between the three:

1.  Normal order
    -   Evaluate the left side first, then the right. That is,
        potentially evaluate a function definition first before
        evaluating the argument.

2.  Applicative order
    -   Evaluate the right side first, then the left. That is,
        evaluate the argument before evaluating the left-hand term.

3.  Lazy
    -   Like normal order, except cache the results of function calls
        to avoid re-evaluating expressions. This can be a huge
        time-saver in certain contexts, as I later found out when
        working on the project.

Thus, beta reduction is a glorified `eval` function for lambda
calculus.

The book continues by using this as a basis for notionally
implementing, without hard details, a simple functional
programming language - functional, in the sense that it eschews
side effects. Later on, things like sorting algorithms and string
processing are discussed in terms of this bespoke language.

To be honest, I found some of the later chapters a bit difficult
when I first read them. I _suspect_ that wouldn't be true anymore,
though I haven't revisited them in earnest quite yet.

One thing is for certain though - the section on the Y combinator,
which takes up only a few pages in the book, took me something
like three days to fully digest and understand!


## The Inspiration {#the-inspiration}

At some point in years past while reading the book, I became
inspired to implement the language. The book was, in my view,
advertising the fact that any computation could be reduced to mere
textual manipulation, similar to how all Euclidean geometry is
possible with only a straightedge and compass.

I first halfheartedly attempted an Elisp[^fn:3] implementation,
since I figured I could get away with using simple string-copying
techniques to implement, for example, beta-reduction. However, my
lack of Elisp skills prevented this from getting too far
along. Looking back in retrospect, I probably also didn't
understand normal-order reduction well enough to have gotten that
far anyway.

Snap back to today. Boot.dev advertises its hackathon: I'm game,
and decide to go back to once and for all put this old idea of
mine to rest. The plan would more or less be to re-read slowly
through each chapter and implement the language as I go.


## Choose Your Weapon! {#choose-your-weapon}

I initially went with Go as my language of choice. However, I
found Go somewhat difficult to wield, on account of its strict
static type system. For example, I asked ChatGPT for help on how
to set up definitions for the various lambda terms, given that
they should all inherit from a common `Term` type, and for
whatever reason I didn't like what it suggested. In the end, I
chose Python instead, which is definitely more pliant, forgiving,
and maximalist in what it offers the developer.

And with Python, I hit the ground running: I chose a simple
enum-tagging scheme for a given (parsed) lambda term, which were
nothing more than Python dicts.

Somewhere down the line, I stumbled upon the idea of using Python
type hints. I had already been familiar with them when working on
my first Boot.dev portfolio project[^fn:4], but didn't want to
introduce them prematurely, as this would put my thought process
in a straightjacket - after all, this is exactly what happened
when I tried to use Go.

However, at some point, I managed to incorporate `pyright`
LSP-based linting plus type hints into my code. In the end, I find
that static typic is amazing at helping me avoid tearing my hair
out over what turn out to be ridiculously simple bugs.


## Unit Testing {#unit-testing}

For this project, I thoroughly embraced unit testing (using Python's
`unittest` library).  As of the time of this writing, I'm not a
professional software developer, and so can't intelligently opine on
whether unit tests are a net good or evil. I'm sure there are plenty
of opinions on that in the wild. However, so far, I can say that
they give my project a conceptual "spine". On the one hand, my tests
for this project don't provide 100% coverage. Yet, I see my tests as
"interviewing" my code: can it get certain basic things right?  Does
a given feature work as expected, in a straightforward, common-sense
way? Does it give a false positive for a certain computation?  Does
it avoid a certain pitfall in relation to a given feature?  And so
on.

In the end, tests have definitely identified places where my
implementation exhibited serious flaws and regressions.


## Technical Details {#technical-details}

Lambda Term has [its own dedicated page]({{< relref lambda-term.md >}}) in the Projects section of
this blog.


## And The Hackathon? {#and-the-hackathon}

I had barely managed to get down maybe a page worth of code by the
submission deadline. However, I decided to make this my Boot.dev
capstone project.

[^fn:1]: <https://www.boot.dev/>
[^fn:2]: Dover, 2011. Part of the Dover Books on Mathematics series.
[^fn:3]: While the term "currying" comes from the last name of the
    American mathematician Haskell Curry, I especially like the term
    "currying" because it evokes a similarity to the Latin verb _currere_,
    meaning "to run" (cf. Spanish _correr_). And so I think of the
    consecutive parameters in a curried function as "running across" the
    expression from left to right.
[^fn:4]: I think I abused the heck out of type hints for that
    project. Suffice it to say, the [PEP 8 maximum line length](https://peps.python.org/pep-0008/#maximum-line-length) guideline
    went straight out the window for that project.
