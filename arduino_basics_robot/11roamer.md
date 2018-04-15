Now that you know how to read sensors and control LEDs and motors, lets build a robot.
So the robot we will build is going to be an obstacle avoiding robot, it will drive around
and avoid things in it's way.

We will make it a "car" robot so it is going to move around on wheels,
and to sense it's surroundings we will give it an ultrasonic range-finder.

**Powering the Arduino with a battery**

The processor on the Arduino can only take up-to 5 volts, more than that and it can
burn up, so far we used a USB port connected to a computer to power it and USB ports
deliver 5v power so we were fine, but for the robot we use 9 volt batteries,
so how do we power the Arduino with 9 volt batteries and not burn it? Well to
power it with batteries we can use the *vin* on the Arduino, this pin is connected
to a power regulation circuit, so it can take 7 to 12 volts and convert it to 5 volts
for the processor.

So to power the Arduino with the 9volt, battery plug the positive terminal into *vin*
and the negative terminal into *GND*.

**Parts:**

* An Arduino.
* An HC-SR04 ultrasonic sensor.
* An L293D motor driver board.
* A robot chassis.
* Two DC motors.
* A castor(used as a third wheel, it can move in any direction).
* Two wheels that fit the motor shafts.
* Two 9v batteries and accompanying battery clips.
* Various screws and bolts to mount all this stuff to the robot chassis.
* A screwdriver.

### Building the robot

* Unscrew the big nut on the motors:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly1.jpg" alt="assembly1" width="600" height="536" />

* And send the motors axle side through the mounting hole on the chassis:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly2.jpg" alt="assembly2" width="600" height="612" />

* Then screw on the nut back to the motors:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly3.jpg" alt="assembly3" width="600" height="496" />

* Now slide the wheels onto the motors axles and tighten the screws on the motors
  till the wheels are firmly connected to the motors,
  don't tighten too much or you could ruin the threading on the wheels:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly4.jpg" alt="assembly4" width="600" height="856" />

* Now with screws and nuts mount the castor to the bottom front of the chassis:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly5.jpg" alt="assembly5" width="702" height="600" />

* Put a layer of plastic or paper on the top of the chassis
  then put the Arduino and motor driver on it, the layer of paper is to stop the
  Arduino and motor driver from shorting on the metal chassis:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly6.jpg" alt="assembly6" width="601" height="600" />

* Use screws to mount the ultrasonic sensor to the front of the chassis:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly7.jpg" alt="assembly1" width="746" height="600" />


## Wiring


Arduino     |     Motor driver
------------|-----------------
pin 0       |     Enable motor a
pin 1       |     Motor a pin 1
pin 2       |     Motor a pin 2
pin 3       |     PWR
pin 4       |     GND
pin 5       |     Enable motor b
pin 6       |     Motor b pin 2
pin 7       |     Motor b pin 1

Arduino     |     HC-SR04
------------|-----------------
pin 12      |     Trigger (ping)
pin 11      |     Echo (input)
GND         |     GND
PWR         |     PWR

Motor driver     |     Motors
-----------------|----------------
terminalA1       |     motorA wire 1
terminalA2       |     motorA wire 2
terminalB1       |     motorB wire 1
terminalB2       |     motorB wire 2

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/rover.png" alt="rover" width="600" height="600" />

### The code
```
// an obstacle avoiding robot

long randNumber; // used to store the random number.

// assign the motor controller pins
int ea = 0; //enable a
// motor one
int a1 = 1;
int a2 = 2;
int eb = 5; // enable b
// motor two
int b2 = 6;
int b1 = 7;
int V = 3; // power to the motor controller
int G = 4; // ground to the motor controller
// the ultrasonic sensors pins
const int trigger = 12;
const int echo = 11;

/*
function used to make the robot back up and then turn left
*/
void backLeft() {
  // back up for a second
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
  delay(1000);
  // stop the motors
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(10);
  // turn left for a second
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, HIGH);
  delay(1000);
  // stop the motors
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(10);
}

/*
function used to make the robot back up and then turn right
*/
void backRight() {
  // back up for a second
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
  delay(1000);
  // stop the motors
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(10);
  // turn right for a second
  digitalWrite(a1, LOW);
  digitalWrite(a2, HIGH);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
  delay(1000);
  // stop the motors
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(10);
}

// the setup stuff
void setup() {
  // setup the motor controller pins
  pinMode(ea, OUTPUT);
  pinMode(a1, OUTPUT);
  pinMode(a2, OUTPUT);
  pinMode(V, OUTPUT);
  pinMode(G, INPUT);
  pinMode(eb, OUTPUT);
  pinMode(b2, OUTPUT);
  pinMode(b1, OUTPUT);
  // turn the motor driver on and enable both motors
  digitalWrite(ea, HIGH);
  digitalWrite(eb, HIGH);
  digitalWrite(V, HIGH);
  digitalWrite(G, LOW);
  pinMode(trigger, OUTPUT);
  pinMode(echo, INPUT);
}

void loop() {
  // the ping stuff
  long duration, cm;
  // send a ping
  digitalWrite(trigger, LOW);
  delayMicroseconds(2);
  digitalWrite(trigger, HIGH);
  delayMicroseconds(5);
  digitalWrite(trigger, LOW);

  // listen for a return
  duration = pulseIn(echo, HIGH);
  // convert the time of the ping into cm
  cm = microsecondsToCentimeters(duration);
  // put random in a variable
  randNumber = random(0, 2);

  // the avoidance stuff
  // filter out values less than 1 and greater than 20 cm
  if (cm > 1){
    if (cm < 20){
    // based on randNumber turn left (0) or right(1)
      if (randNumber == 0){
        backLeft();
      }
      if (randNumber == 1){
        backRight();
      }
    } else { // if there isn't anything in front just go straight
      digitalWrite(a1, HIGH);
      digitalWrite(a2, LOW);
      digitalWrite(b1, HIGH);
      digitalWrite(b2, LOW);
      delay(150);
    }
  }
}

/*
function that converts the ping time reading to distance
*/
long microsecondsToCentimeters(long microseconds)
{
  // The speed of sound is 340 m/s or 29 microseconds per centimeter.
  // The ping travels out and back, so to find the distance of the
  // object we take half of the distance traveled.
  return microseconds /29 / 2;
}
```
For this project we want the robot to avoid objects by backing away from them then
turning left or right, and going forward again, to make the Arduino randomly decide
which way to turn after avoiding an object we use the *random()* function to choose
a random number from 0 and 1, and based on that the Arduino turns left(0) or right(1).

*random()* takes two arguments the first one is the lower limit of the random value
and the second one is the upper bound and is excluded from the random range, so
*random(0, 2)* generates a number from 0 to 1 not 0 to 2.

To turn left or right requires quite a bit of code so to keep things neat we put
those instructions in functions.
To do this we make the Arduino drive back for one second then spin one motor
back while the other stays off, turning the robot:
```
void backLeft() {
  // back up for a second
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
  delay(1000);
  // stop the motors
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(10);
  // turn left for a second
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, HIGH);
  delay(1000);
  // stop the motors
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(10);
}
```

Then in the loop we send a ping and store it's duration, this is also where we
generate our random number:
```
void loop() {
  // the ping stuff
  long duration, cm;
  // send a ping
  digitalWrite(trigger, LOW);
  delayMicroseconds(2);
  digitalWrite(trigger, HIGH);
  delayMicroseconds(5);
  digitalWrite(trigger, LOW);

  // listen for a return
  duration = pulseIn(echo, HIGH);
  // convert the time of the ping into cm
  cm = microsecondsToCentimeters(duration);
  // put random in a variable
  randNumber = random(0, 2);
```

This code does the obstacle avoiding:
```
// the avoidance stuff
// filter out values less than 1 and greater than 20 cm
if (cm > 1){
  if (cm < 20){
  // based on randNumber turn left (0) or right(1)
    if (randNumber == 0){
      backLeft();
    }
    if (randNumber == 1){
      backRight();
    }
  } else { // if there isn't anything in front just go straight
    digitalWrite(a1, HIGH);
    digitalWrite(a2, LOW);
    digitalWrite(b1, HIGH);
    digitalWrite(b2, LOW);
    delay(150);
  }
}
```
We use if statements to see if anything is in the way:
```
if (cm > 1){
  if (cm < 20){
```
if there is an obstacle we turn left if randNumber is 0, and right if randNumber is 1:
```
// based on randNumber turn left (0) or right(1)
  if (randNumber == 0){
    backLeft();
  }
  if (randNumber == 1){
    backRight();
  }
```
If there are no obstacles in the way we make the robot drive forward:
```
} else { // if there isn't anything in front just go straight
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
  delay(150);
}
```

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
