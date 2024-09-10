# Motors and Motor Controllers

A motor is a device that causes something to rotate when supplied with 
electrical power. A motor controller is an electrical cicuit that helps
a motor to regulate the voltage going into it (thus acheiving varying speeds).

## Using Motors in software
The ```Motor``` interface provides standard functions and properties to control a modern 
motor controller. For instance:

- To fetch the built-in encoder of a motor, use the ```motor.encoder``` property.
- To read a motor's current output, use ```motor.statorCurrent```.
- Call ```motor.appliedVoltage``` to read the voltage, and ```motor.appliedVoltage = 5.0.volts```
to set desired voltage.
- Optionally, you can use ```motor.percentOut = 0.5``` to set power to the motor as a percentage
of it's available power.
- To know whether the motor is inverted, use the ```motor.inverted``` boolean getter.

### Configuring motors
A motor usually has a large amount of internal parameters that may or may not be set.

One of these is gear ratio. In robotics, motors are usually attached to another mechanism, which has some sort
of reduction(to give it more force/more speed than the raw motor's power). There might be a gear
spinning 1 rotation / second spinning another gear 3 times bigger, with that final gear connected to an arm joint.
Since absolute encoders are usually connected to the arm joint, they usually read the appropriate values; however,
motor-based relative encoders are connected to the motor itself. This means that the motor gear ratio has to be configured
before the motor's encoders return correct measurements.

To configure a motor, the  ```configure``` method of a motor is used. Every single parameter within the configure() is optional,
but should be used as a named parameter. 
```configure``` also returns the motor itself so that method calls can be chained.
```
motor.configure(inverted = true, rampRate = 45.seconds)
val motor = ChargerSparkMax(6)
                .configure(gearRatio = 96.0, brakeWhenIdle = false)
                .configure(followers = listOf(ChargerSparkMax(5), ChargerTalonFX(3))) // these motors will now spin when the initial motor spins
```

Here is a full list of all configurable parameters:
1. inverted(default false)
2. brakeWhenIdle(default true): if false, the motor will be set to coast mode.
3. optimizeUpdateRate: If true, will only update voltage, current, position, and velocity readings from the motor. Only use this if you don't access the base motor controller(the base property).
4. gearRatio(Double): Explained above
5. startingPosition(Angle): The starting position for the relative encoder
6. followerMotors(List<Motor>): Can be any motor; if it's the same brand, adds vendor-side optimizations
7. rampRate(Time): How fast the motor reaches max power
8. statorCurrentLimit(Current): Useful to prevent over voltages/brownouts
9. positionUpdateRate + velocityUpdateRate(Frequency)
10. positionPID + velocityPID + continuousInput(PIDConstants, PIDConstants, Boolean) - Explained in the controls section(still TBD)

## Types of motors

- ```ChargerSparkMax(id)``` - Corresponds to Neo Motors(base class is CANSparkMax)
- ```ChargerSparkFlex(id)``` - Corresponds to Neo Vortex Motors(base class is CANSparkFlex)
- ```ChargerTalonFX(id)``` - Corresponds to Kraken/Falcon Motors(base class is TalonFX)
- ```MotorSim(...)``` - Simulates a Regular Motor(base class is DCMotorSim)
```ArmMotorSim(...)``` - Simulates a motor around a single jointed arm(base class is SingleJointedArmSim)
- ```ElevatorMotorSim(...)``` - Simulates a motor controlling an elevator(base class is ElevatorSim)

All of these classes implement ```Motor```, and can be used when desired.