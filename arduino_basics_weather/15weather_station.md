This is the final project for this course, we will build a weather station that
measures temperature, humidity, light levels, absolute and sea-level corrected
air pressure, and altitude.
We will display the readings on the LCD used in the previous segment.

**Parts:**

* An Arduino.
* A BMP180 barometer.
* A DHT11 temperature/humidity sensor.
* A rain sensor.
* A JHD 162A LCD.
* An LDR.
* A 220 ohm resistor.
* A 1k resistor.
* A 10k resistor.

**Libraries:**

* [LiquidCrystal](https://github.com/arduino-libraries/LiquidCrystal)
* [DHT sensor library](https://github.com/adafruit/DHT-sensor-library)
* [BMP180 library](https://github.com/sparkfun/BMP180_Breakout_Arduino_Library)

### Wiring up the sensors
I wont go over wiring the screen over here as well, just use the previous segment
as a reference to wire the screen.

If you have a 4 pin version of the DHT11:

* Connect pin 1 (on the left) of the sensor to 5v.
* Connect pin 2 to pin 7 on the Arduino.
* Connect pin4 (on the right) of the sensor to GND.
* Connect a 10k resistor between pin 1(PWR) and pin 2(signal) of the sensor.

If you have a 3 pin DHT11:

Arduino    |    DHT11
-----------|-------------
Pin7       |    OUT
GND        |    GND
5v         |    PWR

**DHT11 diagrams:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/dht11_3.png" alt="dht11_3" width="640" height="600" />

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/dht11.png" alt="dht11_4" width="750" height="600" />

**Warning:** the BMP180 is a 3volt sensor do not power it with 5volts.
On different Arduinos the SDA and SCL pins are different so look up which
pins the SCL and SDA pins are for your specific Arduino.

Arduino    |    BMP180
-----------|-------------
SDA        |    SDA
SCL        |    SCL
GND        |    GND
3v3        |    PWR

Arduino    |    Rain sensor
-----------|----------------
Pin8       |    DO
GND        |    GND
PWR        |    PWR

**BMP180 diagram:**


##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
