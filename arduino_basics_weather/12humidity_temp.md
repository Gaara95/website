For our weather station we will need to measure humidity and temperature,
to do that we will be using a DHT11 sensor.

This sensor is not a simple *on/off* or analog sensor, to use it we need to use a
*library*, a library is a bunch of code that makes doing complex things, like talking
to a DHT11, simpler. The library we will use has all the code to read values from the sensor.

In this segment we will read from the DHT11 and send the readings to a computer.

**Parts:**

* An Arduino.
* A DHT11.
* A breadboard.

**Libraries:**

* [DHT-sensor-library](https://github.com/adafruit/DHT-sensor-library)

### Wiring

If you have the 3 pin version of this sensor:

Arduino    |    DHT11
-----------|------------
Pin7       |    signal out
GND        |    GND
PWR        |    PWR

If you have the 4 pin version, you will need a 10k resistor:

* Connect pin 1 (on the left) of the sensor to 5v.
* Connect pin 2 to pin 7 on the Arduino.
* Connect pin4 (on the right) of the sensor to GND.
* Connect a 10k resistor between pin 1(PWR) and pin 2(signal) of the sensor.

**Diagrams:**

3 pin:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/dht11_3.png" alt="dht11_3" width="640" height="600" />

4 pin:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/dht11.png" alt="dht11_4" width="750" height="600" />

### Installing Libraries

To use the library you need it to be in the *libraries* folder of the Arduino IDE,
to get it follow whichever method you prefer:

**From the IDE(the easy way):**

* Open the Arduino IDE.
* Click on sketch > Include library > Manage libraries.
* A new window will open, in here search for DHT11 in the search bar in the top
  right corner, then from the search results click on the *DHT sensor library*
  and click install.

**Direct download(the way I prefer):**

* Go to the GitHub repository for the library: https://github.com/adafruit/DHT-sensor-library
* Click on *clone or download* then click download zip.
* Extract the zip file and put the extracted folder in the *Arduino/libraries* folder.
* **Or** if you have git do: *git clone https://github.com/adafruit/DHT-sensor-library.git*
  in the Arduino/libraries folder.

### The code
```
// reads temperature and humidity from a DHTXX and sends it to a computer over serial.
// this codes repo: https://github.com/afshaan4/other_arduino_projects

#include "DHT.h"

//set the sensor pin
#define DHTPIN 7

//Uncomment whatever type you're using.
#define DHTTYPE DHT11   // DHT 11
//#define DHTTYPE DHT22   // DHT 22  (AM2302), AM2321
//#define DHTTYPE DHT21   // DHT 21 (AM2301)

/*
 Connect pin 1 (on the left) of the sensor to +5V
 NOTE: If using a board with 3.3V logic like an Arduino Due connect pin 1
 to 3.3V instead of 5V!
 Connect pin 2 of the sensor to whatever your DHTPIN is
 Connect pin 4 (on the right) of the sensor to GROUND
 Connect a 10K resistor from pin 2 (data) to pin 1 (power) of the sensor
*/

/*
 Initialize DHT sensor.
 Note that older versions of this library took an optional third parameter to
 tweak the timings for faster processors.  This parameter is no longer needed
 as the current DHT reading algorithm adjusts itself to work on faster processors.
*/
DHT dht(DHTPIN, DHTTYPE);

void setup() {
  dht.begin(); // start the library
  Serial.begin(9600);
}

void loop() {
  //wait a few seconds between measurements.
  delay(2000);
  //reading temperature or humidity takes about 250 milliseconds.
  //sensor readings may also be up to 2 seconds 'old' (its a very slow sensor)
  float h = dht.readHumidity();
  //read temperature as Celsius (the default)
  float t = dht.readTemperature();
  //If you want the temperature as Fahrenheit uncomment the line below (isFahrenheit = true)
  //float f = dht.readTemperature(true);

  //check if any reads failed and exit early (to try again).
  if (isnan(h) || isnan(t)) {
    Serial.println("Failed to read from DHT sensor :(");
    return;
  }

  /*
  print humidity and temperature readings to the screen.
  */

  // humidity goes on one line.
  Serial.print("Humidity: ");
  Serial.print(h);
  Serial.println("%");
  //temperature goes on the next line.
  Serial.print("Temp: ");
  Serial.print(t);
  Serial.println("*C   ");
  //separate each set of readings with a empty line.
  Serial.println("");
}
```

To use a library in our code we need to *include* it, we do this by typing:
```
#include "DHT.h"
```
At the top of our code, this adds the library to our code when we send it to the Arduino.

Then to set the pin the sensor is connected to we do:
```
#define DHTPIN 7
```
We can't use *const int DHTPIN = 7;* because we need to tell the *library* that
the sensor is on pin 7 and to do that we have to use *#define*.

This library can read from the DHT11, DHT21, and DHT22 and since each of these
are different we need to tell the library which sensor it will be reading from:
```
#define DHTTYPE DHT11
```
After this we do:
```
DHT dht(DHTPIN, DHTTYPE);
```
To tell the library that the sensor is connected to *DHTPIN* and that it is the
*DHTTYPE* model, this line also says that we will use *dht* as the variable for
the sensor.

In setup we start the library:
```
dht.begin();
```

Then in *loop()* we read the humidity and temperature from the sensor and store
them in floats:
```
float h = dht.readHumidity();

float t = dht.readTemperature();
```
*dht.readHumidity();* reads the humidity values from the sensor,
and *dht.readTemperature* reads the temperature, this is all we need to do to
get readings from the sensor.
We store the humidity readings in a *float* called h, and the temperature readings
in a *float* called t, floats are used when the value stored is a decimal number,
and humidity and temperature readings are usually decimals.

After we read from the sensor we check if the values from the sensor are numbers,
if they are not numbers then we failed to read from the sensor.
```
if (isnan(h) || isnan(t)) {
  Serial.println("Failed to read from DHT sensor :(");
  return;
}
```
*isnan()* is used to check if the values are numbers it returns *True* when the
values are numbers and *False* when the values are not numbers.
We use the *or* operator: *||* to check if h or t are not numbers that way if
either fails the program tells us.

After we have checked if the sensor works we just send the values to the computer:
```
// humidity goes on one line.
Serial.print("Humidity: ");
Serial.print(h);
Serial.println("%");
//temperature goes on the next line.
Serial.print("Temp: ");
Serial.print(t);
Serial.println("*C   ");
//separate each set of readings with a empty line.
Serial.println("");
```

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
