# Kotlin 3: Control structures (part 2)

In the [last chapter](2-control.md), we have seen how to control the flow of execution of a program conditionally.

We will now see other ways to control the execution of a program.

## Nested ifs

Last time, we finished with a really simple example:

```kotlin
val number = 5

val evenOrOdd = if (number % 2 == 0) "even" else "odd"

println("The number $number is $evenOrOdd.")
```

We will now look at leap years.

Here is the logic:

- A leap year occurs every 4 years,
- Every 100 years, there is no leap year (even though there should be one according to the previous rule),
- Every 400 years, there is a leap year (even though there shouldn't be one according to the previous rule).

```kotlin
val year = 12345

val isLeapYear: Boolean

// Logic here

val leapYearMessage =
	if (isLeapYear) "is a leap year"
	else "is not a leap year"
println("The year $year $leapYearMessage")
```

Using our previous knowledge, we can write this:

```kotlin
if (year % 400 == 0) {
	isLeapYear = true
} else {
	if (year % 100 == 0) {
		isLeapYear = false
	} else {
		if (year % 4 == 0) {
			isLeapYear = true
		} else {
			isLeapYear = false
		}
	}
}
```

You can easily see that the code is going to be unreadable very fast if we nest these more. For this reason, we can join an `else` with an `if`:

```kotlin
if (year % 400 == 0) {
	isLeapYear = true
} else if (year % 100 == 0) {
	isLeapYear = false
} else if (year % 4 == 0) {
	isLeapYear = true
} else {
	isLeapYear = false
}
```

Now we know how to write multiple conditions cleanly!

It turns out, however, that we can make it much better.

##### Version 1: removing code duplication

First, let's remove brackets and put the assignment outside of the conditions:

```kotlin
isLeapYear =
	if (year % 400 == 0) true
	else if (year % 100 == 0) false
	else if (year % 4 == 0) true
	else false
```

If you wrote this in IntelliJ, you will have noticed a brown line under the second `if`. This is because we are using a condition to create a boolean:

```kotlin
// Simplified example
val something =
	if (someCondition) true
	else false
```

Here, `someCondition` is already a Boolean: we are writing "if `someCondition` is `true` then `true`, if `someCondition` is `false` then `false`". This is completely redondant, we could just use `someCondition` directly:

```kotlin
val something = someCondition
```

Let's take another look at our previous code:

```kotlin
isLeapYear =
	if (year % 400 == 0) true
	else if (year % 100 == 0) false
	else if (year % 4 == 0) true
	else false
```

The last `if…else` gives the condition itself. We can rewrite it:

```kotlin
isLeapYear =
	if (year % 400 == 0) true
	else if (year % 100 == 0) false
	else year % 4 == 0
```

Our code can be simplified further:

```kotlin
isLeapYear = year % 400 == 0 || (year % 100 != 0 && year % 4 == 0)
```

However, not everyone agrees that this is easier to read than the previous version. Let's see another way to clean it up.

##### Version 2: using 'when'

The `when` keyword is a simplification of nested `if` statements.

```kotlin
// Using if…else
if (someCondition1) 
	someValue1
else if (someCondition2) 
	someValue2
else if (someCondition3) 
	someValue3
else 
	someValue4

// The exact same code, using when
when {
	someCondition1 -> someValue1
	someCondition2 -> someValue2
	someCondition3 -> someValue3
	else -> someValue4
}
```

Using `when`, we can rewrite our previous code:

```kotlin
// with if…else
isLeapYear =
	if (year % 400 == 0) true
	else if (year % 100 == 0) false
	else if (year % 4 == 0) true
	else false

// with when
isLeapYear = when {
	year % 400 == 0 -> true
	year % 100 == 0 -> false
	year % 4 == 0 -> true
	else -> false
}
```

In general, we will use `when` as soon as there are more than 2 or 3 cases in a `if … else if … else` chain.

## Loops

Only printing some text in some cases is a great start, but to create real programs we will need another component: loops. In general, a ‘loop’ is a construct that tells the computer to run the same code multiple times. There are many ways to do so, here we will focus on the two simplest forms.

### Loop a specific number of times

When we know how many times we want the loop to run, we can use a `for` loop.

```kotlin
for (i in 0..5) {
	// Here, anything you want to run multiple times
	println(i)
}
```

Here, `0..5` is the range that starts from 0 (inclusive) and ends at 5 (inclusive). The `i` variable is a temporary variable that is used to know which loop we currently are on: on the first loop, `i` is equal to 0, on the second loop it is equal to `1`, on the third it is equal to `2`, etc. You could name the variable any way you want; `i` is a popular choice for simple loops (when writing anything complicated, use a proper name, so the code is easier to read). `i` was chosen because it is the initial of “index”.

The output of the previous program is:

```text
0
1
2
3
4
5
```

Note that in computer science, we very often start counting at 0, much like buildings in the UK (ground floor, first floor, second floor…). Our range `0..5` therefore contains 6 numbers.

When we want to loop a certain number of times, we will use the range `0 until 5`: from 0 (inclusive) to 5 (exclusive):

```kotlin
for (i in 0 until 5) {
	println(i)
}
```

```text
0
1
2
3
4
```

We will explain in the next chapter one of the reason why we prefer starting at 0 than at 1, in the general case. You are still free to create a range from any other value to any other, if you'd like to.

Here are a few other examples of ranges that you can create:

| Range | Numbers |
|---|---|
| `0..3` | 0, 1, 2, 3 |
| `0 until 3` | 0, 1, 2 |
| `0..6 step 2` | 0, 2, 4, 6 |
| `0 until 6 step 2` | 0, 2, 4 |
| `3 downTo 0` | 3, 2, 1, 0 |
| `3 downTo 0 step 2` | 3, 1 |

For the special case where we want a very simple `0 until n` loop, the `repeat` function can help make the code easier to read:

```kotlin
// The simplest for loop
for (i in 0 until 10) {
	println(i)
}

// The repeat function makes this exact case shorter
repeat(10) {
	println(it)
}
```

Note that using the `repeat` function, the index is called `it` and not `i`, and that the `repeat` function is not highlighted in the same color as the `for` keyword. These are due to the differences between function and keyword, which we will detail in a later chapter.

The `repeat` function doesn't use ranges, so we won't use it for anything complicated. However, most of the time loops are simple enough that it fits.

### Loop an unknown number of times

Sometimes, however, we do not know in advance how many times we want to loop. For example, we want to know how many times a number is divisible by two, without using the modulo.

```kotlin
var number = 26 // or any other number
var numberOfTimesDivisible = 0

while (number > 1) {
	number = number / 2
	numberOfTimesDivisible = numberOfTimesDivisible + 1
}

println(numberOfTimesDivisible)
```

Here, there a quite a few concepts. Let's go through them in order.

So far, we only knew how to create _read-only variables_, with the `val` keyword. We could choose their value once, but we could never modify them in the future.

Kotlin also allows _mutable variables_, with the `var` keyword. They are identical in all points, except that we are allowed to change their value as many times as we want after they've been created. If they offer strictly more, why didn't we start with them? We didn't, because more power means more responsibility. We can modify the value on purpose, but it is now also possible to modify the value accidentally (which, in the real world, happens very often). In Kotlin, we should use `val` as much as possible, and only use `var` when we have to: it also makes the code easier to read, as it is clear to the reader what is supposed to change or not.

When we modify a `var`, we provide a new value with the `=` operator:

```kotlin
var a = 1
a = 2
a = 3
```

We never repeat the `var` keyword: the presence of `val` or `var` means that we create a _new variable_, not that we modify the previous one.

When we modify a `var`, we are not allowed to change the type:

```kotlin
var a: Int = 1
a: Long // forbidden
```

We can now read the contents of the loop above:

```kotlin
// Divide the 'number' by 2, then replace the old value by the new one
number = number / 2
```

Using temporary variables, the above is equivalent to:

```kotlin
// Divide the 'number' by 2
val temporary = number / 2

// Replace the old value by the new one
number = temporary
```

It is important to understand that the right side of the `=` operator is calculated first, and its result then replaces the old value of the left side. This is why `=` is called the "assignment operator", it is very different from the `=` sign in mathematics.

Our code looked like this:

```kotlin
number = number / 2
numberOfTimesDivisible = numberOfTimesDivisible + 1
```

As always with the language, there are shortcuts for simple modifications of a value:

| Shorthand | Long equivalent |
| --------- | --------------- |
| `a += b`  | `a = a + b` |
| `a -= b`  | `a = a - b` |
| `a *= b`  | `a = a * b` |
| `a /= b`  | `a = a / b` |

This list is not exhaustive, as most operators have their shorthand formed with this pattern.

For the special case of the value 1, there are two further shortcuts:

| Shorthand | Long equivalent |
| --------- | --------------- |
| `a++`  | `a = a + 1` |
| `a--`  | `a = a - 1` |

All of these shorthands are common to almost all programming languages. These are where `C++` takes its name (it was supposed to be the improvement over the `C` language).

Using these shorthands, we can simplify our previous code:

```kotlin
var number = 26 // or any other number
var numberOfTimesDivisible = 0

while (number > 1) {
	number /= 2
	numberOfTimesDivisible++
}

println(numberOfTimesDivisible)
```

Notice how similar `while` is with `if`: their syntax is identical, because their purpose is almost the same:

- `if`: if this condition is `true`, run this code
- `while`: if this condition is `true`, run this code _again_

However, there is no equivalent of `else` or `else if` for `while`.

The loop will run as long as the provided condition is `true`:

```kotlin
var i = 0
while (i < 5) {
	println(i)
	i++
}
```

```text
0
1
2
3
4
```

Or, in our example:

```kotlin
var number = 26
var numberOfTimesDivisible = 0

while (number > 1) {
	println(number)
	number /= 2
	numberOfTimesDivisible++
}

println("Done")
println(numberOfTimesDivisible)
```

```text
26
13
6
3
Done
4
```

In the next chapter, we will discuss [lists](4-lists.md).
