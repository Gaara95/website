
Now that you learned to control one LED let's try controlling ten.
For this project we will connect 10 LED's to the Arduino and light them up in a sequence, making a wave.

**Parts:**

* An Arduino.
* 10 LED's.
* 10 1k resistors.
* A breadboard.

### Wiring

Connect all the Ground pins of the LED's to one of the ground rails on the
breadboard through 1k resistors.
Then connect the Power pins on the LED's to Arduino pins 2 to 11.

Arduino | LED's
-------------|-------------
Pin 2 | LED 1
Pin 3 | LED 2
Pin 4 | LED 3
Pin 5 | LED 4
Pin 6 | LED 5
Pin 7 | LED 6
Pin 8 | LED 7
Pin 9 | LED 8
Pin 10 | LED 9
Pin 11 | LED 10
GND | GND

**Diagram:**

<img class="aligncenter wp-image-89 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/03/led_wave.png" alt="Led wave" width="600" height="528" />

Led wave

### The code

You can copy the code below, or download it from [here](https://github.com/afshaan4/other_arduino_projects/blob/master/ledwave/ledwave.ino)
```
// turns on and then off, a line of LED's one after the other, making a wave

//set up the variables for the leds
const int LED1 = 2;
const int LED2 = 3;
const int LED3 = 4;
const int LED4 = 5;
const int LED5 = 6;
const int LED6 = 7;
const int LED7 = 8;
const int LED8 = 9;
const int LED9 = 10;
const int LED10 = 11;

//set all the led pins as outputs
void setup() {
  pinMode(LED1, OUTPUT);
  pinMode(LED2, OUTPUT);
  pinMode(LED3, OUTPUT);
  pinMode(LED4, OUTPUT);
  pinMode(LED5, OUTPUT);
  pinMode(LED6, OUTPUT);
  pinMode(LED7, OUTPUT);
  pinMode(LED8, OUTPUT);
  pinMode(LED9, OUTPUT);
  pinMode(LED10, OUTPUT);
}

/*
turn on all the LED's one by one
then turn them all off one after the other
starting with the first one
*/
void loop() {
  digitalWrite(LED1, HIGH);
  delay(500);
  digitalWrite(LED2, HIGH);
  delay(500);
  digitalWrite(LED3, HIGH);
  delay(500);
  digitalWrite(LED4, HIGH);
  delay(500);
  digitalWrite(LED5, HIGH);
  delay(500);
  digitalWrite(LED6, HIGH);
  delay(500);
  digitalWrite(LED7, HIGH);
  delay(500);
  digitalWrite(LED8, HIGH);
  delay(500);
  digitalWrite(LED9, HIGH);
  delay(500);
  digitalWrite(LED10, HIGH);
  delay(500);
  digitalWrite(LED1, LOW);
  delay(500);
  digitalWrite(LED2, LOW);
  delay(500);
  digitalWrite(LED3, LOW);
  delay(500);
  digitalWrite(LED4, LOW);
  delay(500);
  digitalWrite(LED5, LOW);
  delay(500);
  digitalWrite(LED6, LOW);
  delay(500);
  digitalWrite(LED7, LOW);
  delay(500);
  digitalWrite(LED8, LOW);
  delay(500);
  digitalWrite(LED9, LOW);
  delay(500);
  digitalWrite(LED10, LOW);
  delay(500);
}
```

So what is this code doing?

```
//set up the variables for the leds
const int LED1 = 2;
const int LED2 = 3;
const int LED3 = 4;
const int LED4 = 5;
const int LED5 = 6;
const int LED6 = 7;
const int LED7 = 8;
const int LED8 = 9;
const int LED9 = 10;
const int LED10 = 11;
```
We assign all the pins to variables so if we need to change a pin we just change these lines.

```
//set all the led pins as outputs
void setup() {
  pinMode(LED1, OUTPUT);
  pinMode(LED2, OUTPUT);
  pinMode(LED3, OUTPUT);
  pinMode(LED4, OUTPUT);
  pinMode(LED5, OUTPUT);
  pinMode(LED6, OUTPUT);
  pinMode(LED7, OUTPUT);
  pinMode(LED8, OUTPUT);
  pinMode(LED9, OUTPUT);
  pinMode(LED10, OUTPUT);
}
```
Then in the *void setup()* function we set all 10 LEDs as OUTPUTS.

```
/*
turn on all the LED's one by one
then turn them all off one after the other
starting with the first one
*/
void loop() {
  digitalWrite(LED1, HIGH);
  delay(500);
  digitalWrite(LED2, HIGH);
  delay(500);
  digitalWrite(LED3, HIGH);
  delay(500);
  ...
```
Now in the *loop()* function we start switching on the LED's one by one with
half second delays in between, (the rising edge of the wave).

```
  digitalWrite(LED7, LOW);
  delay(500);
  digitalWrite(LED8, LOW);
  delay(500);
  digitalWrite(LED9, LOW);
  delay(500);
  digitalWrite(LED10, LOW);
  delay(500);
}
```
Then after all the LED's are on, we start switching them off with half second
delays in between, (the falling edge of the wave).

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
