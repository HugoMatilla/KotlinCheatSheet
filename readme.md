# Hello Kotlin
## Hello World
We declare a package-level function main which returns Unit and takes
an Array of strings as a parameter. Note that semicolons are optional.

``` kotlin
>	fun main(args: Array<String>) {
    	println("Hello, world!")
	}
```


## Reading a name from the command line
Line 13 demonstrates string templates and array access.
See this pages for details:
http://kotlinlang.org/docs/reference/basic-types.html#strings
http://kotlinlang.org/docs/reference/basic-types.html#arrays


``` kotlin
>	fun main(args: Array<String>) {
	    if (args.size == 0) {
	        println("Please provide a name as a command-line argument")
	        return
	    }
	    println("Hello, ${args[0]}!")
	}
```

## Reading many names from the command line
Line 2 demonstrates the for-loop, that would have been called "enhanced"
if there were any other for-loop in Kotlin.
See http://kotlinlang.org/docs/reference/basic-syntax.html#using-a-for-loop


``` kotlin
>	fun main(args: Array<String>) {
	    for (name in args)
	        println("Hello, $name!")
	    
	    for (i in args.indices)
	    	print(args[i])
	}
```

## _When_
In this example, `val` denotes a declaration of a read-only local variable,
that is assigned an pattern matching expression.
See http://kotlinlang.org/docs/reference/control-flow.html#when-expression


``` kotlin
>	fun main(args: Array<String>) {
	    val language = if (args.size == 0) "EN" else args[0]
	    println(when (language) {
	        "EN" -> "Hello!"
	        "FR" -> "Salut!"
	        "IT" -> "Ciao!"
	        else -> "Sorry, I can't greet you in $language yet"
	    })
	}
``` 

## OOP Simple Class
Here we have a class with a primary constructor and a member function.
Note that there's no `new` keyword used to create an object.
See http://kotlinlang.org/docs/reference/classes.html#classes


``` kotlin
>	class Greeter(val name: String) {
		fun greet() {
		    println("Hello, ${name}");
		}
	}

	fun main(args: Array<String>) {
	    Greeter(args[0]).greet()
	}
```
#Basics 
## _if_
`if` is an expression, i.e. it returns a value.
Therefore there is no ternary operator (condition ? then : else),
because ordinary `if` works fine in this role.
See http://kotlinlang.org/docs/reference/control-flow.html#if-expression

``` kotlin
>	fun main(args: Array<String>) {
    	println(max(Integer.parseInt(args[0]), Integer.parseInt(args[1])))
	}

	fun max(a: Int, b: Int) = if (a > b) a else b

``` 

## Nullables
A reference must be explicitly marked as nullable to be able hold a null.
See http://kotlinlang.org/docs/reference/null-safety.html#null-safety

``` kotlin
>	package multiplier

	// Return null if str does not hold a number
	fun parseInt(str: String): Int? {
	    try {
	        return Integer.parseInt(str)
	    } catch (e: NumberFormatException) {
	        println("One of the arguments isn't Int")
	    }
	    return null
	}

	fun main(args: Array<String>) {
	    if (args.size < 2) {
	        println("No number supplied");
	    } else {
	        val x = parseInt(args[0])
	        val y = parseInt(args[1])

	        // We cannot say 'xy' now because they may hold nulls
	        if (x != null && y != null) {
	            print(xy) // Now we can
	        } else {
	            println("One of the arguments is null")
	    }
	    }
	}
``` 
## _is_
The `is` operator checks if an expression is an instance of a type and more.
If we is-checked an immutable local variable or property, there's no need
to cast it explicitly to the is-checked type.
See this pages for details:
http://kotlinlang.org/docs/reference/classes.html#classes-and-inheritance
http://kotlinlang.org/docs/reference/typecasts.html#smart-casts

``` kotlin
>	fun main(args: Array<String>) {
		println(getStringLength("aaa"))
    	println(getStringLength(1))
	}

	fun getStringLength(obj: Any): Int? {
	    if (obj is String)
	        return obj.length // no cast to String is needed
	    return null
	}
``` 
## _while_ and _do..while_
See http://kotlinlang.org/docs/reference/control-flow.html#while-loops

``` kotlin
>	fun main(args: Array<String>) {
    var i = 0
    while (i < args.size)
        println(args[i++])
}
``` 

## Ranges
Check if a number lies within a range.
Check if a number is out of range.
Check if a collection contains an object.
See http://kotlinlang.org/docs/reference/ranges.html#ranges


``` kotlin
>	fun main(args: Array<String>) {
    val x = Integer.parseInt(args[0])
    //Check if a number lies within a range:
    val y = 10
    if (x in 1..y - 1)
        println("OK")

    //Iterate over a range:
    for (a in 1..5)
        print("${a} ")

    //Check if a number is out of range:
    println()
    val array = arrayListOf<String>()
    array.add("aaa")
    array.add("bbb")
    array.add("ccc")

    if (x !in 0..array.size - 1)
        println("Out: array has only ${array.size} elements. x = ${x}")

    //Check if a collection contains an object:
    if ("aaa" in array) // collection.contains(obj) is called
        println("Yes: array contains aaa")

    if ("ddd" in array) // collection.contains(obj) is called
        println("Yes: array contains ddd")
    else
        println("No: array doesn't contains ddd")
}
``` 

## Control flow _when_
See http://kotlinlang.org/docs/reference/control-flow.html#when-expression


``` kotlin
>	fun main(args: Array<String>) {
    cases("Hello")
    cases(1)
    cases(System.currentTimeMillis())
    cases(MyClass())
    cases("hello")
}

	fun cases(obj: Any) {
	    when (obj) {
	        1 -> println("One")
	        "Hello" -> println("Greeting")
	        is Long -> println("Long")
	        !is String -> println("Not a string")
	        else -> println("Unknown")
	    }
	}

	class MyClass() {
	}
``` 

# Multi-declarations and Data classes
## Multi-declarations

This example introduces a concept that we call mutli-declarations.
It creates multiple variable at once. Anything can be on the right-hand
side of a mutli-declaration, as long as the required number of component
functions can be called on it.
See http://kotlinlang.org/docs/reference/multi-declarations.html#multi-declarations
 
``` kotlin
>	fun main(args: Array<String>) {
	    val pair = Pair(1, "one")

	    val (num, name) = pair

	    println("num = $num, name = $name")
	}

	class Pair<K, V>(val first: K, val second: V) {
	    operator fun component1(): K {
	        return first
	    }

	    operator fun component2(): V {
	        return second
	    }
	}
``` 
## Data classes
Data class gets component functions, one for each property declared
in the primary constructor, generated automatically, same for all the
other goodies common for data _toString()_, _equals()_, _hashCode()_ and _copy()_.
See http://kotlinlang.org/docs/reference/data-classes.html#data-classes
 
``` kotlin
>	data class User(val name: String, val id: Int)

	fun getUser(): User {
	    return User("Alex", 1)
	}

	fun main(args: Array<String>) {
	    val user = getUser()
	    println("name = ${user.name}, id = ${user.id}")

	    // or

	    val (name, id) = getUser()
	    println("name = $name, id = $id")

	    // or

	    println("name = ${getUser().component1()}, id = ${getUser().component2()}")
	}
``` 


## Data Maps

 Kotlin Standart Library provide component functions for Map.Entry
 
``` kotlin
>	fun main(args: Array<String>) {
	    val map = hashMapOf<String, Int>()
	    map.put("one", 1)
	    map.put("two", 2)

	    for ((key, value) in map) {
	        println("key = $key, value = $value")
	    }
	}
``` 

## Autogenerated Functions

Data class gets next functions, generated automatically:
_component functions_, _toString()_, _equals()_, _hashCode()_ and _copy()_.
See http://kotlinlang.org/docs/reference/data-classes.html#data-classes
 
``` kotlin
>	data class User(val name: String, val id: Int)

	fun main(args: Array<String>) {
	    val user = User("Alex", 1)
	    println(user) // toString()

	    val secondUser = User("Alex", 1)
	    val thirdUser = User("Max", 2)

	    println("user == secondUser: ${user == secondUser}")
	    println("user == thirdUser: ${user == thirdUser}")

	    // copy() function
	    println(user.copy())
	    println(user.copy("Max"))
	    println(user.copy(id = 2))
	    println(user.copy("Max", 2))
	}
``` 

#Delegated properties
## Custom Delegate

There's some new syntax: you can say `val 'property name': 'Type' by 'expression'`.
The expression after by is the delegate, because get() and set() methods
corresponding to the property will be delegated to it.
Property delegates don't have to implement any interface, but they have
to provide methods named get() and set() to be called.
 
``` kotlin
>	import kotlin.reflect.KProperty

	class Example {
	    var p: String by Delegate()

	    override fun toString() = "Example Class"
	}

	class Delegate() {
	): String {
	        return "$thisRef, thank you for delegating '${prop.name}' to me!"
	    }

	, value: String) {
	        println("$value has been assigned to ${prop.name} in $thisRef")
	    }
	}

	fun main(args: Array<String>) {
	    val e = Example()
	    println(e.p)
	    e.p = "NEW"
	}
``` 

## Lazy property

Delegates.lazy() is a function that returns a delegate that implements a lazy property:
the first call to get() executes the lambda expression passed to lazy() as an argument
and remembers the result, subsequent calls to get() simply return the remembered result.
If you want thread safety, use blockingLazy() instead: it guarantees that the values will
be computed only in one thread, and that all threads will see the same value.
 
``` kotlin
>	class LazySample {
	    val lazy: String by lazy {
	        println("computed!")
	        "my lazy"
	    }
	}

	fun main(args: Array<String>) {
	    val sample = LazySample()
	    println("lazy = ${sample.lazy}")
	    println("lazy = ${sample.lazy}")
	}
``` 

## Observable Property

The observable() function takes two arguments: initial value and a handler for modifications.
The handler gets called every time we assign to `name`, it has three parameters:
a property being assigned to, the old value and the new one. If you want to be able to veto
the assignment, use vetoable() instead of observable().

``` kotlin
>	import kotlin.properties.Delegates

	class User {
	    var name: String by Delegates.observable("no name") {
	        d, old, new ->
	        println("$old - $new")
	    }
	}

	fun main(args: Array<String>) {
	    val user = User()
	    user.name = "Carl"
	}
``` 

## NotNull property

Users frequently ask what to do when you have a non-null var, but you don't have an
appropriate value to assign to it in constructor (i.e. it must be assigned later)?
You can't have an uninitialized non-abstract property in Kotlin. You could initialize it
with null, bit then you'd have to check every time you access it. Now you have a delegate
to handle this. If you read from this property before writing to it, it throws an exception,
after the first assignment it works as expected.
 
``` kotlin
>	import kotlin.properties.Delegates

	class User {
	    var name: String by Delegates.notNull()

	    fun init(name: String) {
	        this.name = name
	    }
	}

	fun main(args: Array<String>) {
	    val user = User()
	    // user.name -> IllegalStateException
	    user.init("Carl")
	    println(user.name)
	}
``` 

## Properties in map 

Properties stored in a map. This comes up a lot in applications like parsing JSON
or doing other "dynamic" stuff. Delegates take values from this map (by the string keys -
names of properties). Of course, you can have var's as well (add import kotlin.properties.setValue),
that will modify the map upon assignment (note that you'd need MutableMap instead of read-only Map).

``` kotlin
>	import kotlin.properties.getValue

	class User(val map: Map<String, Any?>) {
	    val name: String by map
	    val age: Int     by map
	}

	fun main(args: Array<String>) {
	    val user = User(mapOf(
	            "name" to "John Doe",
	            "age"  to 25
	    ))

	    println("name = ${user.name}, age = ${user.age}")
	}
``` 

# Callable references
## Reference to a function
"Callable References" or "Feature Literals", i.e. an ability to pass
named functions or properties as values. Users often ask
"I have a foo() function, how do I pass it as an argument?".
The answer is: "you prefix it with a `::`".


``` kotlin
>	fun main(args: Array<String>) {
	    val numbers = listOf(1, 2, 3)
	    println(numbers.filter(::isOdd))
	}

	fun isOdd(x: Int) = x % 2 != 0
```

## Composition of functions
The composition function return a composition of two functions passed to it:
compose(f, g) = f(g).
Now, you can apply it to callable references.


``` kotlin
>	fun main(args: Array<String>) {
	    val oddLength = compose(::isOdd, ::length)
	    val strings = listOf("a", "ab", "abc")
	    println(strings.filter(oddLength))
	}

	fun isOdd(x: Int) = x % 2 != 0
	fun length(s: String) = s.length

	fun <A, B, C> compose(f: (B) -> C, g: (A) -> B): (A) -> C {
	    return { x -> f(g(x)) }
	}
```

# Longer Examples
## 99 Botles of Beer

This example implements the famous "99 Bottles of Beer" program
See http://99-bottles-of-beer.net/
The point is to print out a song with the following lyrics:
* The "99 bottles of beer" song
* 99 bottles of beer on the wall, 99 bottles of beer.
* Take one down, pass it around, 98 bottles of beer on the wall.
* 98 bottles of beer on the wall, 98 bottles of beer.
* Take one down, pass it around, 97 bottles of beer on the wall.
*   ...
* 2 bottles of beer on the wall, 2 bottles of beer.
* Take one down, pass it around, 1 bottle of beer on the wall.
* 1 bottle of beer on the wall, 1 bottle of beer.
* Take one down, pass it around, no more bottles of beer on the wall.
* No more bottles of beer on the wall, no more bottles of beer.
* Go to the store and buy some more, 99 bottles of beer on the wall.

Additionally, you can pass the desired initial number of bottles to use (rather than 99)
as a command-line argument 
``` kotlin
>	package bottles

	fun main(args: Array<String>) {
	    if (args.isEmpty) {
	        printBottles(99)
	    } else {
	        try {
	            printBottles(Integer.parseInt(args[0]))
	        } catch (e: NumberFormatException) {
	            System.err.println("You have passed '${args[0]}' as a number of bottles, " +
	                    "but it is not a valid integer number")
	        }
	    }
	}

	fun printBottles(bottleCount: Int) {
	    if (bottleCount <= 0) {
	        println("No bottles - no song")
	        return
	    }

	    println("The \"${bottlesOfBeer(bottleCount)}\" song\n")

	    var bottles = bottleCount
	    while (bottles > 0) {
	        val bottlesOfBeer = bottlesOfBeer(bottles)
	        print("$bottlesOfBeer on the wall, $bottlesOfBeer.\nTake one down, pass it around, ")
	        bottles--
	        println("${bottlesOfBeer(bottles)} on the wall.\n")
	    }
	    println("No more bottles of beer on the wall, no more bottles of beer.\n" +
	            "Go to the store and buy some more, ${bottlesOfBeer(bottleCount)} on the wall.")
	}

	fun bottlesOfBeer(count: Int): String =
	        when (count) {
	            0 -> "no more bottles"
	            1 -> "1 bottle"
	            else -> "$count bottles"
	        } + " of beer"

	An excerpt from the Standard Library



	// This is an extension property, i.e. a property that is defined for the
	// type Array<T>, but does not sit inside the class Array
	val <T> Array<T>.isEmpty: Boolean get() = size == 0
```

## HTML Builder
This is an example of a Type-Safe Groovy-style Builder
Builders are good for declaratively describing data in your code.
In this example we show how to describe an HTML page in Kotlin.
See this page for details:
http://kotlinlang.org/docs/reference/type-safe-builders.html

``` kotlin
>	package html

	fun main(args: Array<String>) {
	    val result =
	            html {
	                head {
	                    title { +"XML encoding with Kotlin" }
	                }
	                body {
	                    h1 { +"XML encoding with Kotlin" }
	                    p { +"this format can be used as an alternative markup to XML" }

	                    // an element with attributes and text content
	                    a(href = "http://jetbrains.com/kotlin") { +"Kotlin" }

	                    // mixed content
	                    p {
	                        +"This is some"
	                        b { +"mixed" }
	                        +"text. For more see the"
	                        a(href = "http://jetbrains.com/kotlin") { +"Kotlin" }
	                        +"project"
	                    }
	                    p { +"some text" }

	                    // content generated from command-line arguments
	                    p {
	                        +"Command line arguments were:"
	                        ul {
	                            for (arg in args)
	                                li { +arg }
	            }
	                    }
	                }
	            }
	    println(result)
	}

	interface Element {
	    fun render(builder: StringBuilder, indent: String)
	}

	class TextElement(val text: String) : Element {
	    override fun render(builder: StringBuilder, indent: String) {
	        builder.append("$indent$text\n")
	    }
	}

	abstract class Tag(val name: String) : Element {
	    val children = arrayListOf<Element>()
	    val attributes = hashMapOf<String, String>()

	    protected fun <T : Element> initTag(tag: T, init: T.() -> Unit): T {
	        tag.init()
	        children.add(tag)
	        return tag
	    }

	    override fun render(builder: StringBuilder, indent: String) {
	        builder.append("$indent<$name${renderAttributes()}>\n")
	        for (c in children) {
	            c.render(builder, indent + "  ")
	        }
	        builder.append("$indent</$name>\n")
	    }

	    private fun renderAttributes(): String? {
	        val builder = StringBuilder()
	        for (a in attributes.keys) {
	            builder.append(" $a=\"${attributes[a]}\"")
	    }
	        return builder.toString()
	    }


	    override fun toString(): String {
	        val builder = StringBuilder()
	        render(builder, "")
	        return builder.toString()
	    }
	}

	abstract class TagWithText(name: String) : Tag(name) {
	    operator fun String.unaryPlus() {
	        children.add(TextElement(this))
	    }
	}

	class HTML() : TagWithText("html") {
	    fun head(init: Head.() -> Unit) = initTag(Head(), init)

	    fun body(init: Body.() -> Unit) = initTag(Body(), init)
	}

	class Head() : TagWithText("head") {
	    fun title(init: Title.() -> Unit) = initTag(Title(), init)
	}

	class Title() : TagWithText("title")

	abstract class BodyTag(name: String) : TagWithText(name) {
	    fun b(init: B.() -> Unit) = initTag(B(), init)
	    fun p(init: P.() -> Unit) = initTag(P(), init)
	    fun h1(init: H1.() -> Unit) = initTag(H1(), init)
	    fun ul(init: UL.() -> Unit) = initTag(UL(), init)
	    fun a(href: String, init: A.() -> Unit) {
	        val a = initTag(A(), init)
	        a.href = href
	    }
	}

	class Body() : BodyTag("body")
	class UL() : BodyTag("ul") {
	    fun li(init: LI.() -> Unit) = initTag(LI(), init)
	}

	class B() : BodyTag("b")
	class LI() : BodyTag("li")
	class P() : BodyTag("p")
	class H1() : BodyTag("h1")

	class A() : BodyTag("a") {
	    public var href: String
	        get() = attributes["href"]!!
	        set(value) {
	            attributes["href"] = value
	        }
	}

	fun html(init: HTML.() -> Unit): HTML {
	    val html = HTML()
	    html.init()
	    return html
	}
```
## Game of life
Ommitted
## Maze
Ommitted

#Problems
## Sum
Your task is to implement the sum() function so that it computes the sum of
all elements in the given array a.

```kotlin
>	package sum

	fun sum(a: IntArray): Int {
	    var result=0
	      for (number in a)
	    	result += number
	    return result
	}
```	
## indexOfMax
Your task is to implement the indexOfMax() function so that it returns
the index of the largest element in the array, or null if the array is empty.
```kotlin
>	package maxindex

	fun indexOfMax(a: IntArray): Int? {
	    var maxNumber = Integer.MIN_VALUE
	    var result = 0
	    if (a.size==0)
	    	return null
	    else{
	    	for (i in a.indices){
	            maxNumber = max(maxNumber,a[i])
				if(maxNumber == a[i])
		            result = i
	        }
	    }
	    return result
	}
	fun max(a: Int, b: Int) = if (a > b) a else b
```
## Runs

Any array may be viewed as a number of "runs" of equal numbers. 
For example, the following array has two runs:  1, 1, 1, 2, 2
Three 1's in a row form the first run, and two 2's form the second. 
This array has two runs of length one:  3, 4
And this one has five runs:  1, 0, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0  
Your task is to implement the runs() function so that it returns the number of runs in the given array.

```kotlin
>	package runs

	fun runs(a: IntArray): Int {
		if (a.size==0)
		    return 0
	    
	    var runNumber =a[0]
	    var result = 1
	    
	    for (number in a)
	    	if(runNumber != number){
	    		result ++
	            runNumber = number
	        }
	    return result
	}
```
## Pairless
Think of a perfect world where everybody has a soulmate.
Now, the real world is imperfect: there is exactly one number in the array
that does not have a pair. A pair is an element with the same value.  

For example in this array:
  1, 2, 1, 2
every number has a pair, but in this one:
  1, 1, 1
one of the ones is lonely.

Your task is to implement the findPairless() function so that it finds the
lonely number and returns it.
A hint: there's a solution that looks at each element only once and uses no
data structures like collections or trees.

```kotlin
>	package pairless

	fun findPairless(a: IntArray): Int {
	    val distinctElements = a.distinct()
	    for (element in distinctElements){
	        if(!a.count{it == element}.isEven())
	           return element
	    }
	    return 0
	}
	fun Int.isEven() = this.mod(2) !=1
```
# Canvas
Ommitted
#Koans
## joinOptions
```kotlin
>	fun joinOptions(options: Collection<String>) = options.joinToString(", ","[","]")
```
##Default arguments

```kotlin

	fun foo(name: String, number: Int = 42, toUpperCase: Boolean = false) =
        (if (toUpperCase) name.toUpperCase() else name) + number

	fun useFoo() = listOf(
	        foo("a"),
	        foo("b", number = 1),
	        foo("c", toUpperCase = true),
	        foo(name = "d", number = 2, toUpperCase = true)
	)
```
## Lambdas
```kotlin
>	fun containsEven(collection: Collection<Int>): Boolean = collection.any { it.mod(2) == 0 }
```	

## Strings
```kotlin
>	val s = "abc"
	val str = "$s.length is ${s.length}" // evaluates to "abc.length is 3"
	...
	val month = "(JAN|FEB|MAR|APR|MAY|JUN|JUL|AUG|SEP|OCT|NOV|DEC)"
	fun getPattern() = """\d{2} ${month} \d{4}"""
```	

## Data classes
```kotlin
>	data class Person (var name: String, var age: Int) 

	fun getPeople(): List<Person> {
	    return listOf(Person("Alice", 29), Person("Bob", 31))
	}
```	

##Nullable types
```kotlin
>	fun sendMessageToClient(client: Client?, message: String?, mailer: Mailer){
		    val mail = client?.personalInfo?.email ?: return
		    if (message!=null)
			    mailer.sendMessage(mail, message)
		}

	class Client (val personalInfo: PersonalInfo?)
	class PersonalInfo (val email: String?)
	interface Mailer {fun sendMessage(email: String, message: String)}
```	

## Smart Cast
```kotlin
>	fun eval(expr: Expr): Int =
        when (expr) {
            is Num -> expr.value
            is Sum -> eval(expr.left) + eval(expr.right) 
            else -> throw IllegalArgumentException("Unknown expression")
        }

	interface Expr
	class Num(val value: Int) : Expr
	class Sum(val left: Expr, val right: Expr) : Expr
```	

## Extension Functions
```kotlin
>	fun Int.r(): RationalNumber = RationalNumber(this,1)
	fun Pair<Int, Int>.r(): RationalNumber = RationalNumber(this.component1(),this.component2())

	data class RationalNumber(val numerator: Int, val denominator: Int)
```	

## Object expressions
```kotlin
>	import java.util.*

	fun getList(): List<Int> {
	    val arrayList = arrayListOf(1, 5, 2)
	    Collections.sort(arrayList, object : Comparator<Int> {
	    	override fun compare(x: Int, y: Int) = y - x
	    })
	    return arrayList
	}
```	

## SAM conversions
```kotlin
>	import java.util.*

	fun getList(): List<Int> {
	    val arrayList = arrayListOf(1, 5, 2)
	    Collections.sort(arrayList, { x, y -> y-x })
	    return arrayList
	}
```	

## Extension functions on collections
```kotlin
>	fun getList(): List<Int> {
    	return arrayListOf(1, 5, 2).sortedDescending()
	}
```	

##
```kotlin
>	
```	


##
```kotlin
>	
```	


##
```kotlin
>	
```	

#Advent of Code
--- Day 1: Not Quite Lisp ---

Santa was hoping for a white Christmas, but his weather machine's "snow" function is powered by stars, and he's fresh out! To save Christmas, he needs you to collect fifty stars by December 25th.

Collect stars by helping Santa solve puzzles. Two puzzles will be made available on each day in the advent calendar; the second puzzle is unlocked when you complete the first. Each puzzle grants one star. Good luck!

Here's an easy puzzle to warm you up.

Santa is trying to deliver presents in a large apartment building, but he can't find the right floor - the directions he got are a little confusing. He starts on the ground floor (floor 0) and then follows the instructions one character at a time.

An opening parenthesis, (, means he should go up one floor, and a closing parenthesis, ), means he should go down one floor.

The apartment building is very tall, and the basement is very deep; he will never find the top or bottom floors.

For example:

(()) and ()() both result in floor 0.
((( and (()(()( both result in floor 3.
))((((( also results in floor 3.
()) and ))( both result in floor -1 (the first basement level).
))) and )())()) both result in floor -3.
To what floor do the instructions take Santa?

Your puzzle answer was 138.

The first half of this puzzle is complete! It provides one gold star: *

--- Part Two ---

Now, given the same instructions, find the position of the first character that causes him to enter the basement (floor -1). The first character in the instructions has position 1, the second character has position 2, and so on.

For example:

) causes him to enter the basement at character position 1.
()()) causes him to enter the basement at character position 5.
What is the position of the character that causes Santa to first enter the basement?


```kotlin
>	
```	