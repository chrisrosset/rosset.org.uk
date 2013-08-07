----
title: How Kind of You
tags: haskell kinds types
----

Compared to most programming languages, Haskell puts an incredible focus on types; and for good reason. Types simply reasoning about the code and are great safeguard against all kinds of nasty bugs that love to find their way into programs. Types make the programmer be explicit in their actions which, in general, appears to lead to a more clear style of programming.

Ok -- types are good.

We can look at some values and we can describe them by a type.

    'a' :: Char
    666 :: Num a => a
    map :: (a -> b) -> [a] -> [b]

So far so good.

But what about these?

    Int
    Maybe
    Either

Well, Int is a type. But the other two? They obviously aren't values. They're not data constructors either. They're pretty close to being regular types, but not exactly -- they're all missing their type parameter (`Maybe a` would be a type; the `a` is missing though here).

We can draw a parallel to functions. A function that takes `n` arguments can be given fewer arguments than `n`. The result of this is yet another function, that takes the parameters that it hasn't been provided yet (this is called [Partial application](http://www.haskell.org/haskellwiki/Partial_application)).

    add3 :: Int -> Int -> Int
    add3 a b c = a + b + c
    add3 5 5 :: Int -> Int

In the example above, we supply our function (that takes three integers and adds them together) with two numbers which gives us a function that adds 10 to its only argument (this new function only takes one argument).

So, how do reason like this about the incomplete types mentioned above?

_Kinds_ are the answer. These can be inspected in `ghci` by using `:k` similiar to how `:t` can be used to query the interpreter for the type of a value. Let's take a look at what `ghci` produces for our troublemakers. (Note: in order to be able to inspect kinds, you need to use a GHC extension: `KindSignatures`. This can be enabled either by using a pragma or running GHC with an `-XDataKinds` parameter)

    ghci> :k Int
    Int :: *
    ghci> :k Maybe
    Maybe :: * -> *
    ghci> :k Either
    Either :: * -> * -> *

These do look a lot like partially applied functions -- except we've got stars instead of the usual type variables. The `*` is a placeholder for a type. Looking at the `Int` example, we see that it's just a type. The other two, however, are incomplete and that's reflected in their kind signatures. `Maybe` requires a type parameter to return a type. Similarly, `Either` needs two types to return a type.


## Acknowledgement

I'd like to thank dr Nicolas Wu, my MSc project supervisor, who spent a good deal of time trying to explain this concept to me. He wrote [a short introductory post](http://zenzike.com/posts/2010-11-04-a-kind-introduction) on this subject as well which I used as a reference too and recommend.

