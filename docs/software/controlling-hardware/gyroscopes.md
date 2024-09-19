# Gyroscopes

A basic class that can return the heading of the robot is represented as a ```HeadingProvider```, 
which is accessed using the ```heading``` property:

```
// drivetrains can provide heading via encoder readings
val headingProvider: HeadingProvider = EncoderDifferentialDrivetrain(....)
val headingProviderTwo: HeadingProvider = ChargerNavX()

val heading: Angle get() = headingProviderTwo.heading
```

A heading provider that can be zeroed is represented using the  ```ZeroableHeadingProvider``` interface, 
which has the ```zeroHeading(angle: Angle)``` function. When called without a specified angle, 
this function will set the angle to 0 degrees.

In addition, the ChargerNavX and ChargerPigeon2 return the linear accelerations and roll,
pitch, and yaw of the gyro itself.

```
val navX = ChargerNavX()
println(navX.yaw + navX.pitch + navX.roll) // Angle
println(navX.yawRate) // note: the navX does not support pitchRate and rollRate(the Pigeon2 does)
println(navX.xAcceleration + navX.yAcceleration + ...) // Acceleration
```

HeadingProvider implementing classes: 
- drivetrain subsystems
- ```ChargerPigeon2()```
- ```ChargerNavX()``

```ChargerPigeon2(id)``` has subclassed ThreeAxisGyroscope and ThreeAxisAccelerometer implementations.
In addition, it supports fetching yaw rate, pitch rate and roll rate as well.

```ChargerNavX()```  has subclassed ThreeAxisGyroscope, ThreeAxisAccelerometer and ThreeAxisSpeedometer impls.
