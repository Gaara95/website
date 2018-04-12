## Making analog signals
Now that you know how to read analog signals lets learn how to make analog signals
with the Arduino.

I mentioned in the Blinking LED section, that changing the delay times to small numbers
between turning the LED on and off results in something "interesting",
if you did that you would have noticed that as the delays get smaller you don't
see the LED blinking anymore but instead it gets dimmer.

This is [persistence of vision](https://en.wikipedia.org/wiki/Persistence_of_vision)
you can do some pretty impressive stuff with it like [POV displays](https://learn.adafruit.com/search?q=POV&)
which use just few LEDs in a line and when spun display words or drawings.

The Arduino has hardware that can make analog signals on pins 9, 10, and 11
To make analog signals on these pins we use the *analogWrite()* this takes the pin
it is controlling and a value between 0 an 255 as the analog signal where 0 is off
and 255 is maximum brightness, so this:
```
analogWrite(9, 128);
```
will set the brightness of an LED connected to pin 9 to 50%.
analogWrite doesn't only have to be used to set an LEDs brightness, it can be used
to control the speed of a motor, or change the sound of a buzzer and more.

So, for this example we'll be pulsing an LED like on a sleeping laptop.

**Parts:**

* An Arduino.
* An LED.
* A 1k resistor.
* A breadboard.

### Wiring

Put a 1k resistor between the cathode of the LED and ground.

Arduino   |   LED
----------|---------
Pin9      |   Anode
GND       |   Cathode

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/03/analog_write.png" alt="AnalogWrite" width="600" height="664" />

### The code

```
// Fades an LED in and out, like a sleeping laptop

const int LED = 9;
int i = 0; //used to count up and down

void setup() {
  pinMode(LED, OUTPUT);
}

void loop() {
  // loop from 0 to 255 and increase the
  // brightness of the LED each time we go through the loop
  for (i = 0; i < 255; i++) {
    analogWrite(LED, i);
    delay(10); // wait a bit so we actually see the effect
  }

  // loop from 255 to 0 and decrease the
  // brightness of the LED each time we go through the loop
  for (i = 255; i > 0; i--) {
    analogWrite(LED, i);
    delay(10); // wait a bit so we actually see the effect
  }
}
```
Here we use a *for* loop to dim the LED, what is a for loop?
It's a loop that is used when you want to do something a certain amount of times,
for example if you want to start at 0 and repeat an action 50 times.

It's typical syntax is:
```
for (starting value; value to end at; increase or decrease the value) {
  stuff to do over and over
}
```

In our case we are using it to increase then decrease the LED brightness,
first we have it start at 0
```
int i = 0; //used to count up and down
```
Then each time we go through the loop we increase *i* by one and we set the LED brightness
to *i*, since *i* is increased each time the LED brightness does too, then we wait
for 10 milliseconds(just enough so we see the pulsing but not see the LED blinking).
We keep doing this till *i = 255* which means the LED is at maximum brightness.
```
// loop from 0 to 255 and increase the
// brightness of the LED each time we go through the loop
for (i = 0; i < 255; i++) {
  analogWrite(LED, i);
  delay(10); // wait a bit so we actually see the effect
}
```

Then we do the opposite starting at 255 and count down to 0 and decrease the
LED brightness each time.
```
// loop from 255 to 0 and decrease the
// brightness of the LED each time we go through the loop
for (i = 255; i > 0; i--) {
  analogWrite(LED, i);
  delay(10); // wait a bit so we actually see the effect
}
```
##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
