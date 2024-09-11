# Kmeasure

Kmeasure is a custom units library that we use. 

Even though WPILib itself has a units library, it is significantly constrained
by performance, while Kmeasure is not. 

[Check out Kmeasure's Github page here!](https://github.com/battery-staple/KMeasure)

### Here is a quick overview of how to use Kmeasure:

1. All "types" of quantities have their very own type name. For instance,
you represent an angle with the type ```Angle```, a distance with type ```Distance```, 
and so on.
2. In order to express a quantity, you would use <value>.<unit>: like ```3.degrees```, 
```4.0.inches```, and so on. IntelliJ will give you an option to automatically
import these units for you. Alternatively, you can also use ```4.ofUnit(inches)```.
3. In order to convert any "unit" value to a ```double```, use the ```inUnit``` function.
For instance: ```angleValue.inUnit(radians)```, ```distanceValue.inUnit(meters)```.


### Here are some more advanced concepts:

In Kmeasure, all values are templated off of the ```Quantity<D: Dimension>``` class. 

This class has only one property: 'siValue', which is the base value of the quantity 
in the International System of Unit's designated base unit for the class.

The Dimension type represents a generic unit/dimension, and is used to distinguish between different ```Quantity```'s. It is represented using 4 components: Mass(M), Length(L), Time(T) and Current(I). For instance:
```
typealias dimensionA =  Dimension<Mass0, Length0, Time1, Current0> // dimension = time
typealias dimensionB =  Dimension<Mass1, LengthN1, Time2, Current1> // represents mass^1 * length^-1 * time^2 * current^1
``` 
Dimension is a sealed class; thus, it can only be passed as a type argument, and cannot be instantiated by itself.
In order to represent a generic dimension, use ```Dimension<*,*,*,*>``` as a type bound. 

There are named typealiases for common dimensions and Quantity's. For instance:
```
typealias TimeDimension = Dimension<Mass0, Length0, Time1, Current0>
typealias Time = Quantity<TimeDimension>

// instead of doing this
val time: Quantity<Dimension<Mass0, Length0, Time1, Current0>> = Quantity(0.0)
// you can now do this:
val time: Time = Time(0.0)
// or this: (TimeDimension
val time: Quantity<TimeDimension> = Quantity(0.0)
```

### Why is Quantity<D> faster than the wpilib units library?
The Quantity class is an example of an inline value class. An inline value class
stores a singular property(here, it is siValue), and the kotlin runtime
treats the class as if it were actually the property's value instead of an object.
This allows them to be more performant than other classes, at the cost
of only being able to store a singular value. 
