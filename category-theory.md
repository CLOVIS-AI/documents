# Category Theory: Cheatsheet

## Introduction

### Basics

##### Object

An object is just a ‘thing’. Objects do not have any particular properties, apart from having a name: for example, `a`, `b`…

##### Arrow, morphism

An arrow (or morphism) goes from one object to another (or to the same one). They are written `arrowName :: start -> finish` (Haskell notation). Two arrows sharing the same starting and ending object are not necessarily the same arrow.

##### Identity

The identity arrow is an arrow that goes from an object to itself. It is often called `id`.

##### Composition

An arrow that ends at an object and an arrow that starts at this object can be combined into a single arrow:
If there are three objects `a`, `b` and `c`, and two arrows `f :: a -> b` and `g :: b -> c`, we have `g ○ f :: a -> c` (pronounced ‘`g` after `f`’).

##### Hom-Set

The Hom-Set from `a` to `b` is the set of all arrows that start at `a` and end at `b`.

### Category

A category is a bunch of objects and arrows that must follow these rules:

- Each object must have an identity: for all `a`, there is `id_a :: a -> a`,
- Each category must allow its arrows to be composed,
- The composition of two arrows must be associative (meaning that `h ○ g ○ f == (h ○ g) ○ f == h ○ (g ○ f)`; parenthesis can be removed as they hold no meaning).

Many ‘well known’ categories exist, including the Category of Types and Functions, which describes most programming languages. This document will often take it as an example, but (unless specified)
everything works with all categories.

- It is always possible to define a function from a type to itself,
- Two functions can be composed using lambdas: `g ○ f` is a function that takes `f`'s parameter, gives `f`'s result as a parameter to `g` and returns `g`'s result; it can often be written as `g(f(x))`.
- Function composition is always associative in programming languages that have functions as first-order objects or have lambdas (Java, Kotlin, C++, Python, JS, …), but the proof depends on the language.

Other common examples are the Category of Sets (in which objects are sets and arrows are relationships; this theory is useful as the bridge between Category Theory and Set Theory, the basis of modern mathematics) and the Category of Categories.

##### Reversible category

A category in which every arrow has an opposite arrow (for each `f :: a -> b`, there is a `f' :: b -> a`).

##### Small and large categories

If objects of a category form a set, then we call it a “small category”. Categories in which objects do not form a set are called “large categories”.

### Concrete Category

A concrete category is a category in which objects are sets of objects, and arrows are sets of arrows on those sets' elements.

Again, the Category of Types and Functions is one of those: each type is a set of objects (each type is a set of instances of that type), and functions go from sets to other sets (an arrow `f :: a -> b` goes from instances of type `a` to instances of type `b`).

##### Domain, co-domain, image

The following vocabulary is valid for all categories, but is much easier to explain for concrete categories:

**Domain**: The domain of an arrow is the set of objects it starts from.

**Co-domain**: The co-domain of an arrow is the set of objects it goes to.

**Image**: The image of an arrow is the set of objects that can be reached by following it (it is a subset of the co-domain).

As an example, let's take the C function `int sqr(int x) { return x*x; }`.

- Its domain is `int` (the parameter)
- Its co-domain is `int` as well (the return type)
- Its image is the set of all positive `int`s (it is not possible to return a negative integer).

#### Epimorphism

An epimorphism is an arrow that has equal co-domain and image (it's possible to reach any object of the co-domain).

- `+1 :: int -> int`: ✓ it's possible to get any `int` (we do not care about overflows here).
- `^2 :: int -> int`: ✗ it's impossible to find -5, even though it is an `int`.
- `^2 :: int -> unsigned int`: ✓ it's possible to find any `unsigned int` if we start at the correct `int`.

#### Monomorphism

A monomorphism is an arrow that only has one way to reach a certain point.

- `+1 :: int -> int`: ✓ the only way to get 5 is to start with 4.
- `^2 :: int -> int`: ✗ it is possible to get 4 by starting at 2 and -2.

#### Isomorphism

An isomorphism is an arrow that goes from every object of the domain to exactly one object of the co-domain, such that there is a single way to find each object of the co-domain. In a reversible category, an isomorphism allows to prove that the domain and co-domain are equivalent (or ‘isomorphic’, hence the name).

- `toString :: char[] -> String`: ✓ every given `char[]` will give a different unique `String`. Because the Category of Types and Functions is reversible, this morphism is a proof that `char[]`
  and `String` are equivalent types: nothing is lost by converting back and forth.
- `+1 :: int -> int`: ✓ we can reach every `int` in exactly one way (again, ignoring overflows).
- `^2 :: int -> int`: ✗ it's not possible to reach -3.
- `^2 :: int -> unsigned int`: ✗ there are multiple ways to reach 4.

### Orders

Orders are categories in which arrows represent relationships between objects (for example, `<=`). In these categories, arrows cannot be represented as functions (they go from one element of the relationship to another, not from a parameter to a result).

##### Pre-Order, thin category

A pre-order is a category built by a relationship that respects the normal rules of categories, but nothing more. Two arrows that share the same starting and ending point must be the same arrow. Opposite arrows are allowed. By definition, all arrows are monomorphisms and epimorphisms.

##### Partial order

A partial order is a pre-order that doesn't have cycles (its graph representation is a tree). They cannot be reversible.

##### Total order

A total order is an order in which there exists an arrow between every two objects (it's possible to compare any two objects).

- `<= :: int -> int`: ✓
- `sublist :: List<Int> -> List<Int>`: ✓

##### Thick category

A thick category is a category in which at least two different arrows that start on the same object, and end on the same object.

### Categories with very few objects

##### Void

Let us define a category with no objects. We can still define arrows `Void -> a`, but how would you ever use it? Since this is arrow cannot be used, it's called `absurd`, and it is sometimes used in demonstrations. An arrow `a -> Void` is also useless: if it was a function, it would be a function that _cannot_ return (there are no elements it can return): we can deduce that `Void` is the return type of infinite loops. The `Void` category doesn't have arrows nor composition, but since there are no objects either this is not an issue.

- Do not mistake this category with most imperative languages' `void`.
- In Kotlin, this category is called `Nothing`.

##### Unit

`Unit` is the name of the category that only has a single object. In this category, all arrows are composable (they all go from `Unit` to `Unit`). It is possible to define an arrow from any category to `Unit` and vice versa. Categories that are isomorphic with `Unit` are called `Monoid`.

- In imperative languages, it is often called `void`, which can be confused with the `Void`
  category.
- In object langages, it is also known as the Singleton pattern.
- In Kotlin, it is called `Unit` (and the keyword `object` allows to create types isomorphic to it).
- We can notice that the category where objects are `String` and where arrows are concatenation is a concrete `Monoid`: all arrows go from `String` to `String`.
- The `Monoid` category is equivalent to the Monoid concept in Set Theory (which defines a set with a default element and an operation).

##### Boolean

`Boolean` is the category with two objects, `true` and `false`.

Because objects do not hold any special meaning, all categories with zero objects are isomorphic to `Void`, all categories with a single object are isomorphic to `Monoid` and all categories with two objects are isomorphic to `Boolean`.

## More about categories

### Kleisli

Pre-requisites:

- Let us select an arbitrary category, C.
- We will suppose C has three objects, `a`, `b` and `c`, with arrows `a -> b` and `b -> c` (and identities).
- For any monoid M, we will call an ‘embellished type’ the category in which `a`, `b` and `c` are replaced by a pair of `a` and M, the pair of `b` and M, and the pair of `c` and M. We will call this category K.
- Let us define that for any arrow `f: a -> b` in C, an arrow `f': a -> (b, M)` in K.

As an example, let's take M to be `String`. For each arrow in C, we can define its ‘embellished version’ as:

- Identity: `id_a: a -> (a, "")`
- Arrows: `f: a -> b` becomes `f': a -> (b, "some string")`
- Composition: `(b -> c) ○ (a -> b) -> (a -> c)`
  becomes `(b -> (c, s')) ○ (a -> (b, s)) -> (a -> (c, s+s'))`

The Kleisli Category of C and a monoid M is the category that has the same objects as C, but in which all arrows are replaced by an embellished version by M.

### Defining Set Theory concepts in Category Theory

##### Terminal object

An object from which a unique arrow comes from any other object.

It is not possible to have multiple terminal objects.

For example, in a category with four objects `a`, `b`, `c` and `d`, and the arrows `f: a -> d`
, `g: b -> c`, `h: c -> d`, `d` is a terminal object because each object has a single arrow to `d`:

- `f: a -> d`
- `g ○ h: b -> d`
- `g: c -> d`
- `id_d: d -> d`

If the arrows represent `<=`, then the terminal object is the maximum.

##### Opposite category

The opposite category of C is the category that has the same objects as C, but in which all arrows are reversed.

The opposite category is useful for proofs: for example, we can define the 'initial object' of a category C as being the terminal object of its opposite category C'.

When we prove something using the opposite category, we usually append ‘co-’ to its name.

##### Initial object

The object from which there exists a unique arrow going to all other objects (
it's the opposite of the terminal object).

##### Product

Let us take two objects `a`, `b`, as well as:

- `c` such that `fst: c -> a` and `snd: c -> b`
- `c'` such that `fst': c' -> a` and `snd': c' -> b`
- etc

We will attempt to sort all `c`, `c'`… objects: if there exists an arrow `c' -> c`, then `c` is "better" than `c'`.

We define the product of `a` and `b` as the object `c` that is ranked the best.

More practically, the product of `a` and `b` could be a pair `(a, b)`.

In the category of Types and Functions, the product could be:

```haskell
-- Haskell
data Point = P Float Float
```

```kotlin
// Kotlin
class Point(val x: Float, val y: Float)
```

##### Co-product, sum

The co-product follows a similar proof to the product, but in the opposite category:

- We have two objects `a` and `b`
- `c` such that `fst: a -> c` and `snd: b -> c`
- `c'` such that `fst': a -> c'` and `snd': b -> c'`
- etc
- We sort the `c`'s: if there is an arrow `c -> c'`, then `c` is "better" than `c'`
- The best `c` is called the co-product of `a` and `b`.

In Set Theory, the co-product of `a` and `b` could be the discriminated union of `a` and `b` (if there is common data in `a` and `b`, it will appear twice in the co-product, not once).

In the category of Types and Functions, the co-product of two types `a` and `b` is a sum type (`enum class`, `sealed class`). In Haskell:

```haskell
-- sum type/co-product of a and b
data Either a b = Left a | Right b

-- a variable
x :: Either Int Bool

-- a function
f :: Either Int Bool -> Bool
f (Left i) = i > 0
f (Right b) = b
```

## Functors and Algebraic Data Types (ADTs)

### Regular functor

#### Definition

Let us take the example of two categories A and B.

- A has objects `a`, `b`, `c`
- A has `f: a -> b`, `g: b -> c` (which are therefore composable)

A functor is mapping from A to B, such that:

- Objects in A are mapped to an object in B (let's call them `a'`, `b'`, `c'`)
- Arrows are mapped to another arrow with the same start and end (there must exist `f': a' -> b'` and `g': b' -> c'`)
- Composable arrows much remain composable (`f` and `g` were composable, so `f'` and `g'` must be as well)

Functors must map every object and every arrow, however they can map to a subset of the category they map to.

In small categories, functors are regular functions on objects.

##### Faithful, full, constant, endofunctors and bifunctors

- A **faithful functor** is a functor that is injective on Hom-Sets.
- A **full functor** is a functor that is surjective on Hom-Sets.
- The **constant functor** is the functor that maps a category to a single object of another category. It's written `Δc` where `c` is the object it maps to.
- An **endofunctor** is a functor from a category to itself. In programming, all functors are endofunctors.
- A **bifunctor** is a functor from a product category to another category.

##### Cat: the category of categories  

- The **identity functor** is the constant endofunctor.
- Functors can be composed.
- The **Cat** category is the category in which arrows are functors, and objects are categories.

#### In Haskell

A functor in Haskell is a typeclass, and is implemented like so:
```haskell
class Functor f where
  fmap :: (a -> b) -> (f a -> f b)
```

An implementation (for example `List`) could look like this:
```haskell
data List a = Nil | Cons a (List a)

instance Functor List where
  fmap _ Nil = Nil
  fmap f (Cons head tail) = Cons (f head) (fmap f tail)
```

### Bifunctor

##### Product category, bifunctor

For two categories `C` and `D`, we can define the category of pairs of `C` and `D`, called `C×D`.
In `C×D`, objects are pairs of objects from `C` and `D`, and arrows are pairs of arrows from `C` and `D`.
These objects and arrows follow category rules:
- For category `C` with `c`, `c'` and `f: c -> c'`,
- For category `D` with `d`, `d'` and `g: d -> d'`,
- `C×D` has the objects `(c, d)` and `(c', d')`,
- `C×D` has the arrow `(f, g)`
- `C×D` has the identities `id_(c, d) = (id_c, id_d)`

We call the mapping from `C` and `D` to `C×D` a “categorical product” (since it creates a product category).

A bifunctor is a functor from a product category to another category (whereas a functor is written `C -> D`, a bifunctor is written `C×D -> E`).

Therefore, the categorical product is the mapping from `C` and `D` to `C×D`, which could be written `(C) × (D) -> (C×D)`, and is a bifunctor.

With a similar definition, we can show that the categorical sum is a bifunctor as well (although there is only one value, it is still a mapping from two types to another type).

##### Monoidal categories

The categorical product has a 'neutral element': the terminal object.
Because of this, the categorical product is a Monoidal category.

Similarly, the categorical co-product is a Monoidal category with the initial object as a 'neutral element'.

More generally, Monoidal categories have a tensor product (written ⨂), with a unit (written 1).

##### In Haskell

```haskell
class Bifunctor f where
  bimap :: (a -> a') -> (b -> b') -> (f a b -> f a' b')
```

A good example of a Bifunctor is Either:

```haskell
data Either a b = Left a | Right b

instance Bifunctor (Either a b) where
  bimap fa fb (Left a) = fa a
  bimap fa fb (Right b) = fb b
```

### Functoriality of ADTs

##### Demonstration in Haskell

```haskell
-- The simplest functor:
-- It's a functor of a, but it disregards a
-- It can be thought of as the container that is always empty
data Const c a = Const c

instance Functor (Const c) where
  -- fmap :: (a -> b) -> Const c a -> Const c b
  -- Because we don't 'store' any value, we can ignore the function f
  fmap f (Const c) = Const c
```

```haskell
-- The functor that stores 1 element
data Identity a = Identity a

instance Functor (Identity a) where
  -- We can just apply f to the contents
  fmap f (Identity a) = Identity (f a)
```

We now have a functor of size 0, a functor of size 1, a functorial product, and a functorial sum.
Because ADTs are formed with those elements, and functors are composable, then all ADTs are functorial as well.

For example, we can deconstruct our types into functors:
```haskell
-- Let's take the example of Maybe
data Maybe a = Nothing | Just a

-- This type is mathematically identical:
Either () (Identity a)

-- Because unit is a set of size 1, we can replace it with Const:
Either (Const () a) (Identity a)

-- Either is a bifunctor, and a functor,
-- Const is a functor, Identity is a functor,
-- functors are composable, so Maybe is a functor.
```

This procedure of proving functors can be automated: `{-# LANGUAGE DeriveFunctor #-}`.

### Profunctor

##### Contravariant functor

```haskell
class Contravariant f where
  contramap :: (b -> a) -> (f a -> f b)
```

A contravariant is a "negative container": it expects `a` instead of 'storing' it.

##### Co-functor, profunctor

The co-functor (functor in the opposite category) is usually named profunctor (for historical reasons).

```haskell
class Profunctor p where
  dimap :: (a' -> a) -> (b -> b') -> p a b -> p a' b'
```

As an example, we can take a simple function type: `a -> b`. Function types are profunctors. The parameters we get are:

- `f: a' -> a`
- `g: b -> b'`
- `h: a -> b`
- We want to create `res: a' -> b'`

We can easily find that `res = g ○ h ○ f`. This makes sense, because:

- We first use `f` (a contravariant functor) to create an `a`,
- We then use `h` to convert it to a `b`,
- And finally we use `g` (a regular functor) to get a `b'`.

### Function object

##### Definition

A function can be seen as a morphism from the product (tuple) of the function name and its arguments, to its return type. Because of this, we will always define the product before we can define functions.

This way, if we have an object `z` which represents a function object that expects a parameter `a` and returns `b`, then the function is written as the morphism `eval: z × a -> b`.

By universal construction, if there are two functions `z` and `z'` from `a` to `b`, and there is a morphism `h: z' -> z`, then we define that `z` is a better candidate function object than `z'`. We now call the best candidate a "function object", and write it `a => b`.

Hence, for any candidate `z'` of function object, we can define `eval': z' × a -> b`, which can be rewritten as `eval' = eval ○ (h × id_a)` (using the functioriality of the product). We see that `h` and `eval'` have a 1–1 relationship, which allows us to prove that these two writings are equivalent:

- `h :: z' -> z`, or, as we've seen, `h :: z' -> (a => b)`
- `eval' :: z' × a -> b`

Because we know these two writings are equivalent, we can decide:

- That it isn't important to use a different notation for function objects, so we allow ourselves to use the normal arrow and rewrite `h` as `h :: z' -> (a -> b)`.
- We now have proved that an arrow of a single argument that maps to an arrow of a single argument that maps to a result (`f -> (g -> h)`) is equivalent to an arrow of two arguments that maps to a single result (`f × g -> h`); this is Curry's theorem.

##### A function object is an exponential

Let's take the example of a function `Bool -> Int`.

- It can only have two return values; we can therefore encode it as the `Int` that is returned when `true`, and the `Int` returned when `false`: we can encode our function as a pair of two ints, `(Int, Int)`.
- The type of this function can therefore be encoded as the set of all possible pairs of `Int`, which can be written `Int × Int`, or even `Int²`.
- As we've seen when we defined the product, when we talk about types (or, in a general sense, categories), we can write `Unit` as 1, `Boolean` as 2, etc.
- We can then rewrite `Int^2` as `Int^Boolean`.

Thanks to curryfication, we can rewrite any function of multiple arguments to functions of single arguments that return functions, which means:

- `(Int, Int) -> Boolean` can be rewritten as:
- `Int -> Int -> Boolean` which can be rewritten as:
- `Boolean^Int^Int`.

We call categories that support function objects "Cartesian-close categories".

- For any pair of objects `a` and `b`, they have an exponential `a^b`,
- They have a terminal object (`a¹` is `a`, `a²` is a pair of `a`, `a⁰` is the terminal object).
