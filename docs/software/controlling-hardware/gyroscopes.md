# Gyroscopes

A basic class that can return the heading of the robot is represented as a ```HeadingProvider```, 
which is accessed using the ```heading``` property:

```kotlin
// drivetrains can provide heading via encoder readings
val headingProvider: HeadingProvider = EncoderDifferentialDrivetrain(....)
val headingProviderTwo: HeadingProvider = ChargerNavX()

val heading: Angle get() = headingProviderTwo.heading
```

A heading provider that can be zeroed is represented using the  ```ZeroableHeadingProvider``` interface, 
which has the ```zeroHeading(angle: Angle)``` function. When called without a specified angle, 
this function will set the angle to 0 degrees.

In addition, there are the ```ThreeAxisGyroscope```, ```ThreeAxisAccelerometer``` and ```ThreeAxisSpeedometer``` classes, 
which measure angle, acceleration and velocity, respectively, in all 3 axes. For instance:

```kotlin
val navX = ChargerNavX()
val gyroscope: ThreeAxisGyroscope = navX.gyroscope

println(gyroscope.yaw + gyroscope.pitch + gyroscope.roll) // Angle

val accelerometer: ThreeAxisAccelerometer = navX.accelerometer

println(accelerometer.xAcceleration + accelerometer.yAcceleration + ...) // Acceleration

val speedometer: ThreeAxisSpeedometer = navX.speedometer

println(accelerometer.xVelocity + accelerometer.yVelocity + ...) // Velocity
```

These interfaces are usually subclassed properties of overarching IMU classes.

HeadingProvider implementing classes: 
- drivetrain subsystems
- ```ChargerPigeon2()```
- ```ChargerNavX()``
- 

```ChargerPigeon2(id)``` has subclassed ThreeAxisGyroscope and ThreeAxisAccelerometer implementations.
In addition, it supports fetching yaw rate, pitch rate and roll rate as well.

```ChargerNavX()```  has subclassed ThreeAxisGyroscope, ThreeAxisAccelerometer and ThreeAxisSpeedometer impls.
