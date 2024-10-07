# Drivetrains

Note: before reading this section, make sure you have a firm grasp
of command-based programming.

ChargerLib offers built-in drivetrain classes that we can re-use from year
to year. These classes are the ```EncoderDifferentialDrivetrain```(for differential drives)
and the ```EncoderHolonomicDrivetrain```(for swerve drives, aka what we use this year).

If you don't already know, a tank drivetrain is a basic drivetrain that has 2 sets of motors
on each side. If they spin the same direction, the robot moves forward/backward; 
if they spin in opposite directions, the robot turns. Meanwhile, a swerve drivetrain
is a drivetrain that has 4 "swerve modules": with each module having a steering motor(which
controls the direction the wheel faces) and a driving motor. This allows the robot
to move sideways and move straight while turning.

### EncoderHolonomicDrivetrain

The following creates an instance of a swerve drivetrain object:
```
val realDrivetrain = EncoderHolonomicDrivetrain(
    name = "Drivetrain",
    turnMotors = listOf(ChargerSparkMax(6), ...),
    turnEncoders = listOf(ChargerCANcoder(21), ...), // optional in simulation
    driveMotors = listOf(...),
    constants = SwerveConstants(...),
    gyro = ChargerNavX() // optional in simulation
)
val simDrivetrain = EncoderHolonomicDrivetrain(
    name = "Drivetrain",
    turnMotors = List(4){ MotorSim(DCMotor.getNEO(1), moi) }, // simply replace the real motors with sim motors in sim
    ...
)
```
The ```SwerveConstants``` class is a class that stores various swerve constants. 
```
val constants = SwerveConstants(
    trackWidth = 27.inches, // x direction
    wheelBase = 28.inches, // y direction
    moduleType = ModuleType.Mk4iL2,
    azimuthPID = PIDConstants(kP, kI, kD),
    velocityPID = PIDConstants(kP, kI, kD), // pid constants for closed-loop velocity control
    velocityFF = AngularMotorFeedforward(kS, kV) // feedforward for velocityDrive; must be tuned by SysID(will talk about later)
)
```
There are also numerous optional parameters as well. For instance, you can set ```odometryUpdateRate = 0.05.seconds```
to make the pose estimation run faster, which makes it more accurate.

```EncoderHolonomicDrivetrain``` has many useful methods shown below:
```
val drivetrain = ...

fun robotPeriodic() {
    drivetrain.swerveDrive(0.5, 0.3, 0.0) // will drive forward at 50% max speed, sideways at 30% max speed
    drivetrain.swerveDrive(ChassisPowers(0.5, 0.0, 0.3)) // forward: 50% max speed, rotation: 30% max speed(driving forward while rotating)
    drivetrain.velocityDrive(3.ofUnit(meters/seconds), 2.ofUnit(meters/seconds), AngularVelocity(0.0)) // drives at exact velocities
    drivetrain.velocityDrive(ChassisSpeeds(3.meters / 1.seconds, Velocity(0.0), AngularVelocity(0.0))
    
    val pose: Pose2d = drivetrain.robotPose
    val xMeters: Double = pose.x
    val y: Distance = pose.y.ofUnit(meters) // x and y components are always in meters for the Pose2d class(part of wpilib)
    val angle: Angle = pose.rotation.angle 
    
    drivetrain.resetPose(Pose2d(1.meters, 2.meters, 1.rotations)) // chargerLib defines an overload of Pose2d that accepts kmeasure quantities
    drivetrain.stop()
    drivetrain.stopInX() // Stops with the wheels in an "X" formation to increase traction
}


```
