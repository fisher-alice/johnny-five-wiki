## Symptom

Adding Servos to the Pololu Zumo Robot results in a loss of fine Motor control.  In other words the Motors only responds to setting the speed to 0 or 255.  All other settings are seemingly ignored, which can give the impression that the Motors are not working at all.

### Underlying problem

Four Arduino pins are used to control the motor driver and therefore control movement of the robot:
* **Digital pin 7** controls the right motor direction (LOW drives the motor forward, HIGH drives it in reverse).
* **Digital pin 8** controls the left motor direction.
* **Digital pin 9** controls the right motor speed with PWM (pulse width modulation).
* **Digital pin 10** controls the left motor speed with PWM.

When adding Servos to the Zumo the PWM Pins are disabled (both in Arduino and Firmata).  This is the rootcause of the above problem.  The reasons for this are [discussed here](http://arduino.cc/en/reference/servo#.Uxo-UOddVR4).

## Solution

When using the Arduino programming language, one can solve this [programmatically by using Timers](https://www.pololu.com/docs/0J57/8.a).

When using Johnny-Five, one is required to remap the PWM pins that control the Motor.  Note that this involves permanently cutting the connections between the pins 9 and 10 and the Motor Driver.  This also requires that you have two pins available to function as the new PWM pins.

***Warning : Doing this will permanently damage your Pololu Zumo!  Read through all steps before starting and only continue if you have the relevant skills and knowledge to resolve any problems that arise!***

### Step 1 : Cutting the Connections

First you’ll cut the connection between Pins 9 and 10 and the Motor Driver.  This is located on the bottom of the Zumo Shield and can be accessed via the battery compartment.

[Where to cut](https://www.dropbox.com/s/vxdxoysjcg5g826/Cutting.jpeg?dl=0)

Using a sharp knife, sever the connections between the innermost two columns of Pins, as I have shown above.

### Step 2 : Remapping the Connections

Your next step is to remap the PWM pins, so that you can control the speed of your Motors.  I remapped pin 9 to pin 5, and pin 10 to pin 11.  To do this you’ll need to place jumper cables between the relevant pins in the innermost column.

[Remapping the pins](https://www.dropbox.com/s/6plzqin34cx4h4w/Remapping.jpeg?dl=0)

This picture shows where the jumper cables are located.  I had to squash them down (especially the jumper cable between 10 and 11), but it should be reasonably clear what you need to do. 

### Step 3 :Updating your Johnny-Five Code

Don’t forget to update your code to reflect the remapped pins!  In my case...

`var leftMotor = new five.Motor([10, 8]);`
`var rightMotor = new five.Motor([9, 7]);`

...was replaced with...

`var leftMotor = new five.Motor([11, 8]);`
`var rightMotor = new five.Motor([5, 7]);`

You should now be able to control both the Motors and your Servos!  If you still encounter problems you should check that the Zumo's power supply is adequate to support the Zumo and all the extra stuff you've added.

## References

* [Pololu Robotics Forum](http://forum.pololu.com/viewtopic.php?f=29&t=9622&sid=49c73328d8ed0aec87189b91385a40a0).
* [GitHub Discussion](https://github.com/rwaldron/johnny-five/issues/309).