This is just a hacked up example script from Adafruit's PCA9685 PWM servo/LED controller python library
for Raspberry Pi, which will work the Parallax 900-00008 Continuous Rotation Servo which people have some 
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

# Pre-requisites
================

Jump the following pins from the PCA9685 to your Raspberry Pi:

<pre>
GND <---> GND (Pin 6)
SCL <---> I2C SDA (Pin 3)
SDA <---> I2C SCL (Pin 5)
VCC <---> 3.3V (Pin 1) [WARNING!  Do NOT use 5V for this.)
</pre>

Now, add a 4V to 6V power supply or battery pack to the V+ and GND terminal lugs on the PCA9685.  This power must be 
separate.  Do not believe any pages which say otherwise.

Now, plug your Parallax 900-00008 servo into slot 0 on the PCA9685.  The black wire goes to GND, which is the 
outter-most pin on this board.

# Installation
==============

```
# Get your Raspberry Pi ready for I2C
raspi-config  # enable I2C and reboot
apt-get install git build-essential python-dev i2c-tools python-smbus
i2cdetect -y 1

# Get the Adafruit Python PCA9685 library and install it
cd /usr/local/src
git clone https://github.com/adafruit/Adafruit_Python_PCA9685
cd Adafruit_Python_PCA9685
python setup.py install

# Get my hacked-up script and try it out!
cd /usr/local/src/
git clone https://github.com/lleevveell66/ServoControl
cd ServoControl
chmod 755 PCA9685_Parallax900-00008_Test
./PCA9685_Parallax900-00008_Test
```
