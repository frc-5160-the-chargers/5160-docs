# BuildCommand

In regular wpilib, there are 2 types of commands: subclassed commands(commands that extend
the ```Command``` base class), and inline command compositions(```InstantCommand```, 
```RunCommand```, ```ParallelCommandGroup```, etc.)

However, ChargerLib introduces a new alternative: the ```buildCommand``` domain-specific language.
A domain-specific language is a relatively complex term to basically encapsulate a
set of functions that generates some kind of output. Here, function calls within
a body of a ```buildCommand``` block generate a command for you.

This example gives a pretty self-explanatory explanation for how it works:
``` 
val armCommand: Command = buildCommand(name = "ArmCommand", log = true) {
    println("hi") // This is run when the command is created; that's it.
    val property = 2.0
    
    require(drivetrain, arm, elevator) // Adds requirements of buildCommand
    
    runOnce {
        println("Command started!") // This will run once every time the command is scheduled
    }
    
    loopForDuration(5) {
        motor.appliedVoltage = 5.volts // A motor will be set to 5V for 5 seconds
        drivetrain.drive()
    }
    
    runParallelUntilOneFinishes {
        runSequentially {
            runOnce{ println("hi!") }
            
            loopForDuration(5){ println("bye!") }
        }
        
        wait(5)
    }
    
    loop{
        motor.stop() // This command will call stop() forever until it's interrupted
    }
}
```


