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

**DHT11:**
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

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/dht11_3.png" alt="dht11_3" width="640" height="600" />

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/dht11.png" alt="dht11_4" width="750" height="600" />

**BMP180:**
Warning! The BMP180 is a 3 volt sensor do not power it with 5volts.
On different Arduinos the SDA and SCL pins are different so look up which
pins the SCL and SDA pins are for your specific Arduino.

Arduino    |    BMP180
-----------|-------------
SDA        |    SDA
SCL        |    SCL
GND        |    GND
3v3        |    PWR

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/BMP180.png" alt="dht11_4" width="597" height="640" />

**Rain sensor:**

Arduino    |    Rain sensor
-----------|----------------
Pin8       |    DO
GND        |    GND
PWR        |    PWR


### The code
```
/*
  An Arduino weather station, using:
  * A BMP180 barometer.
  * A DHT11 temperature and humidity sensor.
  * An LDR.
  * A "rain sensor".
  * A 16x2 LCD.

  The sensor readings are sent to the LCD
  and debugging stuff is sent to a computer over serial

  For wiring diagrams and stuff visit:
  this codes repo: https://github.com/afshaan4/other_arduino_projects
*/

#include "DHT.h"
#include <LiquidCrystal.h>
#include <SFE_BMP180.h>
#include <Wire.h>

const int rain = 8;
const int light = A0;

int rval; // used to store readings from the rain sensor
int lval; // used to store readings from the LDR

// set the sensor and LCD pins
#define DHTPIN 7
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// create an SFE_BMP180 object, here called "pressure":
SFE_BMP180 pressure;
#define ALTITUDE 920.0 // set your altitude here(in meters)

// Uncomment whatever type you're using.
#define DHTTYPE DHT11   // DHT 11
//#define DHTTYPE DHT22   // DHT 22  (AM2302), AM2321
//#define DHTTYPE DHT21   // DHT 21 (AM2301)

/*
 Initialize DHT sensor.
 Note that older versions of this library took an optional third parameter to
 tweak the timings for faster processors.  This parameter is no longer needed
 as the current DHT reading algorithm adjusts itself to work on faster processors.
*/
DHT dht(DHTPIN, DHTTYPE);

void setup() {
  lcd.begin(16, 2);
  dht.begin();
  Serial.begin(9600);

  // Initialize the BMP180 sensor (it is important to get calibration values stored on the device).
  if (pressure.begin()) {
    Serial.println("BMP180 init success");
  } else {
    // Oops, something went wrong, this is usually a connection problem,
    // see the comments at the top of this sketch for the proper connections.
    Serial.println("BMP180 init fail\n\n");
    while(1); // Pause forever.
  }
}

void loop() {

  // variables for the BMP180
  char status;
  double T,P,p0,a; // temperature, pressure, sea-level compensated pressure, computed altitude.

  // read the rain and light sensors
  lval = analogRead(light);
  rval = digitalRead(rain);

  // reading temperature or humidity takes about 250 milliseconds!
  // sensor readings may also be up to 2 seconds 'old' (its a very slow sensor)
  float h = dht.readHumidity();
  // read temperature as Celsius (the default)
  float t = dht.readTemperature();
  // If you want the temperature as Fahrenheit (isFahrenheit = true)
  // float f = dht.readTemperature(true);

  // check if any reads failed and exit early (to try again).
  if (isnan(h) || isnan(t)) {
    Serial.println("Failed to read from DHT sensor :(");
    return;
  }

  // If you want sea-level-compensated pressure, as used in weather reports,
  // you will need to know the altitude at which your measurements are taken.
  // We're using a constant called ALTITUDE in this sketch:

  lcd.print("given alt: ");
  lcd.print(ALTITUDE,0);
  lcd.print("m");
  lcd.setCursor(0, 1);

  // You must first get a temperature measurement to perform a pressure reading.

  // Start a temperature measurement:
  // If request is successful, the number of ms to wait is returned.
  // If request is unsuccessful, 0 is returned.

  status = pressure.startTemperature();
  if (status != 0)
  {
    // Wait for the measurement to complete:
    delay(status);

    // Retrieve the completed temperature measurement:
    // Note that the measurement is stored in the variable T.
    // Function returns 1 if successful, 0 if failure.

    status = pressure.getTemperature(T);
    if (status != 0)
    {
      // Start a pressure measurement:
      // The parameter is the oversampling setting, from 0 to 3 (highest res, longest wait).
      // If request is successful, the number of ms to wait is returned.
      // If request is unsuccessful, 0 is returned.

      status = pressure.startPressure(3);
      if (status != 0)
      {
        // Wait for the measurement to complete:
        delay(status);

        // Retrieve the completed pressure measurement:
        // Note that the measurement is stored in the variable P.
        // Note also that the function requires the previous temperature measurement (T).
        // (If temperature is stable, you can do one temperature measurement for a number of pressure measurements.)
        // Function returns 1 if successful, 0 if failure.

        status = pressure.getPressure(P,T);
        if (status != 0)
        {
          // Print out the measurement:
          lcd.print("ab Pr: ");
          lcd.print(P,2);
          lcd.print("mb");
          delay(3000);
          lcd.clear();

          // The pressure sensor returns absolute pressure, which varies with altitude.
          // To remove the effects of altitude, use the sealevel function and your current altitude.
          // This number is commonly used in weather reports.
          // Parameters: P = absolute pressure in mb, ALTITUDE = current altitude in m.
          // Result: p0 = sea-level compensated pressure in mb

          p0 = pressure.sealevel(P,ALTITUDE); // we're at 920 meters.
          lcd.print("rel Pr ");
          lcd.print(p0,2);
          lcd.print("mb");
          delay(3000);
          lcd.clear();

        }
        // these are for debugging so we send them to the computer.
        else Serial.println("error retrieving pressure measurement\n");
      }
      else Serial.println("error starting pressure measurement\n");
    }
    else Serial.println("error retrieving temperature measurement\n");
  }
  else Serial.println("error starting temperature measurement\n");


  // print humidity and temperature readings to the screen.
  lcd.print("Humidity: ");
  lcd.print(h);
  lcd.print("%");
  lcd.setCursor(0, 1);
  lcd.print("Temp: ");
  lcd.print(t);
  lcd.print("*C   ");
  lcd.setCursor(1, 1);
  delay(3000);
  lcd.clear();

  lcd.print("Light: ");
  lcd.print(lval);
  lcd.setCursor(1, 1);

  if(rval == HIGH) {
    lcd.print("No rain");
    delay(3000);
    lcd.clear();
  } else {
    lcd.print("Raining");
    delay(3000);
    lcd.clear();
  }
}
```
This project has 4 sensors reading data and the screen we used only has 2 lines
of 16 characters to display all that, clearly all the readings wont fit on the screen at once,
so each set of readings is made into a "page" i.e. we just display them one after the other
with a delay in between to set how long they stay on the screen(in our case 3 seconds).
And after the 3 second delay we use *lcd.clear()* to remove previous readings from the screen.

At the top we set all the sensor pins and variables, we also set our current altitude
for the pressure readings(this is used to calculate sea-level compensated pressure):
```
#include "DHT.h"
#include <LiquidCrystal.h>
#include <SFE_BMP180.h>
#include <Wire.h>

const int rain = 8;
const int light = A0;

int rval; // used to store readings from the rain sensor
int lval; // used to store readings from the LDR

// set the sensor and LCD pins
#define DHTPIN 7
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// create an SFE_BMP180 object, here called "pressure":
SFE_BMP180 pressure;
#define ALTITUDE 920.0 // set your altitude here(in meters)

// Uncomment whatever type you're using.
#define DHTTYPE DHT11   // DHT 11
//#define DHTTYPE DHT22   // DHT 22  (AM2302), AM2321
//#define DHT
```

Then in *setup()* we set the LCD's cursor position, initialize the DHT11
and BMP180, and check if the BMP180 worked:
```
void setup() {
  lcd.begin(16, 2);
  dht.begin();
  Serial.begin(9600);

  // Initialize the BMP180 sensor (it is important to get calibration values stored on the device).
  if (pressure.begin()) {
    Serial.println("BMP180 init success");
  } else {
    // Oops, something went wrong, this is usually a connection problem,
    // see the comments at the top of this sketch for the proper connections.
    Serial.println("BMP180 init fail\n\n");
    while(1); // Pause forever.
  }
}
```

At the top of *loop()* we make the variables for reading the BMP180 and read the
rain, light, and humidity/temperature sensors, then we check if the DHT11 worked:
```
char status;
double T,P,p0,a; // temperature, pressure, sea-level compensated pressure, computed altitude.

// read the rain and light sensors
lval = analogRead(light);
rval = digitalRead(rain);

// reading temperature or humidity takes about 250 milliseconds!
// sensor readings may also be up to 2 seconds 'old' (its a very slow sensor)
float h = dht.readHumidity();
// read temperature as Celsius (the default)
float t = dht.readTemperature();


// check if any reads failed and exit early (to try again).
if (isnan(h) || isnan(t)) {
  Serial.println("Failed to read from DHT sensor :(");
  return;
}
```

After that we print the provided altitude and set the cursor to the next line:
```
lcd.print("given alt: ");
lcd.print(ALTITUDE,0);
lcd.print("m");
lcd.setCursor(0, 1);
```

We take a temperature reading followed by the absolute pressure reading,
then we print the absolute pressure reading to the LCD under the altitude
(the cursor was set to the next line after we printed the altitude), then after
3 seconds we clear the screen so the next readings can be printed to it.

After that we calculate the sea-level corrected pressure and print it to the screen:
```
status = pressure.startTemperature();
if (status != 0)
{
  // Wait for the measurement to complete:
  delay(status);

  // Retrieve the completed temperature measurement:
  // Note that the measurement is stored in the variable T.
  // Function returns 1 if successful, 0 if failure.

  status = pressure.getTemperature(T);
  if (status != 0)
  {
    // Start a pressure measurement:
    // The parameter is the oversampling setting, from 0 to 3 (highest res, longest wait).
    // If request is successful, the number of ms to wait is returned.
    // If request is unsuccessful, 0 is returned.

    status = pressure.startPressure(3);
    if (status != 0)
    {
      // Wait for the measurement to complete:
      delay(status);

      // Retrieve the completed pressure measurement:
      // Note that the measurement is stored in the variable P.
      // Note also that the function requires the previous temperature measurement (T).
      // (If temperature is stable, you can do one temperature measurement for a number of pressure measurements.)
      // Function returns 1 if successful, 0 if failure.

      status = pressure.getPressure(P,T);
      if (status != 0)
      {
        // Print out the measurement:
        lcd.print("ab Pr: ");
        lcd.print(P,2);
        lcd.print("mb");
        delay(3000);
        lcd.clear();

        // The pressure sensor returns absolute pressure, which varies with altitude.
        // To remove the effects of altitude, use the sealevel function and your current altitude.
        // This number is commonly used in weather reports.
        // Parameters: P = absolute pressure in mb, ALTITUDE = current altitude in m.
        // Result: p0 = sea-level compensated pressure in mb

        p0 = pressure.sealevel(P,ALTITUDE); // we're at 920 meters.
        lcd.print("rel Pr ");
        lcd.print(p0,2);
        lcd.print("mb");
        delay(3000);
        lcd.clear();

      }
```

All the pressure stuff is in nested *if* statements and if any of them fails it
exits to one of these *else* statements:
```
      // these are for debugging so we send them to the computer.
      else Serial.println("error retrieving pressure measurement\n");
    }
    else Serial.println("error starting pressure measurement\n");
  }
  else Serial.println("error retrieving temperature measurement\n");
}
else Serial.println("error starting temperature measurement\n");
```
These print out debugging messages to a computer so you can tell exactly what failed.

After all that we print the temperature and humidity readings to the screen:
```
// print humidity and temperature readings to the screen.
lcd.print("Humidity: ");
lcd.print(h);
lcd.print("%");
lcd.setCursor(0, 1);
lcd.print("Temp: ");
lcd.print(t);
lcd.print("*C   ");
lcd.setCursor(1, 1);
delay(3000);
lcd.clear();
```
Followed by the light intensity reading and a message telling us whether or not it's raining:
```
lcd.print("Light: ");
lcd.print(lval);
lcd.setCursor(1, 1);

if(rval == HIGH) {
  lcd.print("No rain");
  delay(3000);
  lcd.clear();
} else {
  lcd.print("Raining");
  delay(3000);
  lcd.clear();
}
```

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
