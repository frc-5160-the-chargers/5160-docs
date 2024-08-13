# Intro to Command-based Programming

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
and so on.