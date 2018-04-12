## Driving motors
Now that you learned how to drive small loads like LEDs and take inputs,
lets drive bigger loads, like motors.

Each pin on the Arduino can output up to 20 milliamps, that is a really small amount of current,
and so if you try to power a motor directly with the pin the Arduinos processor will burn out
and it will most probably catch fire.
To drive bigger loads we will use a motor driver, the L293D specifically, it
allows us to switch on and off motors and is controlled by the Arduino.

**Parts:**

* An Arduino.
* A DC motor.
* An L293D motor driver board.
* A 9v battery and battery clip.

### Wiring

Arduino | Motor driver
------------|-------------------
Pin0 | Enable motor a
Pin1 | Motor a pin 1
Pin2 | Motor a pin 2
Pin3 | PWR
Pin4 | GND

9v battery | Motor driver
------------|------------------
9v | PWR to motors
GND | GND for motors

**Diagram:**

<img class="wp-image-112 size-full aligncenter" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/03/motors.png" alt="" width="600" height="599" />

### The code
The motor driver takes three inputs per motor: *motor enable*, *motor pin1*, and *motor pin2*.
If *motor enable* is HIGH the motor gets power and if it is LOW the motor does not get power.

*motor pin1* and *motor pin2* are used to control the direction the motor spins in:
if *motor pin1* is HIGH and *motor pin2* is LOW the motor spins in one direction;
if *motor pin1* is LOW and *motor pin2* is HIGH the motor spins the other way;
if *motor pin1* and *motor pin2* are both LOW the motor stops moving.
That way we can control which way the motor spins and turn it on and off.

```
//makes a DC motor spin one direction for a second then the other

//assign the motor controller pins
int ea = 0; //enable motor a
int a1 = 1; //motor a pin 1
int a2 = 2; //motor a pin 2
int V = 3; //power to the motor controller
int G = 4; //ground to the motor controller

void setup() {
  //setup the motor controller pins
  pinMode(ea, OUTPUT); //enable motor a is an output
  pinMode(a1, OUTPUT);
  pinMode(a2, OUTPUT);
  pinMode(V, OUTPUT);
  pinMode(G, INPUT);

  digitalWrite(ea, HIGH); //turn on motor a
  //power on the controller board
  digitalWrite(V, HIGH);
  digitalWrite(G, LOW);
}

void loop() {
  //turn on pin 1 and turn off pin 2
  //the motor spins one way
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  delay(1000); //let it spin for a second
  //turn off pin 1 and turn on pin 2
  //the motor spins the other way
  digitalWrite(a1, LOW);
  digitalWrite(a2, HIGH);
  delay(1000); //let it spin for a second
}
```
How does this work?

We enable one motor and set *motor pin1* and *motor pin2* as outputs, and use
pins 3 and 4 on the Arduino to power the motor driver, not the motors, since the
L293D chip only needs 20 milliamps to function:
```
void setup() {
  //setup the motor controller pins
  pinMode(ea, OUTPUT); //enable motor a is an output
  pinMode(a1, OUTPUT);
  pinMode(a2, OUTPUT);
  pinMode(V, OUTPUT);
  pinMode(G, INPUT);

  digitalWrite(ea, HIGH); //turn on motor a
  //power on the controller board
  digitalWrite(V, HIGH);
  digitalWrite(G, LOW);
}
```
Then in the loop we just spin the motor one way for a second, then the other
way for a second:
```
void loop() {
  //turn on pin 1 and turn off pin 2
  //the motor spins one way
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  delay(1000); //let it spin for a second
  //turn off pin 1 and turn on pin 2
  //the motor spins the other way
  digitalWrite(a1, LOW);
  digitalWrite(a2, HIGH);
  delay(1000); //let it spin for a second
}
```
##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
