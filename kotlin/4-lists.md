# Kotlin 4: Lists

In the previous chapters, we've seen how to store data in RAM, and how to conditionally execute different code.
Today, we will see how we can combine both of those topics.

## Introduction

In [chapter 2](2-control.md), we printed whether a number was odd or even.
What if we wanted to do that for multiple numbers?

The only thing we know how to do so far, would be to type in a number, run the program, type another number, run the program again, etc.
Not only is this inconvenient, it _scales badly_: for all new values, it's a lot of work!

In the previous chapter, we've seen how to run operations multiple times using a loop.

## What are lists?

So far, we've written variables like:
```kotlin
val a: Int = 5
```

Lists are data structures that allow to store multiple objects of the same type, inside the same variable.
```kotlin
val b: List<Int> = // ??
```
The type `List<Int>` corresponds to a variable that stores multiple `Int`s.

Each integer stored inside the list can be manipulated via the bracket operator (`[]`):
```kotlin
val first = b[0]  // "get the element #0 in the list 'b'"
val second = b[1] // "get the element #1 in the list 'b'"
val third = b[2]  // "get the element #2 in the list 'b'"
```
As is often the case, in programming we count starting from 0: the first element is ranked 0th, not 1st.

Much like with regular variables (`val` vs `var`), there are two versions of lists: _read-only lists_, and _mutable lists_.

> **Note.** Many people call read-only lists "immutable lists". This is incorrect, and will become problematic in the future (there are plans to add real immutable lists).

## List creation

### Create a non-empty list

To create a list that already has elements, we can use:
```kotlin
val readOnlyList = listOf(5, 29, 674)       // the type is List<Int>
val mutableList = mutableListOf(19, 34, 95) // the type is MutableList<Int>

println(readOnlyList[0]) // prints 5
println(readOnlyList[1]) // prints 29
println(readOnlyList[2]) // prints 674

println(mutableList[0])  // prints 19
println(mutableList[1])  // prints 34
println(mutableList[2])  // prints 95
```

Mutable lists can be edited:
```kotlin
mutableList[0] = 5       // replace the 0th number with 5
println(mutableList[0])  // prints 5

mutableList += 6         // adds a 6 at the end of the list
println(mutableList[3])  // prints 6
```

To get the size of a list:
```kotlin
println(readOnlyList.size) // prints 3
println(mutableList.size)  // prints 4
```

### Creating an empty list

To create an empty mutable list, we can simply use:
```kotlin
val list = mutableListOf<Int>()
```
Afterwards, we can use `+=` to add elements to it.
Note that we have to specify the type of the contents of the list: when we give elements, Kotlin can guess which type we mean, but on an empty list it can't guess what we will put inside it in the future.

To create an empty read-only list, we *could* use:
```kotlin
val empty = listOf<Int>()
```
However, this is not optimal.
An empty read-only list can never be modified, and it doesn't store anything.
This means that *all* empty read-only lists are identical.
Instead of creating a new one, we could just reuse the same one multiple times, which is done via the `emptyList` function:
```kotlin
val empty = empyList<Int>()
```

## List usage

Our example was to check if multiple numbers were even or odd.
Now that we have both lists, and loops, we can write that code easily:
```kotlin
// Create a list of 4 numbers
val numbers = listOf(1, 456, 6784, 156)

// Loop: 'i' will go from 0 to the end of the list (exclusive)
for (i in 0 until numbers.length) {
	// Get the #i number from the list
	val number = numbers[i]
	
	print("The number $number: ")
	// Check if it's even or odd (same code as in previous chapters)
	when (number % 2) {
		0 -> println("is even.")
		else -> println("is odd.")
	}
}
```

That last example is how we often write code in older languages, however in Kotlin we can do better.

In the [loops chapter](3-loops.md) we explained that the `for` loop works using ranges (created using `..`, `until`, `step`, etc).
In Kotlin, `for` works using any _iterator_, of which ranges are one particular case.
Lists also are iterators.
```kotlin
val numbers = listOf(1, 456, 6784, 156)

// Loop: 'number' will be a different number from the list each loop
for (number in numbers) {
	print("The number $number: ")
	when (number % 2) {
		0 -> println("is even.")
		else -> println("is odd.")
	}
}
```

Note that we lost the access to the index of the number.
In most cases, the index is not important.
For example, if you want to print all numbers:
```kotlin
val numbers = listOf(1, 2)
for (number in numbers) {
	println(number)
}
```
In some cases, however, we do want to use the index.
For example, if we want to print every other number, we can use:
```kotlin
val numbers = listOf(1, 2, 3, 4)
for (i in numbers.indices) {
	if (i % 2 == 0) 
		println(numbers[i])
}
```

Of course, instead of printing the results, we could store them in another list:
```kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6)
val results = mutableListOf<Int>()

for (i in numbers.indices)
	if (i % 2 == 0)
		results += numbers[i]
```

## Conclusion

We now know:
- How to store a single value,
- How to store multiple values in one,
- How to execute different code depending on some condition,
- How to execute the same code multiple times at once.

This is almost all we need.
Next chapter will mention functions, the last missing piece.
After that, everything else is to help structure the code, but we'll have seen all the basic pieces.
