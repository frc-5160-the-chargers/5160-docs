# Motors and Motor Controllers

A motor is a device that causes something to rotate when supplied with 
electrical power. A motor controller is an electrical circuit that helps
a motor to regulate the voltage going into it (thus achieving varying speeds).

## Using Motors in software
The ```Motor``` interface provides standard functions and properties to control a modern 
motor controller. For instance:

- To fetch the built-in encoder of a motor, use the ```motor.encoder``` property.
- To read a motor's current output, use ```motor.statorCurrent```.
- Call ```val v = motor.voltageOut``` to read the voltage, and ```motor.voltageOut = 5.0.volts```
to set desired voltage.
- Optionally, you can use ```motor.speed = 0.5``` to set power to the motor as a percentage
of its available power.
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
class Arm {
    val motor = ChargerSparkMax(6)
        .configure(gearRatio = 96.0, brakeWhenIdle = false)
        .configure(followers = listOf(ChargerSparkMax(5), ChargerTalonFX(3))) // these motors will now spin when the initial motor spins
    init {
        motor.configure(inverted = true, rampRate = 45.seconds)
    }
}
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
- ```MotorSim(DCMotor)``` - Simulates a Regular Motor(base class is DCMotorSim)
- ```ArmMotorSim(DCMotor, length)``` - Simulates a motor around a single jointed arm(base class is SingleJointedArmSim)
- ```ElevatorMotorSim(DCMotor, carriageMass)``` - Simulates a motor controlling an elevator(base class is ElevatorSim)

All of these classes implement ```Motor```, and can be used when desired.

### Motor Sim definition

All motor sims require a ```DCMotor``` parameter, which name might be a bit misleading.
A ```DCMotor``` class instance is NOT the motor itself; rather, it is a class
representing a set of constants related to one or a group of motors.

For instance, you would use ```DCMotor.getNEO(1)``` to represent a singular neo motor's
constants. To simulate/represent multiple motors in one simulated motor, 
you would use ```DCMotor.getKrakenX60(2)```or ```DCMotor.getNeoVortex(3)```.

```
val motor1: Motor = MotorSim(DCMotor.getNeoVortex(1))
val armJointMotors: Motor = ArmMotorSim(DCMotor.getFalcon500(2), 0.5.meters)
```