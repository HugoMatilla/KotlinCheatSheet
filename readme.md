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


## Reading many names from the command line
Line 2 demonstrates the for-loop, that would have been called "enhanced"
if there were any other for-loop in Kotlin.
See http://kotlinlang.org/docs/reference/basic-syntax.html#using-a-for-loop


``` kotlin
>	fun main(args: Array<String>) {
	    for (name in args)
	        println("Hello, $name!")
	}

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