# ChargerRobot

```ChargerRobot``` is the quintessential base class for robots
built using ChargerLib.

For every type of "Robot" that a codebase has, you would create a class
that extends ```ChargerRobot```, like so:

```
class CompetitionBot: ChargerRobot() {
    ...
}
```

ChargerRobot extends WPILib's [TimedRobot](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/creating-robot-program.html)
class; however, it allows code to be run periodically without being in the robot class
itself. Thus, it is required for your robot class to extend ChargerRobot instead of TimedRobot. 

### Differences from traditional command-based
In traditional WPILib, the main class is split into the "Robot" class 
and the "RobotContainer" class; however, we combine both into a single
Robot class for simplicity.


### Standard ChargerRobot class

```
import edu.wpi.first.wpilibj2.command.button.RobotModeTriggers.autonomous

class Robot: ChargerRobot() {
    private val autoCommand = ...
    private val controller = CommandXboxController()
    private val drivetrain = Drivetrain()
    private val arm = Arm()
    private val elevator = Elevator()
    
    // Button binding configuration, default commands, and autonomous command config
    // go here
    override fun robotInit(){
        // autoCommand will now run during autonomous mode
        // and stop when auto mode ends
        autonomous().whileTrue(autoCommand) 
        drivetrain.defaultCommand = ...
        
        controller.x().whileTrue(SomeRandomCommand())
    }
    
    // super.robotPeriodic() already calls the command scheduler periodically,
    // and does other things as well
    override fun robotPeriodic() {
        doSomething()
        super.robotPeriodic() // You MUST call this, or things will break
    }
```

### Adding periodic callbacks using ChargerRobot

The ```ChargerRobot``` class gives users the unique ability to add periodic callbacks
wherever they want in their code.

There are 3 ways to add periodic callbacks:

```
fun addCallbacks(){
    ChargerRobot.runPeriodic {
        // this block of code will run periodically
        // before the command scheduler
    }
    
    ChargerRobot.runPeriodicWithLowPriority {
        // this block of code will run periodically
        // after the command scheduler
    }
    
    ChargerRobot.runPeriodicAtPeriod(0.01.seconds) {
        // This block of code will run periodically
        // at the specified period.
        // This allows for code to run at a higher(or lower rate) than then normal loop.
    }
}
```


