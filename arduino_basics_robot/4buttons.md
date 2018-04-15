Now that you know how to control LEDs lets learn how to take inputs.

**Parts:**

* An Arduino.
* An LED.
* A button.
* A 1k resistor (for the LED).
* A 10k resistor (for the button).
* A breadboard.

### Wiring

We'll be connecting a 10k "pull-down" resistor between the button pin we are reading and ground,
this is to pull the signal from the button 'LOW' when it isn't pressed and remove 'noise' from the reading.

Arduino | Button
------------|--------------
Pin7 | Pin 1
PWR | Pin 2

Arduino | LED
------------|--------------
Pin13 | Power
GND | GND

**Diagram:**

<img class="aligncenter wp-image-108 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/03/button.png" alt="" width="600" height="632" />

### The code

**Version 1:**
Turns on the LED while the button is pressed and turns it off when the button is released.
For this you will use the *digitalRead()* function, it is used to read signals from pins,
it can tell if there is any voltage applied to the pin and returns 'HIGH' or 'LOW'.

```
//Turn on an LED when the button is pressed.

//the pin for the LED and button
const int LED = 13;
const int BUTTON = 7;

int val = 0; //used to store the state of the input pin

void setup() {
  pinMode(LED, OUTPUT);
  pinMode(BUTTON, INPUT);
}

void loop() {
  val = digitalRead(BUTTON); //read the button's state and store it.

  //if the button is pressed turn on the LED, else leave it off.
  if (val == HIGH) {
    digitalWrite(LED, HIGH);
  } else {
    digitalWrite(LED, LOW);
  }
}
```
How does this work?
First we read the button and store its status in val: *val = digitalRead(BUTTON);*

The if statement:
The *if* statement is possibly the most important part of a programming language,
since it enables the computer to make choices.
The usual structure of an if statement goes like this:
```
if (some condition is fulfilled) {
  do stuff
} else {
  do this stuff
}
```
After an if statement you have the condition to be fulfilled, in our case "is the button pressed?"
If the button is pressed the code in the if statement is run, if the button is not pressed the code in the *else* statement is run.
Notice how in the if statement we use == instead of =, this is because in every programming
language = is used to assign a value to something, like: *apple = red*
and == is used to compare two things it returs *True* if the two things are equal
and *False* if they are not equal, like: *apple == good* which checks if *apple* is equal to *good*.

**Version 2:**
Holding down the button to keep the light on is sub-optimal, so lets change it
to stay on after the button is pressed.

For this we will use a *variable*, a variable is used to store something in
memory, like so: *int val = 0;* here we are saying val is an integer and that
its value is 0, variables can be changed later in the code so you could do
*val = 300;* and vals value would become 300.

```
//Turn on an LED when the button is pressed and keep it on after it is released.

//the pin for the LED and button
const int LED = 13;
const int BUTTON = 7;

int val = 0; //used to store the state of the input pin.

int state = 0; //used to remember if the LED has to be on or off.

void setup() {
  pinMode(LED, OUTPUT);
  pinMode(BUTTON, INPUT);
}

void loop() {
  val = digitalRead(BUTTON); //read the button's state and store it.

  //check if the button is pressed and change the state.
  // 1 - 0 = 1 and 1 - 1 = 0, and so it switches, neat right?
  if (val == HIGH) {
    state = 1 - state;
  }

  //if the state is 1 turn the LED on
  if (state == 1) {
    digitalWrite(LED, HIGH);
  } else {
    digitalWrite(LED, LOW);
  }
}
```
How does it work?
We use *val* to remember the result of *digitalRead(BUTTON);* so we know if the
button is pushed or not.
We also use a second variable *state* to remember if the LED has to stay on or off.

The program waits for the button to be pressed:
```
if (val == HIGH) {
  state = 1 - state;
}
```
and does this interesting thing: *state = 1 - state*, remember *state* can only be
0 or 1, and so we use a math trick: *0 - 1 = 1 and 1 - 1 = 0*,
this way if the button is pressed and the LED was on the LED is turned off,
and if the button is pressed and the LED was off the LED is turned on.

Then it checks if *state* is 1 and turns on the LED if it is, and if *state* is not
1 the LED is turned off.
```
if (state == 1) {
  digitalWrite(LED, HIGH);
} else {
  digitalWrite(LED, LOW);
}
```

**Version 3:**
The previous code has a flaw in it, when you press the button the light changes
rapidly on and off.
The reason it is unreliable is because of how we read the button. The Arduino
executes its instructions at a rate of 16 million a second, this means when the
button is pushed the Arduino reads the button a few thousand times and changes *state*
rapidly.

How to fix this?
We need to detect the exact moment the button is pressed and only change it in that moment.
The way we will be doing it is: we store the value of *val* before we read it again
that way we can compare the current state of the button with the previous state
and change *state* only when the button becomes HIGH after being LOW.
```
if (val == HIGH) && (old_val == LOW)) {
  state = 1 - state;
}
```

This is where we store the old value of val so we can do the comparison spoken of above.
```
old_val = val; // 'val' is old now, remember that.
```

```
//Turn on an LED when the button is pressed and keep it on
//improved so it actually works.

//based on code from Massimo Banzi's 'Getting started with arduino'

//the pin for the LED and button
const int LED = 13;
const int BUTTON = 7;

int val = 0; //used to store the state of the input pin.

int old_val = 0; //used to store the previous value of 'val'.

int state = 0; //used to remember if the LED has to be on or off.

void setup() {
  pinMode(LED, OUTPUT);
  pinMode(BUTTON, INPUT);
}

void loop() {
  val = digitalRead(BUTTON); //read the button's state and store it.

  //check if there was a transition
  //i.e. if val is HIGH and old_val is LOW.
  if (val == HIGH) && (old_val == LOW)) {
    state = 1 - state;
  }

  old_val = val; // 'val' is old now, remember that.

  //if the state is 1 turn the LED on
  if (state == 1) {
    digitalWrite(LED, HIGH);
  } else {
    digitalWrite(LED, LOW);
  }
}
```

**version 4:**
You would have noticed, the previous attempt is also buggy, this is because of the mechanical
switches we are using, these buttons are made from two pieces of metal separated with a spring.
When pressed the pieces come together and current flows, this sounds fine, but the connection is
not always perfect especially when the button isn't fully pressed, and it generates "noisy"
signals called bouncing which the Arduino sees as rapid on and offs.

To fix this we add a 10 to 50 millisecond delay when we are detecting transitions.

The only change we make is adding the delay to this code:
```
if (val == HIGH) && (old_val == LOW)) {
  state = 1 - state;
  delay(10); //debouncing, removes button noise.
}
```

```
//Turn on an LED when the button is pressed and keep it on
//improved so it actually works, with debouncing.

//based on code from Massimo Banzi's 'Getting started with arduino'

//the pin for the LED and button
const int LED = 13;
const int BUTTON = 7;

int val = 0; //used to store the state of the input pin.

int old_val = 0; //used to store the previous value of 'val'.

int state = 0; //used to remember if the LED has to be on or off.

void setup() {
  pinMode(LED, OUTPUT);
  pinMode(BUTTON, INPUT);
}

void loop() {
  val = digitalRead(BUTTON); //read the button's state and store it.

  //check if there was a transition
  if (val == HIGH) && (old_val == LOW)) {
    state = 1 - state;
    delay(10); //debouncing, removes button noise.
  }

  old_val = val; // 'val' is old now, remember that.

  //if the state is 1 turn the LED on
  if (state == 1) {
    digitalWrite(LED, HIGH);
  } else {
    digitalWrite(LED, LOW);
  }
}
```

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
