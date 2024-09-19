_# Intro to Command-Based Programming

Command-based programming is the most popular programming framework for FRC teams
due to it's ease of use and simplicity. 

This section will simply go over the ways ChargerLib extends command-based programming,
and not the fundamentals of it. Thus, it is suggested that you read everything 
[here](https://docs.wpilib.org/en/stable/docs/software/commandbased/index.html)
up until "The Command Scheduler" to get a basic idea of how command-based programming works.

Here is the gist of it:

1. A ```Subsystem``` is a class used to represent a part of the robot
(Arm, Elevator, Shooter, Drivetrain). 
2. A ```Command``` controls multiple subsystems to accomplish a specific action
(ScoreHighCone, ShootInSpeaker, FollowPlottedPath).
3. A ```Trigger``` is used to activate a ```Command``` when a certain condition or
condition(s) return true. This can be from a button press, a sensor reading change,
and so on._

### Trigger syntax

Due to external lambda syntax, you can create Triggers in a much cleaner way, like so:
```
// java syntax
Trigger(() -> condition()).whileTrue(...);
// kotlin syntax
Trigger{ condition() }.whileTrue(...)
```

### Subsystem and Command syntax

Because ```SubsystemBase``` and ```Command``` are abstract classes, 
you must invoke the constructor itself like so:
```
class Elevator: SubsystemBase() { ... }
class SubclassedCommand: Command() { ... }
```

# IMPORTANT: .ignoringDisable()

A lot of command pitfalls can come from not calling the Command.ignoringDisable(true)
method to allow a command to run when disabled.

For default commands and commands that activate on a button press, you don't want
ignoringDisable to be true. However, you might choose to schedule a command in the 
constructor/init block of a class, and if you don't use the ignoringDisable(true)
option, that command will not run. 

In general, think very carefully about whether or not your command should run
when the robot is disabled or not.