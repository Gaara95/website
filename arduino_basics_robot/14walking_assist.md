For this segment we will be building a device that beeps a buzzer when something
comes near it, it is going to be designed to be used by people who can't see.
You can have the device designed however you want, it can be a stick, a cap, or
whatever else.

I made mine cap mounted: it has 3 ultrasonic sensors one facing forward one to
the left and the other to the right, and 3 buzzers one in front one on the back
and one on the right, this way the user can tell where the obstacle is.

**Parts:**

* An Arduino.
* 3 ultrasonic sensors.
* 3 buzzers.
* A whole lot of wires.

### Wiring

| Arduino        | ultrasonic sensors |
| :------------- | :-------------     |
| pin12          | Trigger sensor 1   |
| pin11          | Echo sensor 1      |
| pin10          | Trigger sensor 2   |
| pin9           | Echo sensor 2      |
| pin8           | Trigger sensor 3   |
| pin7           | Echo sensor 3      |
| GND            | GND                |
| 5v             | PWR                |

| Arduino        | Buzzers        |
| :------------- | :------------- |
| pin5           | buzzer1        |
| pin3           | buzzer2        |
| pin7           | buzzer3        |
| GND            | GND            |
| 5v             | PWR            |

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/walking_assist.png" alt="motion" width="600" height="600" />

### code
```
/*
 A cap that beeps buzzers or provides haptic feedback based on how far objects are from it.
 It uses three ultrasonic rangefinders on the rim of the cap to figure out how far stuff is from it.
 This codes github repo: https://github.com/afshaan4/other_arduino_projects
*/

// Right rangefinder
const int rTrig = 12;
const int rEcho = 11;

// Middle rangefinder
const int mTrig = 10;
const int mEcho = 9;

// Left rangefinder
const int lTrig = 8;
const int lEcho = 6;

// The feedback devices (buzzers or vibration motors)
const int rBuzz = 5;
const int mBuzz = 3;
const int lBuzz = 7;

void setup() {
	// set the feedback devices as outputs
	pinMode(rBuzz, OUTPUT);
	pinMode(mBuzz, OUTPUT);
	pinMode(lBuzz, OUTPUT);
	// rangefinder stuff
	pinMode(rTrig, OUTPUT);
	pinMode(mTrig, OUTPUT);
	pinMode(lTrig, OUTPUT);
	pinMode(rEcho, INPUT);
	pinMode(mEcho, INPUT);
	pinMode(lEcho, INPUT);

	// for debugging
	Serial.begin(9600);
}

void loop() {

  long rDuration, rCm; // used to calculate distance for right sensor
  long mDuration, mCm; // used to calculate distance for middle sensor
  long lDuration, lCm; // used to calculate distance for left sensor

  /*
   send a ping on the right sensor
  */
  digitalWrite(rTrig, LOW); // pull it low for a clean ping
  delayMicroseconds(2);
  digitalWrite(rTrig, HIGH); // send the ping
  delayMicroseconds(5);
  digitalWrite(rTrig, LOW); // pull it low again

  // store the time it takes for the ping to return
  rDuration = pulseIn(rEcho, HIGH);
  // convert the time it takes into centimeters
  rCm = microsecondsToCentimeters(rDuration);

 /*
   send a ping on the left sensor
  */
  digitalWrite(lTrig, LOW); // pull it low for a clean ping
  delayMicroseconds(2);
  digitalWrite(lTrig, HIGH); // send the ping
  delayMicroseconds(5);
  digitalWrite(lTrig, LOW); // pull it low again

  // store the time it takes for the ping to return
  lDuration = pulseIn(lEcho, HIGH);
  // convert the time it takes into centimeters
  lCm = microsecondsToCentimeters(lDuration);

  /*
   send a ping on the middle sensor
  */
  digitalWrite(mTrig, LOW); // pull it low for a clean ping
  delayMicroseconds(2);
  digitalWrite(mTrig, HIGH); // send the ping
  delayMicroseconds(5);
  digitalWrite(mTrig, LOW); // pull it low again

  // store the time it takes for the ping to return
  mDuration = pulseIn(mEcho, HIGH);
  // convert the time it takes into centimeters
  mCm = microsecondsToCentimeters(mDuration);

  /*
   turn on the right buzzer if it sees anything
  */
  if ((rCm > 1) and (rCm < 100)) { // ignore stuff closer than 1 cm and further than 100 cm.
    digitalWrite(rBuzz, HIGH);
    delay(rCm * 10);
    digitalWrite(rBuzz, LOW);
    delay(rCm * 10);
    Serial.print(rCm);
    Serial.println("rCm");
  }

  /*
   turn on the left buzzer if it sees anything
  */
  if ((lCm > 1) and (lCm < 100)) { // ignore stuff closer than 1 cm and further than 100 cm.
    digitalWrite(lBuzz, HIGH);
    delay(lCm * 10);
    digitalWrite(lBuzz, LOW);
    delay(lCm * 10);
    Serial.print(lCm);
    Serial.println("lCm");
  }  

  /*
   turn on the middle buzzer if it sees anything
  */
  if ((mCm > 1) and (mCm < 100)) { // ignore stuff closer than 1 cm and further than 100 cm.
    digitalWrite(mBuzz, HIGH);
    delay(mCm * 10);
    digitalWrite(mBuzz, LOW);
    delay(mCm * 10);
    Serial.print(mCm);
    Serial.println("mCm");
  }
}


/*
 function that converts the ping time reading to distance
*/
long microsecondsToCentimeters(long microseconds) {
  // The speed of sound is 340 m/s or 29 microseconds per centimeter.
  // The ping travels out and back, so to find the distance of the
  // object we take half of the distance traveled.
  return microseconds /29 / 2;
}
```

Since I put three rangefinder on the cap we declare them as the left right and
middle rangefinders, there are also three feedback devices -one for each sensor:
```
// Right rangefinder
const int rTrig = 12;
const int rEcho = 11;

// Middle rangefinder
const int mTrig = 10;
const int mEcho = 9;

// Left rangefinder
const int lTrig = 8;
const int lEcho = 6;

// The feedback devices (buzzers or vibration motors)
const int rBuzz = 5;
const int mBuzz = 3;
const int lBuzz = 7;
```

Since we have 3 sensors we need 3 sets of variables to store the readings from each:
```
long rDuration, rCm; // used to calculate distance for right sensor
long mDuration, mCm; // used to calculate distance for middle sensor
long lDuration, lCm; // used to calculate distance for left sensor
```

We also need to send a ping and measure duration 3 times one for each sensor:
```
/*
 send a ping on the right sensor
*/
digitalWrite(rTrig, LOW); // pull it low for a clean ping
delayMicroseconds(2);
digitalWrite(rTrig, HIGH); // send the ping
delayMicroseconds(5);
digitalWrite(rTrig, LOW); // pull it low again

// store the time it takes for the ping to return
rDuration = pulseIn(rEcho, HIGH);
// convert the time it takes into centimeters
rCm = microsecondsToCentimeters(rDuration);

/*
 send a ping on the left sensor
*/
digitalWrite(lTrig, LOW); // pull it low for a clean ping
delayMicroseconds(2);
digitalWrite(lTrig, HIGH); // send the ping
delayMicroseconds(5);
digitalWrite(lTrig, LOW); // pull it low again

// store the time it takes for the ping to return
lDuration = pulseIn(lEcho, HIGH);
// convert the time it takes into centimeters
lCm = microsecondsToCentimeters(lDuration);

/*
 send a ping on the middle sensor
*/
digitalWrite(mTrig, LOW); // pull it low for a clean ping
delayMicroseconds(2);
digitalWrite(mTrig, HIGH); // send the ping
delayMicroseconds(5);
digitalWrite(mTrig, LOW); // pull it low again

// store the time it takes for the ping to return
mDuration = pulseIn(mEcho, HIGH);
// convert the time it takes into centimeters
mCm = microsecondsToCentimeters(mDuration);
```
Notice though, that we don't have 3 separate functions for converting *duration*
into centimeters, that's because we can use the same function for each of the sensors.

After we send the pings we have 3 *if* statements that check if something is in the
way of the sensors and beeps the corresponding buzzer, the closer the obstacle gets
the faster the beeping gets:
```
/*
 turn on the right buzzer if it sees anything
*/
if ((rCm > 1) and (rCm < 100)) { // ignore stuff closer than 1 cm and further than 100 cm.
  digitalWrite(rBuzz, HIGH);
  delay(rCm * 10);
  digitalWrite(rBuzz, LOW);
  delay(rCm * 10);
  Serial.print(rCm);
  Serial.println("rCm");
}

/*
 turn on the left buzzer if it sees anything
*/
if ((lCm > 1) and (lCm < 100)) { // ignore stuff closer than 1 cm and further than 100 cm.
  digitalWrite(lBuzz, HIGH);
  delay(lCm * 10);
  digitalWrite(lBuzz, LOW);
  delay(lCm * 10);
  Serial.print(lCm);
  Serial.println("lCm");
}  

/*
 turn on the middle buzzer if it sees anything
*/
if ((mCm > 1) and (mCm < 100)) { // ignore stuff closer than 1 cm and further than 100 cm.
  digitalWrite(mBuzz, HIGH);
  delay(mCm * 10);
  digitalWrite(mBuzz, LOW);
  delay(mCm * 10);
  Serial.print(mCm);
  Serial.println("mCm");
}
```

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
