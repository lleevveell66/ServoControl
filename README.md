This is just a hacked up example script from Adafruit's PCA9685 PWM servo/LED controller python library
for Raspberry Pi, which will work the Paralax 900-00008 Continuous Rotation Servo which people have some 
trouble with.

The trick on this servo is to find the "magic" center pulse which stops the servo.  Any pulse value
higher than the stop pulse will turn the servo counter-clockwise at a rate proportional to the difference 
from the stop pulse.  Any pulse value lower than the stop pulse will turn clockwise, likewise.  I have 
found the minimum, stop, and maximum pulse values on mine to be:

- Minimum = 375
- Stop = 396
- Maximum = 425

at 60Hz on this PCA9685 controller, which has a resolution of 4096, 12-bits.

So, for example, to stop and hold the servo, send pulse of 396.  Sending any pulse from 397 to 425 will make
the servo turn counter-clockwise.  397 will be veeeeery slow.  425 is about as fast as it will get.  

Remember that you get higher torque at lower speeds, so don't try to over-do it.  If you need speed, go for a normal 
DC motor.
