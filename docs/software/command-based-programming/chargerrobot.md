# ChargerRobot

```ChargerRobot``` is the quintissential base class for differnet types of robots.

For every type of "Robot" that a codebase has, you would create a class
that extends ```ChargerRobot```, like so:

```
class CompetitionBot: ChargerRobot() {
    ...
}
```

ChargerRobot extends WPILib's [TimedRobot](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/creating-robot-program.html)
class; however, it adds a couple of extra ChargerLib-related features,
such as automatic logging setup and adding periodic loops.

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