# Kotlin and Java

## Why Kotlin?

The three standard languages for FRC are Java, C++ and Python.
However, kotlin is also a supported language because fundamentally, 
it is the same language as java; as both languages eventually compile to
the same underlying code. This means that kotlin can call java, and java can call kotlin!

To get started with learning kotlin, 
[read the official documentation here](https://kotlinlang.org/docs/home.html).

### Here is a quick guide of the differences with kotlin and java:

| Task                | Kotlin                                                  | Java                                                     |
|---------------------|---------------------------------------------------------|----------------------------------------------------------|
| Printing Output     | ```println(output)```                                   | ```System.out.println(value);```                         |
| Primitive Types     | ```Int, Double, ...``` - can be used in arrays themself | ```int, double``` - must use "Integer, Double" in arrays |
| Constants           | ```val x: optional_type = value```                      | ```final type x = value;```                              |
| Variables           | ```var x: optional_type = value```                      | ```type x = value;```                                    |
| Functions           | ```fun name(parameters): optional_type{...}```          | ```type name(){...}```                                   |
| Class Constructors  | ```public constructor(){}```                            | ```public ClassName(){}```                               | 
| Class Instantiation | ```val x = ClassName()```                               | ```ClassName x = new ClassName();```                     | 
| Switch Statements   | "when" statement                                        | "switch case" statement                                  |
| "Anything" Type     | ```Any```                                               | ```Object```                                             |

Most other things, such as if statements, class definitions, and return statements, 
are identical. Kotlin also doesn't require semicolons at the end of statements.

## Kotlin Features that aren't in java

### [Extension methods: ](https://kotlinlang.org/docs/extensions.html)
You can "extend" a class with new methods, like so:
```
    fun ClassName.newMethod(): ReturnType{
        // This function has the same "this" as the class
        methodOfClass();
        methodOfClass();
    }
```
If you import this function in another file(intelliJ will automatically give you the option),
you can call it like any other method of the class.

### [Null]()
You might have heard of ```null```, which is the value that stands for nothing.
In java, you can put null as a value of any Object type. In kotlin, you must add a "?"
to the end of the type. 

You cannot do anything with this "?"/nullable type, until after you check
that the value is not null. This includes: returning when a value is null,
using the value in an if statement that checks if the value is null, etc.

```
val hi: Double? = null // Double? represents a number or null

val a = hi + 2.0 // error

if (hi != null){
   val a = hi + 2.0 // valid; hi becomes a regular Double
}
```

### [Operator methods: ](https://kotlinlang.org/docs/operator-overloading.html)
You can now define addition, subtraction, etc. to your own classes. 
Just use the "operator" keyword for any method.
```
    class Example{
        operator fun plus(other: Hello): Hello{...}
        // makes this class "callable" as a function
        operator fun invoke(): Int{}
    }
    
    val hi = Example()
    val bye = Example()
    
    val hibye = hi + bye // will call the plus() function
```

### [Property getters/setters: ](https://kotlinlang.org/docs/properties.html#getters-and-setters)
Instead of having to define getValue() and setValue() methods,
you can make a certain block of code be called when you get or set a variable's value.
```
    class Example{
        var hello: Int
            get(){ // called every time value of "hello" is fetched
                return myFunction()
            }
            set(value){  // called every time value of "hello" is set; only works for mutable properties
                println(value)
            }
    }
    
    val value = Example()
    value.hello // this will call and return myFunction()
    value.hello = 5 // this will print 5
```

### [Alternate constructor definition](https://kotlinlang.org/docs/classes.html)

## General Programming things you haven't learned yet

Note: if you are an experienced programmer, you can probably skip this section.
If not, this will likely be a good read.

### [Generics](https://kotlinlang.org/docs/generics.html)

Generics allow you to have a function/method or class input any type;
not just a singular one. You primarily use this if you want 2 properties or values
to be the same random type that the user of your function/class wants. 

Kotlin and java have identical syntax for generics, like so:

```
// Here, T is the generic
fun <T> example(input: T){...}
// Generics can be "bounded" by another type
// Here, the type T must extend SomeType
// Every property with type T now can access the methods/variables of SomeType
class Example<T: SomeType>{...}
```

Click on the link to learn more about the "in" and "out" keywords(and java's wildcard types);
which are much more complex(and harder to explain here.)

### [Interfaces](https://kotlinlang.org/docs/interfaces.html)

Interfaces are essentially a "template" for a class. 
They define a certain set of functions/properties that a class should have, 
without actually providing an implementation of them. These "unimplemented"
functions are called "abstract methods" and "abstract properties". 
Interfaces may also optionally have regular methods as well.

In return, the compiler forces classes which extend/implement these interface
to provide their own implementation of each of these functions.

Interfaces cannot store any data/state. This means that Interfaces can have:
1. Regular Methods
2. Abstract/Unimplemented Methods
3. Abstract/Unimplemented properties
4. Properties with getters and setters(as these are essentially functions)

This means that interfaces cannot have regular properties/variables!

```
// This is also how you define an interface in java
interface Hello {
    // NOT ALLOWED
    val goodbye: Double = 2.0

    // allowed
    val goodbye: Double get() = 2.0
   
    fun hello(a: Double)
    
    fun helloTwoTimes(a: Double){
        hello(a)
        hello(a)
    }
}
```

### [Lambda functions](https://kotlinlang.org/docs/lambdas.html)

In kotlin(and in java to a certain extent), you can pass in functions as parameters
to other functions/classes. 

In both versions, you would use ```::functionName``` to do this. 
You can also define an function inline, like this:

```
// kotlin
{a: Double, b: Double -> a + b}
// java
(double a, double b) => { return a + b; }
```
These functions are called "lambda functions".

In kotlin, you define a variable that accepts a function with the syntax 
```(TypeA, TypeB) -> ReturnType``` or ```(TypeA, TypeB) => Unit``` for 
functions that dont return anything.

In java, you use ```Function<InputType, ReturnType>, BiFunction<...>, ...```, 
```Supplier<ReturnType```and ```Consumer<InputType>, BiConsumer<...>, ...```.

In kotlin, you can also use an external "block" to represent a lambda
if it is the last argument of a class or function. For instance:

``` 
fun hello(a: () -> Unit){
    a()
}

// both of these are valid
hello({ println("hi") })
hello { 
    println("hi") 
}
```

You may also come across [Inline Functions](https://kotlinlang.org/docs/inline-functions.html)
in our codebase. Don't worry about them for now; just treat them like
regular functions.

### Singletons

Singletons are classes with only one instance. In kotlin, you define a singleton using
the Object keyword:

```
object Something{
    val hello: Double = 2.0
    
    fun doThis(): Double{
        return 2.0
    }
}

Something.hello
Something.doThis()
```

Java doesn't have explicit support for singletons; so developers instead use internal
logic to implement a static getInstance() method for retreiving the only instance
of the class.

### [Enum Classes](https://kotlinlang.org/docs/enum-classes.html) and [Sealed Classes](https://kotlinlang.org/docs/sealed-classes.html)

Let's say that you have a function that takes in a season and returns the
best clothing for that season. How would you represent a season?

You could accept a string; with "spring" being spring, "summer" being summer, and so on.
However, the user could put in "blablabla" into the function and it would successfully compile.

To solve this, kotlin and java have enums, 
which are used to represent multiple "fixed" variants.

```
// use "public enum Hello" in java instead. 
public enum class Hello{
    HI,
    GOODBYE;
    
    fun someMethod(){...}
}

val variant: Hello = Hello.HI
```

Kotlin takes enums a step further with sealed classes. Sealed classes are similar to enum classes;
however, every variant is a class or object in of itself. Read the 2 links above for more details.

### The ClosedRange class

Kotlin provides an option to specify a range with the ```..``` operator, which
returns an instance of a ```ClosedRange<T>``` class.

This range is similar to python's ```range(start, end)```. 

```
val range: ClosedRange<Double> = 2.0..3.0

val range2: ClosedRange<Angle> = 2.degrees..30.degrees
```



