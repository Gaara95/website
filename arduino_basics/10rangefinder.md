## Rangefinder
Now lets use another complex sensor: an ultrasonic rangefinder, these use ultrasonic
sound to tell how far away an object is from it, exactly like [bats echolocation](https://en.wikipedia.org/wiki/Bat#Echolocation).
It does this by sending an ultrasonic *ping* and then waiting for it to return,
by measuring how long it takes for the *ping* to return we can calculate how far
an object is from it.

For this segment we will make an Arduino blink an LED, or buzz a buzzer(or both)
at a rate dependent on how close an object is to it.

**Parts:**

* An Arduino.
* An HC-SR04 ultrasonic sensor.
* An LED or buzzer(or both).
* A breadboard.
* A 1k resistor if you use an LED.

### Wiring

If you use an LED dont forget its resistor.

Arduino     |     HC-SR04
------------|-----------------
pin 12      |     Trigger (ping)
pin 11      |     Echo (input)
GND         |     GND
PWR         |     PWR

Arduino     |     LED and,or buzzer
------------|---------------
Pin13       |     Power
GND         |     GND

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/03/rangefinder.png" alt="rangefinder" width="477" height="599" />

### The code

```
// rangefinder using the HC-SR04 ultrasonic sensor

// the ultrasonic sensors pins
const int trigger = 12; // sends a ping
const int echo = 11; // reads the ping

const int LED = 13; // the LED or buzzer

void setup() {
  pinMode(trigger, OUTPUT); // set the trigger as output
  pinMode(echo, INPUT); // set the reader as input
  pinMode(LED, OUTPUT);
}

void loop() {

  long duration, cm; // used to calculate distance

  // send a ping
  digitalWrite(trigger, LOW); // pull it low for a clean ping
  delayMicroseconds(2);
  digitalWrite(trigger, HIGH); // send the ping
  delayMicroseconds(5);
  digitalWrite(trigger, LOW); // pull it low again

  // store the time it takes for the ping to return
  duration = pulseIn(echo, HIGH);
  // convert the time it takes into centimeters
  cm = microsecondsToCentimeters(duration);

  /*
  depending on how far away a detected object is, blink an LED or beep a buzzer.
  We do this by letting the LED/buzzer be on or off for the distance
  of an object multiplied by 10
  we multiply by ten so the waits are longer else you wouldn't notice them.
  */
  if ((cm > 1) and (cm < 20)) { // ignore stuff closer than 1 cm and further than 20 cm.
    digitalWrite(LED, HIGH);
    delay(cm * 10);
    digitalWrite(LED, LOW);
    delay(cm * 10);
  }
}

/*
function that converts the ping time reading to distance
*/
long microsecondsToCentimeters(long microseconds)
{
  // The speed of sound is 340 m/s or 29 microseconds per centimeter.
  // The ping travels out and back, so to find the distance of the
  // object we take half of the distance traveled.
  return microseconds /29 / 2;
}
```

When the trigger pin on the sensor is set HIGH the sensor sends a *ping*, and
when the ping returns the echo pin goes high.

In the loop function we have two *long* variables: duration and cm.
A *long* variable is a variable in which you can store larger sizes of data,
we use it since *duration* and *cm* can become quite large the further away an
object is from the sensor.
```
long duration, cm;
```
duration is used to store how long it takes for the ping to return and cm is used
to store the objects distance from the sensor.

Then we send the outgoing ping, before we send the ping we pull the trigger pin
low so there is no noise in the ping.
```
digitalWrite(trigger, LOW); // pull it low for a clean ping
delayMicroseconds(2);
digitalWrite(trigger, HIGH); // send the ping
delayMicroseconds(5);
digitalWrite(trigger, LOW); // pull it low again
```
Notice how we use *delayMicroseconds()* instead of *delay()*, this is because the
*pings* have to be really small pulses of sound.

After the ping is sent we use the *pulseIn()* function to measure how long it takes
for *echo* to read *HIGH*, the *pulseIn()* function takes a pin as the first argument
and the second argument is the expected state of the pin, it basically reads the
pin continuously till the value read from the pin matches the expected value and
returns the time taken till the values matched in microseconds.
In our case it reads the *echo* pin and returns the time it took to become *HIGH*,
and we store the time in *duration*.
```
// store the time it takes for the ping to return
duration = pulseIn(echo, HIGH);
// convert the time it takes into centimeters
cm = microsecondsToCentimeters(duration);
```
Then we do *cm = microsecondsToCentimeters(duration);* which calls the function
to convert the time it took into centimeters and store that in *cm* (I'll explain that later.)

Then we use an if statement to filter out objects further than 20 centimeters and
closer than 1 centimeter.
```
if ((cm > 1) and (cm < 20)) { // ignore stuff closer than 1 cm and further than 20 cm.
  digitalWrite(LED, HIGH);
  delay(cm * 10);
  digitalWrite(LED, LOW);
  delay(cm * 10);
}
```
Inside the if statement we blink an LED or buzz a buzzer, and we set the delay times
to the distance of the object from the sensor multiplied by 10, we multiply by
10 so the delays are big enough.

Now, onto the interesting bit, this function takes *duration* and turns it into *cm*
```
long microsecondsToCentimeters(long microseconds)
{
  // The speed of sound is 340 m/s or 29 microseconds per centimeter.
  // The ping travels out and back, so to find the distance of the
  // object we take half of the distance traveled.
  return microseconds /29 / 2;
}
```
The comments explain it, but anyway:
sound travels at 340 meters per second, 29 microseconds per centimeter.
The ping goes out and back.
So to turn *duration* into *cm* we divide *duration* by 29 and divide that by 2.
and we use the *return* statement which just sends back the calculated value so
it can be stored in *cm*

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
