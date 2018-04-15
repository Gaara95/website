For the weather station project, we will be using a LCD to display all the readings,
the display we will be using is a JHD-162A LCD, which has 2 rows of 16 characters.

For this segment we will make the LCD print out a message.

#### Wiring
The wiring for this screen is complex and can fail, so make sure you use good wires,
and if it doesn't work take apart all your wiring and start from the beginning.
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

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
