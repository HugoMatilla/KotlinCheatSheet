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
_if_ is an expression, i.e. it returns a value.
Therefore there is no ternary operator (condition ? then : else),
because ordinary _if_ works fine in this role.
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
 other goodies common for data: toString(), equals(), hashCode() and copy().
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
component functions, toString(), equals(), hashCode() and copy().
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
to provide methods named get() and set() to be called.</p>
 
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