# Tuning

ChargerLib supports tunable parameters as well.
```
class Arm {
    val maxSpeed by tunable(2.0.radians / 1.seconds)
    // value will be tuned with degrees.
    val maxAngle by tunable(20.degrees, key = "Arm/maxAngleDegrees", unit = degrees)
}
```
