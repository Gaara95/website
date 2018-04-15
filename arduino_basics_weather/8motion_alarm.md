Now lets use a more advanced sensor: a PIR motion sensor, which is  basically an infrared light
detector, when a person or object moves in the sensors field of view they cause changes
in the temperature across the room, the sensor sees these changes in temperature
(which is changes in infrared light) as movement.

The PIR sensors signal pin is an Open-collector which means when it detects motion
it pulls its output pin *LOW* and when there is no motion detected the pin is left floating
(it is not set HIGH or LOW), this means that we need to pull the pin up to prevent noise.
Luckily the Arduino has builtin pullup resistors on most of its pins.

For this segment we will be building a motion alarm, which will beep a buzzer
and send a message to a computer whenever it detects motion.

**Parts:**

* An Arduino.
* A PIR motion sensor (also called a D-sun sometimes).
* A buzzer.
* A breadboard.

### Wiring

The buzzer does not need a resistor like an LED.

Arduino     |     buzzer & sensor
------------|-----------------
pin 13      |     buzzer
pin 2       |     Input from the PIR sensor
GND         |     GND
PWR         |     PWR

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/motion_alarm.png" alt="motion" width="644" height="588" />

### The code

```
//Beeps a buzzer and sends a message over serial when motion is detected.

int buzzer = 13;                // the pin for the buzzer
int inputPin = 2;               // input pin (for PIR sensor)
int pirState = LOW;             // we start, assuming no motion detected
int val = 0;                    // variable for reading the pin status

void setup() {
  pinMode(buzzer, OUTPUT);      // declare buzzer as output
  pinMode(MOTION_PIN, INPUT_PULLUP);     // declare sensor as input, and internally pull it up

  Serial.begin(9600);
}

void loop(){
  val = digitalRead(inputPin);  // read input value
  if (val == HIGH) {           // check if the input is HIGH
    digitalWrite(buzzer, HIGH);  // turn buzzer ON
    if (pirState == LOW) {
      // we only print if there is a change in state
      Serial.println("Something moves in the shadows!");
      pirState = HIGH; // the motion began set pirState high
    }
  } else {
    digitalWrite(buzzer, LOW); //turn the buzzer OFF
    if (pirState == HIGH) {
      //we only want to print if there is a change in state
      Serial.println("The movement has ended.");
      pirState = LOW; // the motion began set pirState low
    }
  }
}
```

In the setup function we set *MOTION_PIN* as an *INPUT_PULLUP*, this sets it as an input
and pulls it up to 5v.
```
pinMode(MOTION_PIN, INPUT_PULLUP);
```

We start off assuming no motion is detected *int pirState = LOW;*.

Then in the loop function we read the sensor and if it is
detecting motion we turn the buzzer on and if there is no motion
we turn the buzzer off.

Now, we don't want the Arduino to keep sending us messages if there is motion,
we only want it to send us messages when the motion starts and when it ends,
for this we use the trick from the button example and check if *pirState* *was*
low before it became high and only then send a message.
We also only send a message when the motion ends if *pirState* *was* high before
it became low.
```
void loop(){
  val = digitalRead(inputPin);  // read input value
  if (val == HIGH) {           // check if the input is HIGH
    digitalWrite(buzzer, HIGH);  // turn buzzer ON
    if (pirState == LOW) {
      // we only print if there is a change in state
      Serial.println("Something moves in the shadows!");
      pirState = HIGH; // the motion began set pirState high
    }
  } else {
    digitalWrite(buzzer, LOW); //turn the buzzer OFF
    if (pirState == HIGH) {
      //we only want to print if there is a change in state
      Serial.println("The movement has ended.");
      pirState = LOW; // the motion has ended set pirState low
    }
  }
}
```
##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
