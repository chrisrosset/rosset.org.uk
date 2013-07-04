Everyone familiar with at least some functional programming concepts should be familiar with the concept of `map` -- a simple, yet extremely useful function.

In simple terms, `map` is used to, well, *map* a function to each and every element of a collection in order to produce a collection comprising of the results of applying the function to the elements of the input.

Obviously, this is reflected in the type of `map`:

    map :: (a -> b) -> [a] -> [b]

We can clearly see that it takes a function `(a -> b)` which takes some parameter and produces an output and a list of some values that we will use as inputs to our function.

One of the great advantages of `map` is that is polymorphic. If we look at it's type, we can see that `map` itself doesn't really care what the type of the parameter function. Neither does it put restrictions on the values that it's going to be applying that function too (except that the types match).

So far so good. But one element of that definition is uncomfortably rigid compared to the rest.

The list.

List are great, but they're definitely the be-all and end-all of data structures. Sometimes they're useful, sometimes not so much.

So how do we extend the awesomeness of `map` to work with other datatypes? How do we generalise it?

The answer is the `Functor` class and the `fmap` function whose code is given below.

	class Functor f where
        fmap :: Functor f => (a -> b) -> f a -> f b

So what's changed compared to the regular `map`? We've parameterised the data constructor that is used to contain the values in our input and output.

That way, as long as the datatype we are using is an instance of a `Functor` we can use `fmap` just as we would use `map` on a list.

For example, if we look at the classic binary tree structure:

    data Tree a = EmptyTree | Node a (Tree a) (Tree a)
	deriving (Show)

We can enable the use of `fmap` with it by declaring it an instance of Functor and providing an appropriate function definition.

    instance Functor Tree where
	    fmap f EmptyTree = EmptyTree
		fmap f (Node x left right) = Node (f x) (fmap f left) (fmap f right)

This allows us to use `fmap` with our tree just like we used `map` on a list.

    ghci> fmap (1+) (Node 2 (Node 3 EmptyTree EmptyTree) (Node 4 EmptyTree EmptyTree))
	ghci> (Node 3 (Node 4 EmptyTree EmptyTree) (Node 5 EmptyTree EmptyTree))

In fact, we can make our old, regular `map` a `Functor` too.

    instance Functor [] where
	    fmap = map

These definitions allow us to write our own functions that use mapping without caring about how are our values stored and how exactly the parameter function is applied. All we need to know is that we can accept a new, general type that supports the mapping operation we want to use.
