# Kotlin 1: Variables

In the [previous chapter](0-getting-started.md), you should have created a scratch file in IntelliJ, with the following code:

```kotlin
println("Hello World")
```

In the next few chapters, we will write a simple 'more or less' game: the computer will select a random number, and will try to guess which it is by asking if it is 'more' or 'less' than another number.

## Basic arithmetics

Before we start writing real code, let us play around with numbers.

```kotlin
println(2)
println(2 + 2)
println(2 * 5)
println(100 + 2 * 154 / 15)
```

As you can see, it is easy to ask the computer to perform basic maths.

Now, let us explain the code:

- `println` is a _function_. Functions perform an _action_. `println` just _prints what you give it_ (the `ln` stands for "line").
- parens, `()`, are used to tell the language that we want to _call_ a function. Inside the parens, we give the data we want the function to work with.

Functions are the basic building block of code: they allow us to do complicated things with simple code (do you have any idea what goes on in the computer when you ask it to print stuff?).

Now, try:

```kotlin
println(3 + 3 * 3)
```

You will notice that Kotlin respects the mathematical priorities of operators.

If we break down what happened in multiple steps, we get:

- The language calculated `3 * 3`,
- It added `3` to the previous result,
- It then called `println` with the previous result.

Notice that we often need to work "with the previous result": a computer breaks down your code in very small steps, and stores intermediary results.

Of course, if the computer needs to store intermediary results, we should be able to as well.

## Variables

```kotlin
val a = 3 * 3
```

Here, we create a value (a temporary result) named `a`, that stores the value `3 * 3`.

Storing a value does absolutely nothing that can be observed by the users of your program. In the scratch file, you should see that it does display that you created a variable, but in dark grey (because it's an information the language gives you so you can understand what it does, but it's not actually something that will be displayed when you run it for real).

We can now use that stored result:

```kotlin
println(a)
println(a + 2)
println(a + 5)
```

We can use stored values as many times as we like. Note that they are not stored forever: everytime you execute the program, they are reset. This is because values have a "scope": when the thing that created them dies, they are deleted as well. Currently, we create the value in no particular scope (we will discuss those later), so the value lives as long as our program.

Notice what IntelliJ displayed on the right side of the screen:

```kotlin
val a: Int
```

Kotlin has guessed that our value `a` is an `Int` (integer). This is called a "type", and it allows the language to check that what we're doing is correct.

Run the following examples, and see what types it gives you:

```kotlin
val b = 2
val c = 2 + 5

val d = b > c
val e = true
val f = d && e

val g = "hello"
val h = 'h'
val i = g + h
```

Notice that:

- It guesses the types `Int`, `Boolean` (something that is true or false), `String` (some text) and `Char` (a single character).
- Operator `>` works on two `Int`s but gives a `Boolean`
- Operator `&&` works on two booleans (it's the logical operator AND)
- Operator `+` used on two `Int` gives a `Int`, but when used on a `String` and a `Char` it gives a longer `String`.
- Try to use other combinations of operators, and you'll notice that it either does the right thing, or complains that it's not legal (underlined in red, and your code doesn't run anymore).

Don't hesitate to add a few `println` to see what is inside those values.

The Kotlin language is quite good at guessing which type a value is, however sometimes it can give you something you disagree with, or you just want to be explicit for readability. In that case, you can actually use the syntax it shows you to define the type as well:

```kotlin
val j: Int = 2
```

There are actually other number types in Kotlin. The ones you will use the most are `Int` (integer, positive or negative) and `Double` (decimal, positive or negative), but there are a few more:

```kotlin
val k: Long = 2
val l = 2.0
```

These different types exist for performance reasons: they all have different minimum and maximum values. To ask what their values are, use:

```kotlin
println(Int.MAX_VALUE)
println(Int.MIN_VALUE)
```

and replace `Int` by the type you're interested in.

For now, just keep in mind that `Byte` < `Short` < `Int` < `Long` for integers, and for decimals `Float` < `Long`.

## Conclusion

We've now seen the basic building blocks of programming:

- We can print data to the screen with `println`,
- We have discussed what functions are in general,
- We can store data with `val`,
- We know that a computer understands different types of data, and will check that everything we do is valid according to types.
