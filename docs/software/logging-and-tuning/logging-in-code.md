# Logging in Robot Code

Our team uses the [DogLog](https://doglog.dev/) logging library(made by 
FRC 581) to log our robot code. 

DogLog provides the ability to read live logs(viewed via a system called NetworkTables)
and to log data to a .WPILOG file(here is the AdvantageScope) 

Before you read the rest of the section,
we expect you to roughly read the official docs first(it's pretty intuitive overall).

## HorseLog

ChargerLib introduces the HorseLog logger, which works the same way as the DogLog 
logger, but adds support for logging Kmeasure Quantities, lists, nullable values, 
and more.
```
HorseLog.log("Arm/position", Angle(0.0))
// IMPORTANT: Logging lists has a pretty significant overhead, so use it with caution.
// If you want to be more optimal, you can use arrays instead. 
HorseLog.log("Arm/velocityValues", listOf(AngularVelocity(0.0), 1.radians / 1.seconds))
```
However, the log() function of HorseLog can actually be directly imported(and called by itself)
without the HorseLog qualifier(this is called a static import). In the base robot class, 
call the logger explicitly; in subystems, just import the log() method instead.
```
import frc.chargers.framework.HorseLog.log // directly imports the method
log("Arm/position", Angle(0.0)
logNullableInt("Arm/something", null)
logNullableDouble("Arm/hi", 2.0)
```

# Auto-logged properties

ChargerLib also allows you to define automatically logged properties(both getter and regular ones).
These properties can only be used within a class.
```
class Arm {
    val angle by logged { motor.encoder.angularPosition } // getter property logged as Arm/angle(SI Value)
    val velocity by logged("Arm/vel"){ motor.encoder.angularVelocity }
    var isActivated by logged(false) // regular mutable property logged as Arm/isActivated
}
```

