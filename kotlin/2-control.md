# Kotlin 2: Control structures

In the [previous chapter](1-variables.md), we explained how we can store data, and how to display text.

In effect, this was our first program.

In real life though, programs can do much more than printing a single line of text. They can print two lines of text!

There are two core things we need to be able to create real programs:

- do much more than just printing text,
- be able to control what we do.

For obvious reasons, we won't be able to create games or complex projects in the second lesson: it takes much more to do 3D than simply displaying text. However, control is much easier.

## Conditions

Let us create a simple program that tells us whether a number of even or odd.

```kotlin
// This is a comment. Comments are ignored by the language.
// Use them to write notes to yourself, explain the code, etc.

// First, we want to store the number we are working with:
val number = 5
```

Notice how, unlike previously, we didn't call the value `a`. So far, our examples were very short and easy to understand. In real life, programs can be hundreds of thousands of lines long. It is very important to name things properly, to make code easy to read.

Let us break the problem in its components:

- Test if the number is even or odd
- If it is even, tell the user
- If it is odd, tell the user

In general, breaking a problem into smaller problems helps to develop complex solutions.

##### Test if the number is even or odd

Mathematics tells us that an even number divides 'cleanly' by 2: that is, the reminder of the euclidean division is 0.

Conveniently for us, computers are more or less big calculators. Basic operations, like sum, subtraction, etc, are trivial.

Note that Kotlin makes a distinction between integer and decimal operations. We know the difference between the euclidean division (that has a reminder), and the decimal division (that gives a decimal result). Kotlin will use the euclidean division when both operands are integers, and decimal division if at least one of the operand is a decimal:

```kotlin
// This is an euclidean division
val euclideanDivideByTwo = number / 2

// This is a decimal division
val decimalDivideByTwo = number / 2.0

// This is also a decimal division
val decimalDivideByTwoOther = number.toFloat() / 2
```

Try to guess the type of the three variables, and the value they hold (use `println` to check).

There are 5 operators available on numbers: `+`, `-`, `*`, `/` and `%`. We already know all of those—but the last one. `%` is the reminder of the euclidean division (it's almost as if I planned this). However, we are not really interested in the value of the reminder: we just want to check whether it is 0 (which would mean `number` is even).

We have just seen arithmetic operators, but none of us can help us here. We want to compare values, so we need a few more operators.

To compare two numbers, we can use `<` (less than) and `>` (greater than), that are used exactly the same as in mathematics. These are strict comparison. Non-strict comparison have an additional `=` character, to show that they allow equality: `<=` (less or equal than), and `>=` (greater or equal than). Try to guess the type of these values:

```kotlin
val a = number > 5
val b = number <= 5
```

Both of these values are `Boolean`s, which is a type that only has two values: `true`, and `false`. Booleans are used to store the results of tests.

However, we are interested in equality; and the symbol `=` is already taken to store values. To differentiate `=` ("I want this value to store …") and comparison ("I want to check whether these two things are equal"), we will double it, and use `==` instead.

We can therefore write:

```kotlin
val reminderByTwo = number % 2
val isEven = reminderByTwo == 0

// Prints 'true' if 'number' is even, 'false' if 'number' is odd
println(isEven)
```

We have found a way to test if a number is even! Now, we just need to tell the user in a better way.

##### If it is even, tell the user

We arrive to our first condition. So far, we have always executed each line of code, one after the other. Often, we want to execute different things depending on some condition (on some boolean). We will use the `if` keyword!

```kotlin
if (aBooleanHere) {
	// Our code here
	// Everything in the brackets is only executed if the boolean is 'true'
	// We call the contents of the bracket a 'block'
	println("The condition is true!")
}
```

Note how we have _indented_ everything inside the brackets: all lines inside the brackets start some space after the start of the page. This is very important for readability, and nothing screams "I'm not a good developer" more than improperly indented code. The exact size of indentation has made debate since programming has been created, and many styles exist. In Kotlin, you should always put the opening bracket `{` at the end of the line that opens a block, and the closing bracket `}` on its own line.

```kotlin
// This is good
if (condition) {
	println("so nice")
}
println("so good")

// This is bad
if (condition) {
	println("so bad")
}

// This is bad
if (condition) {
	println("so bad")
} println ("so good")
```

When it comes to indentation size, I personally recommend to use tabulations instead of spaces, and to set your IDE to tabs of size 4. This way, people that prefer a different size can just set their IDE to that size and still use tabs, and everyone feels great about their code. Professionally, by far the majority of Kotlin developers use indentations of size 4 (including all official documentation, examples, etc), however there is no clear majority for the usage of tabs or spaces. No matter what your preference is, a project should follow a single rule: there should never be two files in the same project with different styles.

Another thing to know about brackets is that they control a value's life. Any value created inside a block is destroyed when the block ends.

```kotlin
if (condition) {
	val a = 2
}
println(a) // forbidden, 'a' was destroyed when the block ended
```

Blocks are a major part of the language, and appear everywhere.

We now know how to write our code:

```kotlin
if (isEven) {
	println("The number is even.")
}
```

It would be a bit better if we also displayed the number. We can easily do that by using string templates: the `$` character. Use it to retrieve a value inside of strings.

```kotlin
if (isEven) {
	println("The number $number is even.")
}
```

##### If it is odd, tell the user

We only have to handle the opposite case as well.

The two obvious solutions would be to check for the opposite conditions:

```kotlin
val isOdd = reminderByTwo != 0
if (isOdd) {
	println("The number $number is odd.")
}
```

We could also have simply reversed the condition with the negation operator `!` (read "not"):

```kotlin
if (!isEven) {
	println("The number $number is even.")
}
```

However, in both cases, we are testing the condition _again_. We already tested that the condition is `true`, the language already knows the answer. We can just give a bunch of code to execute when a condition fails, with the `else` keyword.

```kotlin
if (isEven) {
	println("The number $number is even.")
} else {
	println("The number $number is odd.")
}
```

Note that the `else` does not have a condition: it just takes the opposite of the `if` that came just before. Note that we have put the `else` on the same line as the closing bracket. This is the only situation where you should do so: for compound keywords. `else` doesn't make sense alone, it needs to be after an `if`—therefore, it should be placed on the `if`'s closing line.

We have now solved our problem!

Here is the complete code:

```kotlin
val number = 5
val reminderByTwo = number % 2
val isEven = reminderbyTwo == 0
if (isEven) {
	println("The number $number is even.")
} else {
	println("The number $number is odd.")
}
```

Check that it works correctly by changing the value of `number` and running the code again.

## Cleanup

We have written correct code, however it is quite verbose to solve a quite simple problem.

One of the objectives of the Kotlin language is to make code _readable_. In particular, easy problems should not take much code to solve. We will now see how we can simplify the previous code.

First, we stored many intermediary results that we only once. As we saw previously, Kotlin is able to store intermediary results by itself, naming values is just convenient for readability and reuse.

```kotlin
// We can do multiple calculations at once
val a = 5 * 2 + 4 / 4
```

We can remove the temporary `reminderByTwo` and `isEven` to simplify our code.

```kotlin
val number = 5

if (number % 2 == 0) {
	println("The number $number is even.")
} else {
	println("The number $number is odd.")
}
```

Now, we notice that the sentence `The number $number` is written twice. In programming, we should almost never use copy-paste (cut-paste is allowed). The reason is that, as our program grows to thousands a lines that spread through hundreds of files, anyone editing the code will forget to edit the other copy. This is a very prevalent issue in badly written code, so we should learn not to do it from the start.

We can see that only the last word changes. We will create a temporary value to hold that single word. We can do this, because Kotlin allows to _declare_ a value before we _assign_ it:

```kotlin
// We are *declaring* a value 'a', without giving its value
val a: Int

// We are *assigning* 2 to 'a'
a = 2
```

So far, we always wrote those two operations in a single line, but we are also allowed to split them in two lines. Note that when we declare a value without assigning it, we must explicitly tell its type.

We know how to execute different code depending on a condition, so we can write:

```kotlin
val number = 5

// We declare the value without assigning it
val evenOrOdd: String

// We assign it in an 'if', so it has the correct word
if (number % 2 == 0) {
	evenOrOdd = "even"
} else {
	evenOrOdd = "odd"
}

println("The number $number is $evenOrOdd.")
```

Notice that the sentence is now only written once.

However, we can simplify the code further. It is allowed to remove the brackets of `if`s that only have a single line of code, which is the case here:

```kotlin
val number = 5

val evenOrOdd: String
if (number % 2 == 0)
	evenOrOdd = "even"
else
	evenOrOdd = "odd"
println("The number $number is $evenOrOdd.")
```

Note how the indentation keeps it readable.

You should see that IntelliJ adds a squiggly line under the `if`: if you hover it, it says "Assignment can be lifted out of `if`".

This is because, in Kotlin, `if` is both a _statement_ (something that executes code), and an _expression_ (something that has a value). The value of an `if` is the last line of both its blocks (the last line of the `if` and the last line of the `else`).

We can therefore rewrite our code further (our apply the IDE's suggestion, by clicking the 'lamp' icon):

```kotlin
val number = 5

val evenOrOdd = if (number % 2 == 0)
	"even"
else
	"odd"

println("The number $number is $evenOrOdd.")
```

Because the `if` is such a simple case, we can also put everything on a single line:

```kotlin
val number = 5

val evenOrOdd = if (number % 2 == 0) "even" else "odd"

println("The number $number is $evenOrOdd.")
```

Single-line `if`s are very convenient for cases like these, but should not be abused. It is easy to make everything unreadable by overusing these syntaxes. Readable is always better than short.

We have now seen basic conditions, but there is much more to see. Next chapters will extend on this knowledge.
