# Why add custom wrappers around hardware?

In traditional robot programming, each motor controller and encoder is represented with their own class.
The method of fetching readings and setting requests varies from device to device; which means that 
software devs usually have to learn the apis of each distinct motor controller.

ChargerLib provides a set of wrapper classes around modern encoders and motor controllers for a couple of reasons:

1. Units support: different vendors will usually return encoder/motor readings in different units; so, having them automatically
converted to a kmeasure ```Quantity``` makes the process more seamless.
2. Standardization: Instead of having to comprehensively learn the API's of multiple vendors, there is 1 standard API for all motors/encoders.
3. Substitution: You can swap a Spark max with a TalonFX(or a simulated spark max) without any code changes!

## Hardware wrapper rules of thumb

1. To access the base motor controller/encoder/gryoscope class, you can use the "base" property. 
   For instance, ```ChargerCANcoder``` is a wrapper around a ```CANcoder```; to fetch the internal
   CANcoder, you can call ```val baseEncoder: CANcoder = ChargerCANcoder(5).base```.
2. All hardware wrappers can access their ID via the deviceID property(except for sim motors)