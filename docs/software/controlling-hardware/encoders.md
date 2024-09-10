# Encoders
An encoder is a device that helps read the angular position of a certain mechanism.
There are 2 different types of encoders: relative encoders
(usually integrated into motor controllers) and absolute encoders(usually separate devices).

Absolute encoders always know their position: if you turn them on and off, they will still memorize their exact position.
On the other hand, relative encoders don't know their exact position when turned on and off; on startup, they return a position of 0.
Relative encoders are usually found integrated into motor controllers, while absolute encoders are usually separate devices.

Note: there are also quadrature encoders and analog encoders, but they don't really serve a use anymore
in modern FRC.

## Using encoders in software
Encoders within ChargerLib are represented via 2 interfaces:

1. ```PositionEncoder```(an encoder that only measures position)
2. ```Encoder```(an encoder that measures angular velocity and position).

The position of encoder is obtained with the ```angularPosition``` property(returns a kmeasure Angle),
and the velocity of an encoder is obtained with the ```angularVelocity``` property(returns a kmeasure AngularVelocity).

```
   public class SingleJointedArm(val encoder: Encoder, ....){
       public val position: Angle = encoder.angularPosition
       public val velocity: AngularVelocity = encoder.angularVelocity
       
       init{
            println("Arm position in degrees: " + encoder.angularPosition.inUnit(degrees))
       }
   }
   
   val armInstance = SingleJointedArm(ChargerCANcoder(5), ....)
```

## Different Types of External Encoders
|                  Encoder                  |                            Details                             |  ChargerLib wrapper class   |    Base Class    |  Implements(interfaces):  |
|:-----------------------------------------:|:--------------------------------------------------------------:|:---------------------------:|:----------------:|:-------------------------:|
|                 CANcoder                  | Made by CTRE electronics; both a relative and absolute encoder |     ChargerCANcoder(id)     |     CANcoder     | Encoder & PositionEncoder |
| CTRE mag encoder, REV throughbore encoder |  All considered duty-cycle encoders and only measure position  | ChargerDutyCycleEncoder(id) | DutyCycleEncoder |   PositionEncoder only    |
|                 Canandmag                 |                     Made by Redux robotics                     |    ChargerCanandmag(id)     |    Canandmag     | Encoder & PositionEncoder | 

Note: All motors(which we will talk about in the motors subpage) have integrated relative encoders.