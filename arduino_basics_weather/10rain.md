We want the weather station we are going to build to be able to tell if it is
raining or not, for that we will use a "rain sensor" this is basically a pad with
metal traces on it, when water falls on it current flows through the metal traces.

The pad with the metal traces is connected to a board which is used to tune the
sensitivity of the sensor and change whether it sends a *HIGH* or *LOW* when it
detects rain, this board has a *AO* pin and a *DO* pin, these are for analog out
and digital out, since we want to see if there is rain or not we connect the DO
pin to the Arduino and use *digitalRead()* to see if there is rain or not.

The sensor:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/" alt="" width="600" height="783" />

**Parts:**

* An Arduino.
* A rain sensor.
* A breadboard(optional).

### Wiring

Arduino    |    rain sensor
-----------|---------------
5v PWR     |    VIN
GND        |    GND
Pin8       |    DO

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/" alt="" width="600" height="783" />

### Code
We basically read from the sensor and then use an if statement to turn an LED on
when there is rain and off when there is'nt.

If your sensor is not sending signals or the signal doesn't change when you expect
it to, turn the potentiometer on it(you will most probably need a screwdriver for this)
to tune the sensor.

```
//reads a rain detector and turns on an LED if it detects rain

int LED = 13;
int rainSensor = 8;
int val; //used to read sensor

void setup() {
	pinMode(LED, OUTPUT);
	pinMode(rainSensor, INPUT);
}

void loop()  {
	val =  digitalRead(rainSensor);

  //the sensor returns HIGH if there is no rain
  //and LOW when there is rain
	if(val == HIGH) {
		digitalWrite(LED, LOW);
	} else {
		digitalWrite(LED, HIGH);
	}
}
```
##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
