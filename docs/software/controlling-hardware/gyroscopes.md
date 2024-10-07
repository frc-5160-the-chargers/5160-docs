# Gyroscopes

## What is an IMU?
An IMU, or an inertial measurement unit, is a common piece of hardware on robots that serves one primary purpose: to get the direction the robot is tilting or facing. All IMUs support fetching the robot's current roll, pitch, and yaw(with yaw being the most important by far). In addition, IMUs allow you to get pretty accurate linear acceleration readings of the robot as well.

The 2 most common IMUs in robotics are the Pigeon2 and the NavX. The Pigeon2 is overall better than the NavX(currently though, we only have a NavX).

## How do you code them?
A basic class that can return the heading of the robot is represented as a ```HeadingProvider```, 
which is accessed using the ```heading``` property. All IMUs(ChargerPigeon2, ChargerNavX) implement this interface, and most drivetrain classes(we will talk about these later) also implement this interface as well(since you can get your robot's direction, albeit less accurately, from encoders).
```
// drivetrains can provide heading via encoder readings
val headingProvider: HeadingProvider = EncoderDifferentialDrivetrain(....)
val headingProviderTwo: HeadingProvider = ChargerNavX()

val heading: Angle get() = headingProviderTwo.heading
```

A heading provider that can be zeroed is represented using the  ```ZeroableHeadingProvider``` interface, 
which has the ```zeroHeading(angle: Angle)``` function. When called without a specified angle, 
this function will set the angle to 0 degrees. ChargerNavX and ChargerPigeon2 implement ZeroableHeadingProvider.

In addition, the ChargerNavX and ChargerPigeon2 return the linear accelerations and roll,
pitch, and yaw of the gyro itself.

```
val navX = ChargerNavX()
println(navX.yaw + navX.pitch + navX.roll) // Angle kmeasure quantity
println(navX.yawRate) // note: the navX does not support pitchRate and rollRate(the Pigeon2 does)
println(navX.xAcceleration + navX.yAcceleration + ...) // Acceleration kmeasure quantity
```

HeadingProvider implementing classes: 
- drivetrain subsystems
- ```ChargerPigeon2()```
- ```ChargerNavX()```

```ChargerPigeon2(id)``` supports fetching roll, pitch, yaw, rollRate, pitchRate, yawRate, xAcceleration, yAcceleration
and zAcceleration.

```ChargerNavX()``` supports everything that the Pigeon2 does, except that it does not support.
