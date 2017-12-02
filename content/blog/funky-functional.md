---
author: "Robert den Harink"
date: 2017-06-12
title: Funky Functional
subtitle: An introduction to functional programming.
tags: [functional, programming, haskell]
colour: gold
excerpt: |
  Learning functional programming coming from an imperative/OO mindset can be daunting. I got the advice to unlearn programming and start from scratch.
---

It's difficult to completely forget about your old habits and I found you shouldn't, you should be completely open to new concepts and be willing to learn with patience and practice. But the imperative/OO concepts can be used to form better intuitions about functional concepts.

> Disclaimer: The following article is based on my own experience learning functional programming. I still have a long way to go, so please correct me if something seems out of place!
I hope you'll find it helpful in some way

## What is functional programming?
Functional programming is programming with functions. Haha, no really it's just that. But there are some prerequisites.

Functions in "functional land" are pure. A pure function is a function that given the same input it will always produce the same output. This is referred to as *referential transparency*.  Therefore a pure function is a function with no observable side effects. If there are any side effects on a function the evaluation could return different results even if we invoke it with the same arguments.

The longer you think about it, the less it makes sense.

Can do anything useful with it then?

No, you can't if all functions are pure, you would only be heating up your computer.

Luckily functional programming is not about forbidding side effects but about reasoning about them. A function with side effects can be seen as a procedure and is completely valid in a functional language. We only tend to use as few as possible and we use a special structure to perform them called a Monad.

> I would like to point that Monads are not just for IO but far more useful in a range of other areas that I will not go into in the post.

## Types
Before we continue let's settle on some syntax.
The syntax below is used in Haskell and is similar in Purescript or Elm.
```haskell
myFunction :: a -> a
```
The snippet above denotes the type of the `myFunction`. It states that `myFunction` takes a value of type `a` and returns a value of type `a`. A value of type `a` can be anything, we could have also named it `b`, `c` or `whatever` it's just a name.
```haskell
myFunction2 :: a -> a -> a
```
This states that `myFunction2` takes two values of type `a` and returns a value of type `a`.

At a first glance, the signature looks like a function that takes 3 arguments or more like a sequence of things. This is on purpose, it's like mathematical function notation and has something to do with currying.

Currying is a bit out of scope for this post, but in short, it lets you break down a function that takes multiple arguments into a series of functions that take part of the arguments.

### Why are types important?
Types let us reason about the possible implementations of the function. For example, the `myFunction` above only has one possible implementation: to return `a`. Because it can't possibly create a new `a`, or modify `a` since `a` can be anything. The `myFunction2` has 2 possible implementations: it can either return the first `a` or the second `a`. Nothing more nothing less.

The more concrete a function's arguments get (less polymorphic) the more possible implementations there are. for example:
```haskell
myFunction3 :: int -> int
```
The `myFunction3` function could have an infinite number of implementations because it could multiply by 6, always return 0, 1, 2 etc. But it could never return the word "Burrito" or anything other than an `int`.

Types help us reason about our programs before we run it. In most cases, the functional language will compile to an executable program. We call this a _statically typed functional language_.

## Just some words
Below is a list of commonly used words in functional programming. These are terms coming from the mathematical branch of category theory. You don't have to understand category theory to use functional programming, but it is a wonderful topic to read and think about.

The explanations below are meant as a super quick introduction, I advise to read more about them.

#### Identity or Unit
A special element in a set also called the neutral element in mathematics. An example of this is `0` in addition, you can always add zero and the result will stay the same. `1` in multiplication and an `empty list` in lists are also examples of unit values in different sets.

#### Semigroup
Is a set of members together with an associative binary operation.
For example multiplication, a set of numbers and the binary associative operation of multiplication.

#### Associativity
 If you have operations, for example, addition. The order in which these operations are performed does not matter as long as the sequence of the operands is not changed. Rearranging the parentheses in an expression will not change the outcome.
```haskell
1 + 2 + 3 = 1 + (2 + 3) = (1 + 2) + 3
```

#### Monoid
Is a single associative binary operation and an identity element. Every monoid is a semigroup. A monoid allows appending or summing members in a set. This is a destructive operation because once two members are joined they cannot be un-joined.

An example of a monoid is appending strings. The function associated is often called `mappend` for monoid-append. What you should take away here is that the identity element in a monoid is what it differentiates it from a semigroup. A monoid has a way to construct itself from nothing, a semigroup can't.

#### Functor
A functor is a structure that can be mapped over. For example an array. You can apply a function to every element in the array, and you will get back an array.

This is the most basic intuition you can get for a functor. You can see it as a container-ish thing with a value(s) inside. A functor defines a structure to unpack the inner value and apply a function to that value. Eventually, it wraps everything back up in a new functor of the same type.

I refer to a "container-ish" thing because I found it not to fit the analogy of a container or box.  A functor can be used for other things structures like functions. Then the container analogy would no longer fit.

## There is more
Next time I'll post more on Applicative and Monad.
