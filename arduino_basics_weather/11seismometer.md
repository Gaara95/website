For this segment we will be making a seismometer that will send over it's readings
to a computer.
To measure vibrations we will be using a piezo element, these generate an
electrical charge when stressed, so if they vibrate they will generate an analog signal.

Piezo's can produce huge  voltage spikes when stressed, so to prevent it from frying
the Arduino we will need to use a large resistor to "load down" the piezo.

**Parts:**

* An Arduino.
* A Piezo element.
* A 100k to 1 megaohm resistor.
* A breadboard.

### Wiring

Put the resistor between the pin you are reading from and ground.

Arduino   |   piezo
----------|----------
Analog 0  |   Pin1
GND       |   GND

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/seismograph.png" alt="screen" width="758" height="600" />

### The code

```
/*
 A "seismometer" using a piezo element and an Arduino to detect vibrations.
 This is not scientific or accurate, it just shows if there
 is vibration and how much in raw analog signals.

 the sensor is connected to analog in 0.

 Open the serial plotter in the Arduino IDE for nice graphs.
*/

int val = 0;
int reading = 0; // used to map the readings from sensor to a range of 0 to 100.

void setup() {
	Serial.begin(9600);

	//analog pins are input by default.
}

void loop() {
	val = analogRead(0); // read the sensor value.
	/*
	 val varies a lot, from 0 to 500, so to make the graphs a bit neater
	 and so the vibrations are apparent, we map the readings from a range of
	 0 to 500 to a range of 0 to 100.
	 like so: map(value, fromLow, fromHigh, toLow, toHigh);
	*/
	reading = map(val, 0, 500, 0, 100);

	Serial.println(reading);
	delay(10);
}
```

Since the analog readings from the piezo range from 0 to above 1000 we use the
*map()* function to make the readings range smaller.
The *map()* function takes a value to read from, then it takes the current range
of the readings, then it takes the desired range. It's syntax is like this:
```
map(value, fromLow, fromHigh, toLow, toHigh);
```
*fromLow* and *fromHigh* are the low and high of the current range, and *toLow*
and *toHigh* are the low and high of the desired range.

### Making graphs of the readings

Reading the raw values from the sensor wont make much sense to a person, so to
graph the readings from the sensor we can just use the *Serial plotter* in the
Arduino IDE. To launch it press *Ctrl+Shift+L* or click on *tools > Serial monitor*.


##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
