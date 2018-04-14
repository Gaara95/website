## Weather Station
This is the final project for this course, we will build a weather station that
measures temperature, humidity, light levels, absolute and sea-level corrected
air pressure, and  altitude.

For this we will be using a *JHD-162A* LCD to display all the measured weather
stuff, this is a monochrome LCD which can display 2 rows of 16 characters at a time.

**Parts:**

* An Arduino.
* A BMP180 barometer.
* A DHT11 temperature/humidity sensor.
* A rain sensor.
* A JHD 162A LCD.
* A 220 ohm resistor.
* A 1k resistor.
* A 10k resistor.

**Libraries:**

* [LiquidCrystal](https://github.com/arduino-libraries/LiquidCrystal)
* [DHT sensor library](https://github.com/adafruit/DHT-sensor-library)
* [BMP180 library](https://github.com/sparkfun/BMP180_Breakout_Arduino_Library)

### Using the screen
Since the screen is a bit complex and has quite a few connections, we will start
by just getting the screen to print a message.

#### Wiring the screen
There are sixteen pins on the screen, start from the pin labeled *1*.

* Pin 1 to GND.
* Pin 2 to 5v.
* Pin 3 to GND through a 1k resisitor, and to 5v through a 10k resistor.
* Pin 4 to Arduino pin 12.
* Pin 5 to GND.
* Pin 6 to Arduino pin 11.
* Pin 11 to Arduino pin 5.
* Pin 12 to Arduino pin 4.
* Pin 13 to Arduino pin 3.
* Pin 14 to Arduino pin 2.
* Pin 15 to 5v through a 220ohm resistor.
* Pin 16 to GND.

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/screen.png" alt="screen" width="600" height="790" />

### The LCD code
Run this to check if it works, the wiring for this LCD is complex and can fail.
If the message isn't printed to the screen check your wiring.

```
// Displays "Hello World" on a JHD 162A LCD

// include the library code
#include <LiquidCrystal.h>

// initialize the library and set the pins the LCD is connected to
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() {
  // set the LCD's number of columns and rows:
  lcd.begin(16, 2);
}

void loop() {
	// Print a message to the LCD.
  lcd.print("hello, world!");
  // set the cursor to column 0, line 1
  // (note: line 1 is the second row, since counting begins with 0):
  lcd.setCursor(0, 1);
  // print another message on the next line
  lcd.print("random message");
}
```

Above the setup function we define the LCDs pins and we create the LiquidCrystal
object called "lcd" here.
```
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);
```

The syntax to print things to the display is similar to *Serial.print()*, we just do:
```
lcd.print("message");
```

And to print a message on the second line we use:
```
lcd.setCursor(1, 0);
```
The screen has 2 lines and 16 columns, so *lcd.setCursor(1, 5);* would send the
cursor to line 2 and column 5 (we count from 0).
