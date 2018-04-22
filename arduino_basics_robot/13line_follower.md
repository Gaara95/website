Now lets build a line following robot, it's pretty self explanatory: a robot that
follows lines.

To make a robot follow lines we need to be able to detect lines, we will be using
two infrared sensors, one for each side of the line,
infrared sensors have an LED that emits IR light and a photo-diode that detects
any reflected IR light, if there is a white surface in front of the sensor it
detects the reflected light from the IR LED and if there is a black surface in
front of the sensor the light gets absorbed by the black surface. This way we can
detect black lines on lightly colored surfaces.

The reason we use 2 sensors is: we put two on the robot so they are on either side of
the line:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/motion_alarm.png" alt="motion" width="644" height="588" />

If the line bends then the sensor on that side sees the line:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/motion_alarm.png" alt="motion" width="644" height="588" />

### Line following logic:

The logic we use here is simple: if the sensor on the left sees a line turn left,
if the sensor on the right sees a line turn right, if both sensors see a line stop,
and if neither of the sensors see a line go straight.

**Parts:**

* An Arduino.
* 2 IR sensors.
* An L293D motor driver.
* A robot chassis.
* Two DC motors.
* A castor(used as a third wheel, it can move in any direction).
* Two wheels that fit the motor shafts.
* Two 9v batteries and accompanying battery clips.
* Various screws and bolts to mount all this stuff to the robot chassis.
* A screwdriver.

### Wiring

| Arduino        | IR sensors     |
| :------------- | :------------- |
| pin8           | right sensor   |
| pin9           | left sensor    |
| GND            | GND            |
| PWR            | PWR            |

| Arduino        | L293D          |
| :------------- | :------------- |
| pin2           | Motor a pin1   |
| pin3           | Motor a pin2   |
| pin4           | Motor b pin1   |
| pin5           | Motor b pin2   |
| 5v             | enable a, b    |

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/motion_alarm.png" alt="motion" width="644" height="588" />

### code
```
/*
 A simple line follower using 2 IR sensors


 The logic used here is simple:

 If the left sensor is on the line         > turn left
 If the right sensor is on the line        > turn right
 If both the sensors are on the line       > stop
 If neither of the sensors are on the line > go forward
*/

// the left and right IR sensors
const int rSensor = 8;
const int lSensor = 9;

// the motors
const int  a1 = 2;
const int  a2 = 3;
const int  b1 = 4;
const int  b2 = 5;

// used to store readings from the sensors
int lVal;
int rVal;

void setup() {
	// set the sensors as inputs
	pinMode(rSensor, INPUT);
	pinMode(lSensor, INPUT);

	// set the motors as outputs
	pinMode(a1, OUTPUT);
	pinMode(a2, OUTPUT);
	pinMode(b1, OUTPUT);
	pinMode(b2, OUTPUT);
	// for debugging
	Serial.begin(9600);
}

void loop() {
	rVal = digitalRead(rSensor);
	lVal = digitalRead(lSensor);

	// if both sensors see white, go forward
	if ((rVal == 1) && (lVal == 1)) {
		digitalWrite(a1, HIGH);
		digitalWrite(a2, LOW);
		digitalWrite(b1, HIGH);
		digitalWrite(b2, LOW);
	}

	// if the left sensor is on the line, turn left
	if ((rVal == 1) && (lVal == 0)) {
		digitalWrite(a1, HIGH);
		digitalWrite(a2, LOW);
		digitalWrite(b1, LOW);
		digitalWrite(b2, LOW);
	}

	// if the right sensor is on the line, turn right
	if ((rVal == 0) && (lVal == 1)) {
		digitalWrite(a1, LOW);
		digitalWrite(a2, LOW);
		digitalWrite(b1, HIGH);
		digitalWrite(b2, LOW);
	}

	// if both sensors are on the line, stop
	if ((rVal == 0) && (lVal == 0)) {
		digitalWrite(a1, LOW);
		digitalWrite(a2, LOW);
		digitalWrite(b1, LOW);
		digitalWrite(b2, LOW);
	}
}
```

Using the sensors is simple you just have to use *digitalRead()*, if the sensor
sees a line it sends a 0 and when there is no line it sends a 1.
So all we need to do is read the sensors and use a bunch of *if* statements.

If both sensors see no line go forward:
```
// if both sensors see white, go forward
if ((rVal == 1) && (lVal == 1)) {
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
}
```
If the left one sees a line turn left:
```
// if the left sensor is on the line, turn left
if ((rVal == 1) && (lVal == 0)) {
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
}
```
If the right one sees a line turn right:
```
// if the right sensor is on the line, turn right
if ((rVal == 0) && (lVal == 1)) {
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
}
```
If both see a line stop:
```
// if both sensors are on the line, stop
if ((rVal == 0) && (lVal == 0)) {
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
}
```
This is done since most people put a black "ending line" at the end of the track
when the bot sees it the bot stops.

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
