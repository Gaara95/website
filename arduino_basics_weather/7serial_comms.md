## Serial communication
So far we have used LEDs and buzzers as output devices, but what if you wanted to
collect and read values from a sensor? For that we can use the USB port on the Arduino
to communicate with a computer.

So we're going to read from a light sensor and send the values to the computer
using serial communication.

**Parts:**

* An Arduino.
* An LDR (light sensor)
* A 10k resistor.
* A breadboard.

### Wiring

Arduino    |    LDR
-----------|------------
Analog0    |    pin1
PWR        |    pin2

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/serial_comms.png" alt="Serial" width="600" height="721" />

### The code
Once you have uploaded this to the board hit the *Serial monitor* button, which
is in the top right corner of the Arduino IDE, this opens a new window where all
the data coming from the Arduino is displayed.

```
// sends values read from analog input 0
// to a computer with serial print.

const int sensor = 0; // the pin for the analog sensor

int val = 0; // used to store the reading from the sensor

void setup() {
	// open the serial port to the computer
	// and set the transmission speed to 9600 baud
	Serial.begin(9600);

	// analog pins are input by default
}

void loop() {
	val = analogRead(sensor); // read the sensor
	Serial.println(val); // send the value read from the sensor to the computer

	delay(100); // wait between each send makes reading through the stuff easier
}
```
In the *void setup()* function we open the serial connection to the computer
and we set the transmission speed to 9600 baud, the baud rate is how many bits
per second are sent over the connection.
```
Serial.begin(9600);
```
Also notice that we don't set the analog input pin as an input, this is because
analog pins are set as inputs by default.

Then in *void loop()* we read the sensor and use *Serial.println()* to send *val*
to the computer, *Serial.println()* sends the value to the computer then makes a new
line that way we have a column of values and not just a line of all the values.
```
Serial.println(val);
```
We add a delay of 100 milliseconds so there is a pause between each time *val* is
sent to the computer, if you remove this the rate at which val is sent will be really
high and flood the serial monitor.

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
