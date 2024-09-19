# Annotations

Annotations are like "markers" that add some magic to your code. Currently, 2 of these 
are present in code:

## LowPriorityInOverloadResolution
Sometimes, the kotlin compiler has difficulty resolving which method to choose
when there are a bunch of methods with the same name, but different type parameters
(these are called overloads). This annotation simply marks one method as lower priority
than the others.

## JvmName
Kotlin code compiles to the same bytecode as java does. In other words, kotlin functions/classes
can be instantiated within java, and vise versa. However, there are times where you have to
have a java function name to be different than the kotlin one. In this case, the @JvmName
annotation will set the java function name to something different than the kotlin function name.

## JvmField
Under the hood, kotlin properties actually get convertd to getProperty() and setProperty()
methods when called from java:
```
class A{
   val hi: Double = 2.0
}

// java
A inst = new A();
inst.getHi();
inst.setHi(5.0);
```
This is due to the existence of getters and setters in kotlin code, which have to be converted
to get and set methods in java. To prevent this, use the @JvmField annotation.