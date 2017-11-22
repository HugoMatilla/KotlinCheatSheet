# 2 Kotlin Basics
## 2.1 Basic Elements
### 2.1.1 Hello World
```js
fun main(args: Array<String>){
	println("Hello World")
}
```

* Functions can be declared with no class.
* Arrays are just classes
* No Semicolon

### 2.1.2 Functions

```js
fun max(a: Int, b: Int): Int {
	return if (a > b) a else b
}
```
* Return type after ) separated by :
* `if` is an expression not a statement

#### Statements vs Expressions
* Expressions: has a value, which can be used as part of another expression. In Kotlin, most control structures, except for the loops (for, do, and do/
while) are expressions.
* Statement is always a top-level element in its enclosing block and doesn’t have its own value. Assignments are expressions in Java and become statements in
Kotlin

#### Expression Bodies
If the function body is a single expression you can use it as the entire body of the function

```js
fun max(a: Int, b: Int): Int = if (a > b) a else b
```

* Return body: if the function has braces
* Expression  body: if returns an expression

#### Type inference
Omitting the return type, and let the compiler _infer_ the type. (Only in Expression Bodies)

```js
	fun max(a: Int, b: Int) = if (a > b) a else b
```

### 2.1.3  Variables
If a variable has an initializer

```js
val question = "The Ultimate Question of Life, the Universe, and Everything"
val answer = 42
```
 You can add the type, but is not needed
 ```js
 val answer : Int = 42
```
If there is no initializer, you need to add the type

```js
 val answer : Int
 answer = 42
```

#### Mutable and Immutable variables
* `val`: (value) Immutable. (final in Java)
* `var`: (variable) Mutable

Use `val` unless necessary

Initialize it with different values depending on some condition, if the compiler can ensure that only one of the initialization statements will
be executed.

```js
if (canPerformOperation())
	var message = "Success"
else 
	var message = "Error"
```

Objects that a `val` point can be changed

```js
val languages = arrayListOf("Java") // languages is an immutable reference
languages.add("Kotlin")
```

`var` can change the value but not the type

```js
var answer = 42
answer = "no answer" // Compile Error
```

### 2.1.4 Easier string formatting: string templates
```js
var name  = "Peter"
println("Hello, $name!")
println("Hello, ${args[0]}!")
```

## 2.2 Classes and properties
```
class Person(val name: String)
```

* This kind of classes are called _value objects_
* `public` is not needed because all classes are public by default

### 2.2.1 Properties
* _properties_ in Java are the combination of _fields_ and _accessors_ 
* In Kotlin _properties_ are declared with _var_ and _val_ depending on its mutability

```
class Person(
	val name: String,
	var isMarried: Boolean
)
```
* _accessors_ are built in automatically
* Custom _accessors_ can be built
* _properties_ are referenced directly `person.name` and if mutable `person.name = "John"`

### 2.2.2 Custom _accessors_
```php
class Rectangle(val height: Int, val width: Int) {
	val isSquare: Boolean
		get() = return height == width
}
```
* The property isSquare doesn’t need a field to store its value.
* It only has a custom getter with the implementation provided.
* The value is computed every time the property is accessed.

## 2.3 Representing and handling choices: enums and “when”
### 2.3.1 Declaring enum classes
```php
enum class Color {
	RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET
}
```
* `enum` is a soft keyword (i.e. you can use enum in your variable names)
* `class` is a keyword

You can declare properties and methods on enum classes

```php
enum class Color(val r: Int, val g: Int, val b: Int) {
	RED(255, 0, 0), 
	ORANGE(255, 165, 0),
	YELLOW(255, 255, 0),
	GREEN(0, 255, 0),
	BLUE(0, 0, 255),
	INDIGO(75, 0, 130),
	VIOLET(238, 130, 238);
	fun rgb() = (r * 256 + g) * 256 + b
}
```
* _enum constants_: `val r : Int`
* _enum constants_ need to be declared for each constant
* semicolon is needed to separate _enum constants_ list from method definitions

### 2.3.2 Using “when” to deal with enum classes
```js
fun getWarmth(color: Color) =
	when(color) {
		Color.RED, Color.ORANGE, Color.YELLOW -> "warm"
		Color.GREEN -> "neutral"
		Color.BLUE, Color.INDIGO, Color.VIOLET -> "cold"
	}
```

Import enum constants to access without qualifier

```js
import ch02.colors.Color //Imports the Color class declared
import ch02.colors.Color.* //Explicitly imports enum constants to us them by names
fun getWarmth(color: Color) = when(color) {
	RED, ORANGE, YELLOW -> "warm"
	GREEN -> "neutral"
	BLUE, INDIGO, VIOLET -> "cold"
}
```

### 2.3.3 Using “when” with arbitrary objects
Being able to use any expression as a when branch condition lets you write concise and beautiful code in many cases.

```js
fun mix(c1: Color, c2: Color) =
	when (setOf(c1, c2)) {
		setOf(RED, YELLOW) -> ORANGE
		setOf(YELLOW, BLUE) -> GREEN
		setOf(BLUE, VIOLET) -> INDIGO
		else -> throw Exception("Dirty color")
	}
```
### 2.3.4 Using `when` without an argument
If no argument is supplied for the when expression, the branch condition is any Boolean expression.

```js
fun mixOptimized(c1: Color, c2: Color) =
when {
	(c1 == RED && c2 == YELLOW) ||
	(c1 == YELLOW && c2 == RED) ->
	ORANGE
	(c1 == YELLOW && c2 == BLUE) ||
	(c1 == BLUE && c2 == YELLOW) ->
	GREEN
	(c1 == BLUE && c2 == VIOLET) ||
	(c1 == VIOLET && c2 == BLUE) ->
	INDIGO
	else -> throw Exception("Dirty color")
}
```

### 2.3.5 Smart casts: combining type checks and casts
* `is` similar to Java's `instanceOf`
* *smart cast*: `is` do the cast automatically so you don't need to do it manually as in Java. 

```js
if (e is Sum) {
	return eval(e.right) + eval(e.left)
}
```

### 3.3.6 Refactoring: replacing “if” with “when”
`when` branches check the argument type and apply smart casts

```js
fun eval(e: Expr): Int =
	when (e) {
		is Num -> e.value
		is Sum -> eval(e.right) + eval(e.left)
		else -> throw IllegalArgumentException("Unknown expression")
}
```

### 2.3.7 Blocks as branches of “if” and “when”
* `if` and `when` can have blocks as branches. 
* The last expression in the block is the result.

```js
	...
	is Num -> {
		println("num: ${e.value}")
		e.value // returned value
	}...
}
```

This does not apply for regular functions. They must have a `return`

## 2.4 Iterating over things: “while” and “for” loops
### 2.4.1 The “while” loop
The body is executed while the condition is true.

```js
while (condition) {
	...
}
```

The body is executed for the first time unconditionally. After that, it’s executed while the condition is true.

```js
do {
	...
} while (condition)
```

### 2.4.2 Iterating over numbers: ranges and progressions
* _range_: essentially  an interval between two values. `..` Last value is always in the range. (closed/inclusive range)

```js
	val oneToTen = 1..10
```

* You can change the step and the order 

```js
	for (i in 100 downTo 1 step 2)
```

* Iteration in a range

```js
	for (x in 0 until size) //  for (x in 0..size-1)
```

### 2.4.3 Iterating over maps
The .. syntax to create a range works not only for numbers, but also for characters

```js
val binaryReps = TreeMap<Char, String>() // in TreeMaps, keys are sorted
for (c in 'A'..'F') {
	val binary = Integer.toBinaryString(c.toInt())
	binaryReps[c] = binary
}
for ((letter, binary) in binaryReps) { // Iterates over a map, assigning the map key and value to two variables
	println("$letter = $binary")
}
```

You can get the index of a collection

```js
val list = arrayListOf("10", "11", "1001")
for ((index, element) in list.withIndex()) {
	println("$index: $element")
}
```

### 2.4.4 Using “in” to check collection and range membership
You can use the `in` operator to check whether a value is in a range, or its opposite, `!in`

```js
	fun isLetter(c: Char) = c in 'a'..'z' || c in 'A'..'Z'
	fun isNotDigit(c: Char) = c !in '0'..'9'
```
Works also on _when expressions_

```js
fun recognize(c: Char) = when (c) {
	in '0'..'9' -> "It's a digit!"
	in 'a'..'z', in 'A'..'Z' -> "It's a letter!"
	else -> "I don't know…"
}
```
You can use the `in` in any class that implements the `java.lang.Comparable` interface

## 2.5 Exceptions in Kotlin
* You don’t have to use the new keyword to create an instance of the exception
* `throw` construct is an expression

```js
val percentage =
	if (number in 0..100)
		number
	else
		throw IllegalArgumentException("A percentage value must be between 0 and 100: $number")

```
### 2.5.1 “try”, “catch”, and “finally”
* Kotlin doesn’t differentiate between checked and unchecked exceptions
* You don’t have to explicitly specify exceptions that can be thrown

### 2.5.2 “try” as an expression
* If the execution of a try code block behaves normally, the last expression in the block is the result. 
* If an exception is caught, the last expression in a corresponding catch block is the result.

```js
fun readNumber(reader: BufferedReader) {
	val number = try {
		Integer.parseInt(reader.readLine())
	} catch (e: NumberFormatException) {
		null
	}
	println(number)
}
```
## 2.6 Summary
* The `fun` keyword is used to declare a function. The `val` and `var` keywords declare read-only and mutable variables, respectively.
* String templates help you avoid noisy string concatenation. Prefix a variable name with `$` or surround an expression with `${ }` to have its value injected into the string.
* Value-object classes are expressed in a concise way in Kotlin.
* The familiar `if` is now an expression with a return value.
* The `when` expression is analogous to switch in Java but is more powerful.
* You don’t have to cast a variable explicitly after checking that it has a certain type: the compiler casts it for you automatically using a **smart cast**.
* The `for`, `while`, and `do-while` loops are similar to their counterparts in Java, but the `for loop` is now more convenient, especially when you need to iterate over a map or a collection with an index.
* The concise syntax `1..5` creates a range. Ranges and progressions allow Kotlin to use a uniform syntax and set of abstractions in for loops and also work with the `in` and `!in` operators that check whether a value belongs to a range.
* Exception handling in Kotlin is very similar to that in Java, except that **Kotlin doesn’t require you to declare the exceptions that can be thrown** by a function.

# 3 Defining and calling functions
## 3.1 Creating collections in Kotlin
Kotlin uses the standard Java collection classes

```js
val set = hashSetOf(1, 7, 53) // java.util.HashSet
val list = arrayListOf(1, 7, 53) // java.util.ArrayList
val map = hashMapOf(1 to "one", 7 to "seven", 53 to "fifty-three") // java.util.HashMap
```
You can do more with Kotlin than with Java. 
ie

```js
val strings = listOf("first", "second", "fourteenth")
println(strings.last())// fourteenth

val numbers = setOf(1, 14, 2)
println(numbers.max()) // 14
```

## 3.2 Making functions easier to call
Sample call to joinToString

```js
fun <T> joinToString(
		collection: Collection<T>,
		separator: String,
		prefix: String,
		postfix: String
	): String {

	val result = StringBuilder(prefix)
	for ((index, element) in collection.withIndex()) {
		if (index > 0) result.append(separator)
			result.append(element)
		}
	result.append(postfix)
	return result.toString()
}
```
Call

```js
val list = listOf(1, 2, 3)
println(list) // [1, 2, 3]

val list = listOf(1, 2, 3)
println(joinToString(list, "; ", "(", ")")) // (1; 2; 3)
```

### 3.2.1 Named Arguments
To make easier the call `joinToString(collection, " ", " ", ".")` in Kotlin we can add the name of the parameter to the call.

```js
joinToString(collection, separator = " ", prefix = " ", postfix = ".")
```
* If you specify the name of an argument in a call, you should also specify the names for all the arguments after that
* You can’t use named arguments when calling methods written in Java

### 3.2.2 DEafult parameters values
To reduce the number of overloaded methods, we can use default parameters in Kotlin.

```js
fun <T> joinToString(
	collection: Collection<T>,
	separator: String = ", ",
	prefix: String = "",
	postfix: String = ""
): String
```
```js
joinToString(list, ", ", "", "") // 1, 2, 3
joinToString(list) // 1, 2, 3
joinToString(list, "; ") // 1; 2; 3
```

* When using the regular call syntax, you can omit only trailing arguments. 
* When using named arguments, you can omit some arguments from the middle of the list

```js
joinToString(list, suffix = ";", prefix = "# ") // 1, 2, 3
```

* _To use default values from JAVA use `@JVMOverloads`_

### 3.2.3 Getting rid of static utility classes: **top-level** functions and properties
* In Kotlin  you can place functions at the *top level* of a source file, outside of any class.
* They are still members of the package declared at the top of the file.
* You still need to import them if you want to call them from other packages.

```js
package strings
fun joinToString(...): String { ... }
```
* _To use top-level functions from JAVA call it as `NameOfTheFileContainingTheFunction.functionName()`_

#### top-level properties
```js
var opCount = 0
fun performOperation() {opCount++}
fun reportOperationCount() {println("Operation performed $opCount times")}
```
They are stored in static files.

* `const` makes them immutable. In JAVA: `static final`

## 3.3 Adding methods to other people’s classes: **extension** functions and properties
Extension function: a function that can be called as a member of a class but is defined outside of it.





