This project is a simple balancing game, it has 6 LEDs to display your "progress"
and an accelerometer to measure tilt.
You put the circuit on a plank then put the plank on a tube and try to balance
on the plank, the longer you stay balanced more LEDs turn on, and when all the LEDs
turn on the Arduino flashed all the LEDs on and off in a "celebration" animation.
If you lose balance however the Arduino resets you progress, turns off all the LEDs
and starts the game from the beginning.

This project is based on a thing I saw on twitter: https://twitter.com/morrill_rob/status/993850732485918720?s=09


**Parts:**
* An Arduino.
* An MPU 6050 accelerometer/gyroscope.
* 6 LEDs.
* 6 1k resistors (you can use anything from a 220omh to a 1k).
* A breadboard.


### Wiring

The MPU 6050 sends readings over *I2C*, I2C is a communication protocol that is used
by micro-controllers and sensors. I2C has two pins *SDA* and *SCL* which are *serial data*
and *serial clock*

On different Arduino boards the *SDA* and *SCL* pins are different, use these charts
to see which pins to connect to:

MPU 6050     |    Arduino uno
-------------|---------------
SDA          |    SDA
SCL          |    SCL
GND          |    GND
VIN          |    5v

MPU 6050     |    Arduino mega
-------------|---------------
SDA          |    pin 20, or any pin labeled SDA
SCL          |    pin 21, or any pin labeled SCL
GND          |    GND
VIN          |    5v

MPU 6050     |    Arduino pro micro
-------------|---------------
SDA          |    pin 2, or any pin labeled SDA
SCL          |    pin 3, or any pin labeled SCL
GND          |    GND
VIN          |    5v

**Diagram for the Arduino UNO:**

<img class="aligncenter wp-image-110 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/05/balancing_game_diagram.png" alt="" width="922" height="600" />


### The code
```
#include<Wire.h>

// the LEDs
const int led1 = 2;
const int led2 = 3;
const int led3 = 4;
const int led4 = 5;
const int led5 = 6;
const int led6 = 7;

const int MPU_addr=0x68;  // I2C address of the MPU-6050
int16_t AcY; // used to store the accelerometer Y axis readings.

int state = 1; // used to store the state of the sensor, 1 = tilted, 0 = balanced.
int counter = led1; // used to light up the LEDs


void setup() {
  // power on and wake up the sensor
  Wire.begin();
  Wire.beginTransmission(MPU_addr);
  Wire.write(0x6B);  // PWR_MGMT_1 register
  Wire.write(0);     // set to zero (wakes up the MPU-6050)
  Wire.endTransmission(true);

  Serial.begin(9600);
}


void loop() {
	// request readings from the sensor
  Wire.beginTransmission(MPU_addr);
  Wire.write(0x3D);  // starting with register 0x3D (ACCEL_YOUT_H)
  Wire.endTransmission(false);
  Wire.requestFrom(MPU_addr,14,true);  // request a total of 14 registers
  // read the Y axis value
  AcY=Wire.read()<<8|Wire.read();  // 0x3D (ACCEL_YOUT_H) & 0x3E (ACCEL_YOUT_L)

  // for debugging
  Serial.print("AcY = "); Serial.println(AcY);


  /*
   check if the sensor is balanced
   if it is set state to 0 if it isn't
   set state to 1 and turn off all the LEDs
  */
  if (AcY <= 1500 && AcY >= -1500) {
    state = 0;// set "state" to 0
    Serial.println("balanced");
  } else {
    // set state to 1
    Serial.println("tilted");
    state = 1;
    // reset counter
    counter = led1;
    // turn off all the LEDs
    digitalWrite(led1, LOW);
    digitalWrite(led2, LOW);
    digitalWrite(led3, LOW);
    digitalWrite(led4, LOW);
    digitalWrite(led5, LOW);
    digitalWrite(led6, LOW);
  }


  /*
   while the sensor is balanced,
   turn on the LEDs one after the other
  */
  while(state == 0) {
    digitalWrite(counter, HIGH);
    delay(1000);
    counter ++;
    break; // break out of the loop so we can read the accelerometer
  }

  // if all the LEDs have been turned on
  if (counter > (led6 + 1)) {
    // reset counter
    counter = led1;
    // set state to 1
    state = 1;
    // play the "you win" animation
    digitalWrite(led1, LOW);
    digitalWrite(led2, LOW);
    digitalWrite(led3, LOW);
    digitalWrite(led4, LOW);
    digitalWrite(led5, LOW);
    digitalWrite(led6, LOW);
    delay(300);
    digitalWrite(led1, HIGH);
    digitalWrite(led2, HIGH);
    digitalWrite(led3, HIGH);
    digitalWrite(led4, HIGH);
    digitalWrite(led5, HIGH);
    digitalWrite(led6, HIGH);
    delay(300);
    digitalWrite(led1, LOW);
    digitalWrite(led2, LOW);
    digitalWrite(led3, LOW);
    digitalWrite(led4, LOW);
    digitalWrite(led5, LOW);
    digitalWrite(led6, LOW);
    delay(300);
    digitalWrite(led1, HIGH);
    digitalWrite(led2, HIGH);
    digitalWrite(led3, HIGH);
    digitalWrite(led4, HIGH);
    digitalWrite(led5, HIGH);
    digitalWrite(led6, HIGH);
    delay(300);
    digitalWrite(led1, LOW);
    digitalWrite(led2, LOW);
    digitalWrite(led3, LOW);
    digitalWrite(led4, LOW);
    digitalWrite(led5, LOW);
    digitalWrite(led6, LOW);
  }
}
```

**The logic used in the code:**
*counter* is set to the pin of the first LED, it is used to to loop through the
LEDs and turn them on.
*state* is initialized as 1, 1 = tilted, 0 = balanced, so we start off assuming that
the accelerometer is tilted.

* If the readings from the accelerometer stay within the threshold set in the code
  *state* is set to 0. I set my threshold as 1500 to -1500.

* While *state* is 0, do *digitalWrite(counter, HIGH)* and increment counter
  with a 1 second interval between increments.

* If counter reaches the pin of the last LED + 1 which in my case is 8, flash all
  the LEDs on and off in "celebration". *counter* is reset to the pin of the first LED
  and *state* is set to 1, and so the game is started from the beginning.

* If the readings from the accelerometer go above or below the set threshold
  *state* is set to 1 and *counter* is set to the pin of the first LED,
  and so the game is started from the beginning.


**Communicating with the sensor:**
Since this is an I2C sensor, we use the *wire* library to talk to it, if you want
to read more about I2C read this article by Sparkfun: https://learn.sparkfun.com/tutorials/i2c
```
#include<Wire.h>
```

Different I2C devices have a different address, this is done so you can have multiple
different I2C sensors or whatever attached to the Arduino at the same time.
So we have to tell our Arduino what adress the MPU 6050 is at:
```
const int MPU_addr=0x68;  // I2C address of the MPU-6050
```

Once we tell the Arduino what address the sensor is at, we have to "wake up" the
sensor, we do this in the setup function.
```
void setup() {
  // power on and wake up the sensor
  Wire.begin();
  Wire.beginTransmission(MPU_addr);
  Wire.write(0x6B);  // PWR_MGMT_1 register
  Wire.write(0);     // set to zero (wakes up the MPU-6050)
  Wire.endTransmission(true);

  Serial.begin(9600);
}
```

Then at the top of the main loop we read from the sensor, now the value we want
from the sensor is the acceleration along its Y axis and according to the datasheet
of the sensor that is stored at register *0x3D*, so we start reading from there
```
Wire.write(0x3D);  // starting with register 0x3D (ACCEL_YOUT_H)
```
and request the data we want
```
Wire.requestFrom(MPU_addr,14,true);
```
Then we store the readings in the variable *AcY*
```
AcY=Wire.read()<<8|Wire.read();
```
The whole block of code that reads the sensor:
```
void loop() {
	// request readings from the sensor
  Wire.beginTransmission(MPU_addr);
  Wire.write(0x3D);  // starting with register 0x3D (ACCEL_YOUT_H)
  Wire.endTransmission(false);
  Wire.requestFrom(MPU_addr,14,true);  // request a total of 14 registers
  // read the Y axis value
  AcY=Wire.read()<<8|Wire.read();  // 0x3D (ACCEL_YOUT_H) & 0x3E (ACCEL_YOUT_L)
```

The rest of the code is pretty simple and well enough commented that you should
be able to figure out how it works and what it does pretty easily.

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found here: http://mit-license.org/
