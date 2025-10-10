+++
title = "Lambda Term"
author = ["Brandon C. Irizarry"]
date = 2025-10-09T00:00:00-04:00
publishDate = 2025-10-09T00:00:00-04:00
lastmod = 2025-10-10T14:36:10-04:00
tags = ["projects"]
draft = false
summary = "A programming language bootstrapped off of lambda calculus."
+++

A programming language bootstrapped off of lambda calculus.


## Repo {#repo}

<https://github.com/BrandonIrizarry/lambda_term>


## Introduction {#introduction}

Lambda Term is an attempt to implement the hypothetical language
described in _An Introduction to Functional Programming Through
Lambda Calculus_ by Greg Michaelson (Dover, 2011). Here's some code
(from the book, page 141) which defines a function that inserts a
string into a list, preserving lexicographic order:

```text
rec INSERT S [] = [S]
or INSERT S (H::T) =
    IF STRING_LESS S H
    THEN S::H::T
    ELSE H::(INSERT S T)
```

Here various features of the language are being demoed, most notably
pattern matching across arguments.

Ideally, I'd be able to get this example to work in Lambda Term (my
name for the implementation), with some differences in syntax.

Currently, the language is somewhat far from this goal. It is
currently capable of defining functions which can handle basic
arithmetic over natural numbers, implemented as Church
numerals. That is, Church numerals (as defined in the book) are the
only meaningful datatype supported.

Here is a functional prelude from Lambda Term, suitable for loading
into a REPL session (more on this in a bit):

```text

# Some bootstrapping definitions. #

def first x y := x;
def true := first;
def second x y := y;
def false := second;
def if cond e1 e2 := (cond e1 e2);

# Note that boolean values are selector functions. #

def not x := (x false true);
def zero := \x.x;
def iszero n := (n first);
def succ n := \s.(s false n);
def pred1 n := (n second);
def pred n := (if (iszero n) zero (pred1 n));

# Using 'succ' to define the natural numbers from 1 to 10. #

def one := (succ zero);
def two := (succ one);
def three := (succ two);
def four := (succ three);
def five := (succ four);
def six := (succ five);
def seven := (succ six);
def eight := (succ seven);
def nine := (succ eight);
def ten := (succ nine);

# Various arithmetic operations. #

def add x y := (if (iszero y) x (add (succ x) (pred y)));
def sub x y := (if (iszero y) x (sub (pred x) (pred y)));
def mult x y := (if (iszero y) zero (add x (mult x (pred y))));

let abs_diff x y := (add (sub x y) (sub y x))
in def equal x y := (iszero (abs_diff x y));

def greater x y := (not (iszero (sub x y)));
def greater_or_equal x y := (iszero (sub y x));
def less x y := (not (greater_or_equal x y));
def less_or_equal x y := (not (greater x y));

```

Division is currently missing from this listing, as it's under
construction.

You might've noticed that some definitions are recursive. This works
out of the box (for example, without the Y combinator), since global
symbols are resolved only when needed (more on this later.)


## Syntax {#syntax}

The following represents the _core_ grammar of the language, not
augmented by assignment:

TERM := NAME | FUNCTION | APPLICATION
NAME := &lt;any valid C identifier&gt;
FUNCTION := \\.TERM
APPLICATION := (TERM TERM+)

To keep things simple, the backslash character (\\) is used where 'λ'
would otherwise be used.

Also, function application supports multiple arguments as a convenience,
to avoid excessive nesting of parentheses (hence the "+" in the
definition of APPLICATION). For example:

```text
((\x.\y.x \x.x)   \f.\a.(f a))
```

can be rewritten simply as

```text
(\x.\y.x   \x.x   \f.\a.(f a))
```

This allows us to think of functions as accepting multiple arguments
(though currently only single-parameter functions are supported.)
Note that this example selects the first of its arguments, and so the
result is

```text
\x.x
```


## The REPL {#the-repl}

Currently, a REPL is available for evaluating Lambda Term
expressions. To launch a REPL session enriched with some definitions
from a file called `prelude`:

```text
./lambdaterm.py -l prelude
```


#### Prettification {#prettification}

[DeBruijn indices](https://www.cs.cornell.edu/courses/cs4110/2018fa/lectures/lecture15.pdf) are used to avoid name-clashes during
beta-reduction, allowing the implementation to discard the
original names given to function parameters. Now, when the time
comes to print the result of an evaluation in the REPL, one option
is to print out the AST of the result. While useful for debugging,
this is altogether unsatisfying and difficult to read. Hence, a
prettified form of local names is used to reconstitute the various
numericized local variables inside an expression. The solution I
went with involves filling in random dictionary words for these
names.

Because of this, if you try to evaluate `\x.x` at the REPL, you
may well see something like

```text
\glorious.glorious
```

or

```text
\debonair.debonair
```

as the evaluation result. This is consistent with the fact that
all three expressions are valid representations of the identity
function.
