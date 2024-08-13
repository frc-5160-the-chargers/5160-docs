# InputAxis

WPILib's ```Trigger``` class is usually sufficient for activating 
and deactivating commands; however, there are times where you want to read
the direct value of certain controller inputs. For instance,
you might want to read the value of the y axis of the left joystick on a xbox controller
and use that to control your drivetrain.

Thus, the ```InputAxis``` class exists to provide useful additions
to reading input from a controller. For instance, you might 
want to rate limit you're y axis input, or add a deadband, or log it's output,
and so on.

```
private val baseController = CommandXboxController()
val controllerX = 
    InputAxis{ controller.rightY }
        .withDeadband(0.2) 
        .withEquation(Polynomial(0.2, 0.1, 0.0)) // 0.2x^2 + 0.1x + 0
        .withMultiplier(0.5)
        .mapToRange(1.0..3.0) // The value is now scaled to between 1.0 and 3.0
        .invert()

val controllerXOutput = controllerX() // InputAxis is callable as a function

```