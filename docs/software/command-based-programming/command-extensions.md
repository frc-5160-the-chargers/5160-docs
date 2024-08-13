# Command Addons

ChargerLib introduces a lot of useful additions to WPILib's base commands library.

### External Lambda Command Overloads

You can now define InstantCommands and RunCommands like this:

```
val oldInstantCommand = InstantCommand({ motor.run() }, drivetrain)
val oldRunCommmand = RunCommand({ motor.run() }, drivetrain)

val newInstantCommand = InstantCommand(drivetrain){
    motor.run()
}
val newRunCommand = RunCommand(drivetrain){
    motor.run()
}
```

### SetDefaultRunCommand

Many times, you want to set the default command of a subsystem
to be a RunCommand. The ```setDefaultRunCommand``` extension method does this
in a shorter and easier way.

```
drivetrain.defaultCommand = RunCommand(drivetrain){
    drivetrain.drive()
}   

drivetrain.setDefaultRunCommand{
    drivetrain.drive()
}
```

### Command Logging

The ```withLogging``` extension function for commands logs the duration of the command,
as well as when it is active or not.

```
val loggedCommand = SomeCommand().withLogging("SomeCommand")
```