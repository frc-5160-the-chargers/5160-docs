# What are Motors and Encoders?

In robotics, there are 2 key fundamental components to controlling a robot: 
motors and encoders.

### Encoders
An encoder is a device that helps read the angular position of a certain mechanism.
There are 2 different types of encoders: relative encoders
(usually integrated into motor controllers) and absolute encoders(usually separate devices).

While absolute encoders give a useful position reading in of itself, 
relative encoders do not. Instead, relative encoders can only be used 
to determine the change in angle.

### Motors and Motor Controllers

A motor is a device that causes something to rotate when supplied with 
electrical power. A motor controller is an electrical cicuit that helps
a motor to regulate the voltage going into it (thus acheiving varying speeds).


# Controlling Motors and Encoders

Within base WPILib, different encoders/motors from different vendors are called
through code in different ways. However, in chargerlib, we have wrappers around 
existing motor controllers and encoders, which allows motors/encoders
from different vendors to be called the same way. In addition, it allows for easy 
simulation of motors.

### Using Encoders in Software

Encoders within ChargerLib are represented via 2 interfaces:

1. ```PositionEncoder```(an encoder that only measures angular position)
2. ```Encoder```(an encoder that measures angular velocity and position).

Here is a usage example:

```
   public class SingleJointedArm(val encoder: Encoder, ....){
       public val position: Angle = encoder.angularPosition
       public val velocity: AngularVelocity = encoder.angularVelocity
   }
   
   // ChargerCANcoder implements Encoder
   val armInstance = SingleJointedArm(ChargerCANcoder(5), ....)
``` 

Classes that implement ```Encoder```: 
1. ```ChargerCANcoder(id)``` 
2. ```ChargerQuadEncoder(port)```(wraps the wpilib Encoder class)


Classes that implement ```PositionEncoder```: 
1. ```ChargerPotentiometer(port)``` 
2. ```ChargerDutyCycleEncoder(id)```

### Using motors in software

In ChargerLib, motors are represented with the ```Motor``` interface.

The ```Motor``` interface provides standard functions and properties to control a modern 
motor controller. For instance:

- To fetch the built-in encoder of a motor, use the ```motor.encoder``` property.
- To read a motor's stator current, use ```motor.statorCurrent```.
- Call ```motor.appliedVoltage``` to read the voltage, and ```motor.appliedVoltage = 5.0.volts```
to set desired voltage.
- Optionally, you can use ```motor.percentOut = 0.5``` to set power to the motor as a percentage
of it's available power.
- To read and set inverts, use the  ```motor.hasInvert``` getter/setter property.

```
   public class Elevator(val motor: MotorizedComponent, ....){
   
       init{
            motor.hasInvert = true
            val current: Current = motor.statorCurrent
       }
      
       fun run(){
            motor.appliedVoltage = 5.volts
            //motor.percentOut = -0.3
       }
       
       val distancePerAngle = 5.meters
       
       public val position: Distance get() = motor.encoder.angularPosition * distancePerAngle
       public val velocity: Velocity get() = motor.encoder.angularVelocity * distancePerAngle
   }
   
   // neoSparkMax implements Encoder
   val elevatorInstance = Elevator(ChargerSparkMax(6), ....)
   
   val simulatedElevatorInstance = Elevator(MotorSim(...)...)
```

Here are some classes that implement ```Motor```:
- ```ChargerSparkMax(id)``` - Corresponds to Neo Motors
- ```ChargerSparkFlex(id)``` - Corresponds to Neo Vortex Motors
- ```ChargerTalonFX(id)``` - Corresponds to Kraken/Falcon Motors
- ```ChargerTalonSRX(id)```
- ```MotorSim(...)``` - Simulates a Regular Motor
```ArmMotorSim(...)``` - Simulates a motor around a single jointed arm
- ```ElevatorMotorSim(...)``` - Simulates a motor controlling an elevator

### Controlling multiple motors

To control multiple motors, the ```Motor``` interface provides the utility function
```withFollowers```. The ```withFollowers``` function returns the same motor, with 
many other motors that mirror the original motor's commands. 