So far we have learned how to read if a sensor is *on* or *off*, but what if you
want to use a light sensor, which can tell us whether or not there is light, as well as
*how much* light there is, to read such a sensor we use the analog input pins on
an Arduino they're the ones on the lower right of the board, marked *A0* to *A5*,
using the *analogRead()* function we can read the voltage applied to one of these pins.
*analogRead()* returns a number between 0 and 1023 which represents voltages from
0 to 5 volts.

So for this example we'll have an Arduino read values form an LDR, which stands for
light dependent resistor, in the darkness an LDRs resistance is quite high.
When you shine light at it, it's resistance drops proportionally to how much light
shines on it.

And we'll blink an LED at a speed dependent on readings from the sensor.

**Parts:**

* An Arduino.
* An LDR.
* A 10k resistor.
* A breadboard.
* And optionally, an LED and accompanying 1k resistor.

### Wiring

If you are adding an external LED connect it to pin 13.

Arduino   |    LDR
----------|--------------
Analog0   |    pin1
PWR       |    pin2

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/03/analog_read.png" alt="" width="600" height="783" />

### The code
Notice how we don't set the analog-in pin as an input, that is because analog input
pins are set as inputs by default.

```
// blink an LED at a speed based on
// analog input from analog pin 0

const int LED = 13;

int val = 0;

void setup() {
	pinMode(LED, OUTPUT);

	//analog pins are input by default
}

void loop() {
	val = analogRead(0); // read the sensor value

	digitalWrite(LED, HIGH);
	delay(val); // wait, using the reading from val as the wait time
	digitalWrite(LED, LOW);
	delay(val); // wait, using the reading from val as the wait time
}
```

This code is pretty simple, we read the sensor and store the reading in val:

```
val = analogRead(0); // read the sensor value
```

Then we turn on and off the LED, and set the delay time in between on and off
to the reading we get from the sensor:
```
  digitalWrite(LED, HIGH);
  delay(val); // wait, using the reading from val as the wait time
  digitalWrite(LED, LOW);
  delay(val); // wait, using the reading from val as the wait time
}
```
##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
