# Subsystems 

Most subsystems in WPILib are templated off of the ```SubsystemBase``` class. 
In ChargerLib, we instead use the ```SuperSubsystem(subsystemName)``` base class
instead. ```SuperSubsystem``` extends SubsystemBase(and has identical functionality to it), 
but provides builtin logging and tuning capabilities to you're subsystems.

``` 
class Arm: SuperSubsystem("Arm"){
    // Add whatever methods you want here
}
```

