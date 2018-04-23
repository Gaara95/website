DC motors are great for wheeled robots, but if you need more accurate control
of the motors position you should use a servo motor.
Servo motors can be made to turn to a specific angle ranging from 0 to 180
which makes them great for making something like a robot arm.

**Parts:**

* An Arduino.
* A servo motor.

### Wiring

| Arduino        | Servo          |
| :------------- | :------------- |
| Pin9           | Signal         |
| GND            | GND            |
| 5v             | PWR            |

**Diagram:**

<img class="aligncenter wp-image-110 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/servo.png" alt="" width="644" height="600" />

### code
```
// Turns a servo based on readings from an analog sensor.

#include <Servo.h>
// turns a servo to an angle based on readings from analog 0

Servo myServo; // name the servo

int val, mapped; // used to store the reading from analog 0

void setup() {
	myServo.attach(9); // set the pin the servo is connected to
}

void loop() {
	val = analogRead(0); // read analog 0
	// map the readings from the sensor to the servos range.
	mapped = map (val, 100, 900, 0, 180);
	myServo.write(mapped); // turn the servo to the angle read from the sensor
	// print the stuff to the computer
	delay(400);
}
```

We include the servo library and name the servo "myServo", this way if you
have multiple servos attached you can address each one separately:
```
#include <Servo.h>

Servo myServo;
```

We have 2 variables, one to store the readings and one to store the mapped reading:
```
int val, mapped;
```

In *setup()* we tell the servo library that *myServo* is connected to pin 9:
```
void setup() {
	myServo.attach(9); // set the pin the servo is connected to
}
```

In *loop()* we read from the sensor then we use *map()* to make the readings
from the sensor to the 0 to 180 range that the servo needs, then we tell the servo
to go to the value of *mapped* by using *myServo.write()* :
```
void loop() {
	val = analogRead(0); // read analog 0
	// map the readings from the sensor to the servos range.
	mapped = map (val, 100, 900, 0, 180);
	myServo.write(mapped); // turn the servo to the angle read from the sensor
	// print the stuff to the computer
	delay(400);
}
```
The *map()* function takes a value to read from, then it takes the current range
of the readings, then it takes the desired range. It's syntax is like this:
```
map(value, fromLow, fromHigh, toLow, toHigh);
```
*fromLow* and *fromHigh* are the low and high of the current range, and *toLow*
and *toHigh* are the low and high of the desired range.

If you want to set the servo to a specific angle you juts have to do:
```
myServo.write(angle)
```
replacing angle with a number between 0 and 180.

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
