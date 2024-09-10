# Kotlin and Java

## Why Kotlin?

The three standard languages for FRC are Java, C++ and Python.
However, kotlin is also a supported language because fundamentally, 
it is the same language as java; as both languages eventually compile to
the same underlying code. This means that kotlin can call java, and java can call kotlin!

## I'm new to kotlin and don't know a lot of java. What do I do?
To get started with learning kotlin, 
[read the official documentation here](https://kotlinlang.org/docs/home.html).

It is recommended to read the "Basic Syntax" page under the "Basics" category
and everything in the "Concepts" page up until "This expressions".

Finally, make sure to check out the "General Programming things you might not have learned yet" section.

## I'm bad at kotlin but good at java. What do I do?
Here is a quick guide to switch to kotlin from java.

| Task                    | Kotlin                                                   | Java                                                   |
|-------------------------|----------------------------------------------------------|--------------------------------------------------------|
| Printing Output         | ```println(output)```                                    | ```System.out.println(value);```                       |
| Primitive Types         | ```Int, Double, ...``` - can be used in arrays           | ```int, double``` - must use Integer & Double in Lists |
| Constants               | ```val x: optional_type = value```                       | ```final type x = value;```                            |
| Variables               | ```var x: optional_type = value```                       | ```type x = value;```                                  |
| Functions               | ```fun name(p1: type): type{...}  ```                    | ```type name(){...}```                                 |
| Class Constructors      | ```constructor(){}```                                    | ```public ClassName(){}```                             | 
| Class Instantiation     | ```val x = ClassName()```                                | ```ClassName x = new ClassName();```                   | 
| Switch Statements       | ["when" statement](https://www.baeldung.com/kotlin/when) | "switch case" statement                                |
| "Anything" Type         | ```Any```                                                | ```Object```                                           |
| Extending other classes | ```class Child: Parent & constructor(p1, p2): super()``` | ```class Child extends Parent & super() method call``` |

Most other things, such as if statements, class definitions, and return statements, 
are identical. Kotlin also doesn't require semicolons at the end of statements.

Additional note: Things are public in kotlin by default. This means that you can exclude
the 'public' keyword from kotlin by default.
```
class A: B {
    private val b: Double
    constructor(a: Double, b: Double): super(a, b){
        this.b = b
    }
    
    fun noReturnType(anything: Any){ ... }
    fun withReturnType(): Double {
        return 2.0
    }
}
```

### Kotlin Features that aren't in java

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

### [.apply and .also](https://kotlinlang.org/docs/scope-functions.html#also)
There are 2 extension methods that can be used on all classes/values: the ```apply``` 
and ```also``` methods.

Let's say you have a class, called ```AClass```, that has the methods ```configureX()``` and
```configureY```. Now, you want to create an ```AClass``` instance, call both methods,
and pass into another method(called ```accept(AClass)``).

You could do this:

```
val instance = AClass()
instance.configureX()
instance.configureY()
accept(instance)
```

However, if you don't want to define a dummy variable, you can use ```also```:
```
accept(
    AClass().also {
        it.configureX()
        it.configureY()
    }
)
```

Apply does the same thing; however, the method call treats the parameter as ```this```
instead(essentially like calling the block within the class definition body).

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
    val hiResult: Int = hi() // will call the invoke function
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

### [Primary constructors](https://kotlinlang.org/docs/classes.html)

Kotlin has a concept of a "primary constructor", which is really just an alternate(shorter) 
way of writing a constructor. You should try and use primary constructors when possible, as they are a lot cleaner
than the regular constructor syntax.

Here is an example:
```
class NoPrimaryConstructorObject: Parent {
    private val p1: Double
    constructor(p1: Double, p2: Double): super(p1, p2) {
        this.p1 = p1
        println("hi!")
    }
}

class WithPrimaryConstructorObject(
    p1: Double, 
    private val p2: Double // a private property that has its value set to a constructor parameter
): Parent(p1, p2) { 
    init {
        println("hi!")
    }
}
```

## General Programming things you might not have learned yet

Note: if you are a really experienced programmer, you can probably skip this section.
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

To do this, a function is represented as an object with only 1 method. These are called 
"function objects".

#### Instantiating function objects

There are 2 ways to obtain a function object:

1. Creating one from an existing function, using the ```::``` operator. 
```
// identical kotlin/java syntax(aside from the variable definition)
val a = myClassInstance::myMethod

val b = ::myIsolatedFunction
```
2. Lambda functions: this is basically a fancy way to say "anonymous function". 
```
// kotlin
val x = {a, b -> a + b}
val x = {a: Double, b: Double -> a + b}
// java
BiFunction<Double, Double, Double> x = (a, b) -> a + b;
BiConsumer<Double, Double> x = (double a, double b) -> System.out.println(a+b);
```

#### Types of function objects

In kotlin, function object types are represented with the type "(Arguments) -> ReturnType".
If a function does not return anything, use "Unit" instead.

For instance, ```() -> Unit``` is a function that accepts nothing and returns nothing.
```(Int, Int) -> Int``` is a function that accepts 2 Ints and returns an Int.

In kotlin, function objects are callable the same way a function is called.
```
fun higherOrderFun(input: (Double) -> Unit){
    input(2.0)
    input(3.0)
    input(4.0)
}
```

Java has a more complex way of doing things(ask Daniel if you're interested).

Finally, a lambda expression can be moved "outside" of it's enclosing function
if it is the last parameter in the respective higher order function.
``` 
fun hello(a: Int, b: () -> Unit){
    b()
}

// both of these do the same thing
hello(2, { println("hi") })
hello(2) { 
    println("hi") 
}
```

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



