# 2 Kotlin Basics
## 2.1 Basic Elements
### 2.1.1 Hello World
```kotlin
fun main(args: Array<String>){
	println("Hello World")
}
```

* Functions can be declared with no class.
* Arrays are just classes
* No Semicolon

### 2.1.2 Functions

```kotlin
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

```kotlin
fun max(a: Int, b: Int): Int = if (a > b) a else b
```

* Return body: if the function has braces
* Expression  body: if returns an expression

#### Type inference
Omitting the return type, and let the compiler _infer_ the type. (Only in Expression Bodies)

```kotlin
	fun max(a: Int, b: Int) = if (a > b) a else b
```

### 2.1.3  Variables
If a variable has an initializer

```kotlin
val question = "The Ultimate Question of Life, the Universe, and Everything"
val answer = 42
```
 You can add the type, but is not needed
 ```kotlin
 val answer : Int = 42
```
If there is no initializer, you need to add the type

```kotlin
 val answer : Int
 answer = 42
```

#### Mutable and Immutable variables
* `val`: (value) Immutable. (final in Java)
* `var`: (variable) Mutable

Use `val` unless necessary

Initialize it with different values depending on some condition, if the compiler can ensure that only one of the initialization statements will
be executed.

```kotlin
val message: String
if (canPerformOperation())
  message = "Success"
else 
  message = "Error"
```

Objects that a `val` point can be changed

```kotlin
val languages = arrayListOf("Java") // languages is an immutable reference
languages.add("Kotlin")
```

`var` can change the value but not the type

```kotlin
var answer = 42
answer = "no answer" // Compile Error
```

### 2.1.4 Easier string formatting: string templates
```kotlin
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
```kotlin
fun getWarmth(color: Color) =
	when(color) {
		Color.RED, Color.ORANGE, Color.YELLOW -> "warm"
		Color.GREEN -> "neutral"
		Color.BLUE, Color.INDIGO, Color.VIOLET -> "cold"
	}
```

Import enum constants to access without qualifier

```kotlin
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

```kotlin
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

```kotlin
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

```kotlin
if (e is Sum) {
	return eval(e.right) + eval(e.left)
}
```

### 3.3.6 Refactoring: replacing “if” with “when”
`when` branches check the argument type and apply smart casts

```kotlin
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

```kotlin
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

```kotlin
while (condition) {
	...
}
```

The body is executed for the first time unconditionally. After that, it’s executed while the condition is true.

```kotlin
do {
	...
} while (condition)
```

### 2.4.2 Iterating over numbers: ranges and progressions
* _range_: essentially  an interval between two values. `..` Last value is always in the range. (closed/inclusive range)

```kotlin
	val oneToTen = 1..10
```

* You can change the step and the order 

```kotlin
	for (i in 100 downTo 1 step 2)
```

* Iteration in a range

```kotlin
	for (x in 0 until size) //  for (x in 0..size-1)
```

### 2.4.3 Iterating over maps
The .. syntax to create a range works not only for numbers, but also for characters

```kotlin
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

```kotlin
val list = arrayListOf("10", "11", "1001")
for ((index, element) in list.withIndex()) {
	println("$index: $element")
}
```

### 2.4.4 Using “in” to check collection and range membership
You can use the `in` operator to check whether a value is in a range, or its opposite, `!in`

```kotlin
	fun isLetter(c: Char) = c in 'a'..'z' || c in 'A'..'Z'
	fun isNotDigit(c: Char) = c !in '0'..'9'
```
Works also on _when expressions_

```kotlin
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

```kotlin
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

```kotlin
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

```kotlin
val set = hashSetOf(1, 7, 53) // java.util.HashSet
val list = arrayListOf(1, 7, 53) // java.util.ArrayList
val map = hashMapOf(1 to "one", 7 to "seven", 53 to "fifty-three") // java.util.HashMap
```
You can do more with Kotlin than with Java. 
ie

```kotlin
val strings = listOf("first", "second", "fourteenth")
println(strings.last())// fourteenth

val numbers = setOf(1, 14, 2)
println(numbers.max()) // 14
```

## 3.2 Making functions easier to call
Sample call to joinToString

```kotlin
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

```kotlin
val list = listOf(1, 2, 3)
println(list) // [1, 2, 3]

val list = listOf(1, 2, 3)
println(joinToString(list, "; ", "(", ")")) // (1; 2; 3)
```

### 3.2.1 Named Arguments
To make easier the call `joinToString(collection, " ", " ", ".")` in Kotlin we can add the name of the parameter to the call.

```kotlin
joinToString(collection, separator = " ", prefix = " ", postfix = ".")
```
* If you specify the name of an argument in a call, you should also specify the names for all the arguments after that
* You can’t use named arguments when calling methods written in Java

### 3.2.2 Default parameters values
To reduce the number of overloaded methods, we can use default parameters in Kotlin.

```kotlin
fun <T> joinToString(
	collection: Collection<T>,
	separator: String = ", ",
	prefix: String = "",
	postfix: String = ""
): String
```
```kotlin
joinToString(list, ", ", "", "") // 1, 2, 3
joinToString(list) // 1, 2, 3
joinToString(list, "; ") // 1; 2; 3
```

* When using the regular call syntax, you can omit only trailing arguments. 
* When using named arguments, you can omit some arguments from the middle of the list

```kotlin
joinToString(list, suffix = ";", prefix = "# ") // 1, 2, 3
```

* _To use default values from JAVA use `@JVMOverloads`_

### 3.2.3 Getting rid of static utility classes: **top-level** functions and properties
* In Kotlin  you can place functions at the *top level* of a source file, outside of any class.
* They are still members of the package declared at the top of the file.
* You still need to import them if you want to call them from other packages.

```kotlin
package strings
fun joinToString(...): String { ... }
```
* _To use top-level functions from JAVA call it as `NameOfTheFileContainingTheFunction.functionName()`_

#### top-level properties
```kotlin
var opCount = 0
fun performOperation() {opCount++}
fun reportOperationCount() {println("Operation performed $opCount times")}
```
They are stored in static files.

* `const` makes them immutable. In JAVA: `static final`

## 3.3 Adding methods to other people’s classes: **extension** functions and properties
Extension function: a function that can be called as a member of a class but is defined outside of it.

* `String` is the _receiver type_
* `this` is the _receiver object_ ("Kotlin" in the example)
* `this` can be omitted

```kotlin
fun String.lastChar(): Char = this.get(this.length - 1)

println("Kotlin".lastChar()) // n
```
* You can not break encapsulation. From a extension function you don't have access to private or protected members.

### 3.3.1 Imports and extension functions
* You have to import them
* You can import individual functions

```kotlin
import strings.lastChar or //strings.*
"Kotlin".lastChar()
```
You can change the name of the function

```kotlin
import strings.lastChar as last
"Kotlin".last()
```

### 3.3.2 Calling extension function from Java
* An extension function is a static method that accepts the receiver object as its first argument.
* As with other top-level functions, the name of the Java class containing the method is determined from the name of the file where the function is declared.
* Is declared as a top-level function, so it’s compiled to a static method.

```kotlin
/* Java */
char c = StringUtilKt.lastChar("Java");
```

### 3.3.3 Utility functions as extensions
```kotlin
fun <T> Collection<T>.joinToString(
	separator: String = ", ",
	prefix: String = "",
	postfix: String = ""
): String {
	val result = StringBuilder(prefix)
	for ((index, element) in this.withIndex())
		if (index > 0) result.append(separator)
		result.append(element)
	}
	result.append(postfix)
	return result.toString()
}

val list = arrayListOf(1, 2, 3)
list.joinToString(" ")// 1 2 3
```

If we want to use it only in Strings.

```kotlin
fun Collection<String>.join(
	separator: String = ", ",
	prefix: String = "",
	postfix: String = ""
) = joinToString(separator, prefix, postfix)

listOf("one", "two", "eight").join(" ")// one two eight
```

### 3.3.4 No overriding for extension functions
* You can’t override an extension function
* The function that’s called depends on the declared static type of the variable, not on the runtime type of the value stored in that variable.

```kotlin
fun View.showOff() = println("I'm a view!")
fun Button.showOff() = println("I'm a button!")

val view: View = Button()
view.showOff() // I'm a view!
```

* If the class has a member function with the same signature as an extension function, the member function always takes precedence.

### Extension properties
* Extend classes with APIs that can be accessed using the property syntax, be accessed using the property syntax,
* They’re called properties, they can’t have any state. (there’s no proper place to store it)

```kotlin
val String.lastChar: Char
	get() = get(length - 1)
```
* The getter must always be defined. There’s no backing field and therefore no default getter implementation.
* Initializers aren’t allowed for the same reason.

Adding a setter they can be mutable 

```kotlin
var StringBuilder.lastChar: Char
	get() = get(length - 1)
	set(value: Char) {
		this.setCharAt(length - 1, value)
}
println("Kotlin".lastChar) // n
val sb = StringBuilder("Kotlin?")
sb.lastChar = '!'
println(sb)// Kotlin!
```

* From Java, you should invoke its getter explicitly: `StringUtilKt.getLastChar("Java")`

## 3.4 Working with collections: varargs, infix calls, and library support
### 3.4.2 Varargs: functions that accept an arbitrary number of arguments
* Used in `listOf` for example

```kotlin
val list = listOf(2, 3, 5, 7, 11)
```
* Uses `vararg` modifier  on the parameter insted of the `...` from Java

```kotlin 
fun listOf<T>(vararg values: T): List<T> { ... }
```

* Arrays in Java are passed as is, but in Kotlin they have to be unpacked. You can use the _spread operator_: `*`.

```kotlin
fun main(args: Array<String>) {
	val list = listOf("args: ", *args)
	println(list)
}
```

### 3.4.3 Working with pairs: infix calls and destructuring declarations
In an infix call, the method name is placed immediately between the target object name and the parameter

```kotlin
1.to("one") === 1 to "one"
```

* Use `infix` modifier.

```kotlin
infix fun Any.to(other: Any) = Pair(this, other)
```

#### _Destructuring declaration_
```kotlin
val (number, name) = 1 to "one"
```
It is used in loops

```kotlin
for ((index, element) in collection.withIndex()) {
	println("$index: $element")
}
```

## 3.5 Working with strings and regular expressions
Kotlin Strings are the same as Java Strings
### 3.5.1 Splitting strings
* To avoid the confusion in Java with the split, dots and regular expressions Kotlin have overloaded extension. One that take a string and another one that take a Regex.
* You can also give more than one separator to the split method. `"12.345-6.A".split(".", "-")`

### 3.5.2 Regular expressions and triple-quoted strings
Using string extension functions is easy to parse the path, file name and extension of a file.

```kotlin
fun parsePath(path: String) {
	val directory = path.substringBeforeLast("/")
	val fullName = path.substringAfterLast("/")
	val fileName = fullName.substringBeforeLast(".")
	val extension = fullName.substringAfterLast(".")
	println("Dir: $directory, name: $fileName, ext: $extension")
}
parsePath("/Users/yole/kotlin-book/chapter.adoc")
// Dir: /Users/yole/kotlin-book, name: chapter, ext: adoc
```

#### _Triple-quoted string_ and Regex
In _triple-quoted string_, you don’t need to escape any characters, including the backslash, so you can encode the dot symbol with `\.` rather than `\\.`

```kotlin
fun parsePath(path: String) {
	val regex = """(.+)/(.+)\.(.+)""".toRegex()
	val matchResult = regex.matchEntire(path)
	if (matchResult != null) {
		val (directory, filename, extension) = matchResult.destructured
		println("Dir: $directory, name: $filename, ext: $extension")
	}
}
```
### 3.5.3 Multiline triple-quoted strings
* _triple-quoted strings_ allows embed in your programs text containing line breaks
* You can trim the indentation using `trimMargin` to have a better representation.

```kotlin
val kotlinLogo = """| //
				   .|//
				   .|/ \"""
>>> println(kotlinLogo.trimMargin("."))
| //
|//
|/ \
```
### 3.6 Making your code tidy: _local functions_ and extensions
* Making DRY principle less boilerplate and cleaned
* Keep the structure of the class. (Functions are inside the methods where are called.)

From

```kotlin
class User(val id: Int, val name: String, val address: String)

fun saveUser(user: User) {
	if (user.name.isEmpty()) {// <- Repeated
		throw IllegalArgumentException("Can't save user ${user.id}: empty Name") // <- Repeated
	}
	if (user.address.isEmpty()) { // <- Repeated
		throw IllegalArgumentException("Can't save user ${user.id}: empty Address") // <- Repeated
	}
// Save user to the database
}
```

* Local functions: You can nest the functions you’ve extracted in the containing function
* Local functions have access to all parameters and variables of the enclosing function
* Local functions keep separated functions that are only relevant to a parent function, keeping the class clean and clear.
* Local functions can access its public members without extra qualification
* Try not to have more than one level of nesting

```kotlin
class User(val id: Int, val name: String, val address: String)

fun saveUser(user: User) {
  fun validate(value: String, fieldName: String) {
    if (value.isEmpty()) {
      throw IllegalArgumentException( "Can't save user ${user.id}: empty $fieldName")
    }
  }
  validate(user.name, "Name")
  validate(user.address, "Address")
  // Save user to the database
}
```

To a extension function

```kotlin
class User(val id: Int, val name: String, val address: String)

	fun User.validateBeforeSave() {
		fun validate(value: String, fieldName: String) {
			if (value.isEmpty()) {
				throw IllegalArgumentException( "Cant save user ${user.id}: empty $fieldName")
			}
		}
		validate(name, "Name")
		validate(address, "Address")
	}
	
	fun saveUser(user: User) {
		user.validateBeforeSave()
		// Save user to the database
	}
}
```
## 3.7 Summary
* Kotlin doesn’t define its own collection classes and instead enhances the Java collection classes with a richer API.
* Defining default values for function parameters greatly reduces the need to define overloaded functions, and the named-argument syntax makes calls to functions with many parameters much more readable.
* Functions and properties can be declared directly in a file, not just as members of a class, allowing for a more flexible code structure.
* Extension functions and properties let you extend the API of any class, including classes defined in external libraries, without modifying its source code and with no runtime overhead.
* Infix calls provide a clean syntax for calling operator-like methods with a single argument.
* Kotlin provides a large number of convenient string-handling functions for both regular expressions and plain strings.
* Triple-quoted strings provide a clean way to write expressions that would require a lot of noisy escaping and string concatenation in Java.
* Local functions help you structure your code more cleanly and eliminate duplication
