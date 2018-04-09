These are the projects used in our Arduino-basics summer camp.

We will cover setting up a programming environment, wiring up sensors,
the basic electronics, and coding for the Arduino.

All the code can be found [here](https://github.com/afshaan4/other_arduino_projects)

## Installing The Arduino IDE
Go to the Arduino downloads page and download the Arduino IDE for your OS, you will get a zip file.

Follow the installation instructions for your OS.

**For Linux**

* Extract the zip file you downloaded.
* Open the arduino-1.6.x folder created by the extraction process and find install.sh
* Right click on *install.sh* and select *run in terminal*,
if you can't find the option to run the script from the menu
open a terminal window and navigate to the Arduino folder by typing:
```
cd path/to/folder
```
then run the script by typing:
```
./install.sh
```
after it has finished running you should have an Arduino icon on your desktop.

You might run into an error saying *Error opening serial port* when you try to upload a sketch
to the board the first time.
To fix this run
```
sudo usermod -a -G dialout username
```
in a terminal replacing "username"
with your user-name, then log out and log back in for the changes to take effect.

**For macOS**

* Extract the zip file you downloaded.
* Copy the extracted application into your Applications folder.
* And you're done.
* To install FTDI drivers:

*If you are using an Arduino Uno or Mega2560 (or a compatible board) you don't
need to this step, for other boards you need a driver for the FTDI chip on the Arduino.*

* Go to the [FTDI website](http://www.ftdichip.com/Drivers/VCP.htm) and download the drivers.
* Once it's downloaded, double-click the package and follow the instructions from the installer.
* Restart your computer after the installation is done.

**For Windows**

* Extract the zip file you downloaded.
* Run the installer (it's the .exe file) and allow it to install the drivers necessary.
* During the install process select the folder you want to install it in,
I suggest using the default.
* And you're done.
* For better instructions go [here](https://learn.sparkfun.com/tutorials/installing-arduino-ide#windows).

## Getting started

We'll start with the most basic project: blinking an LED.

**Parts:**

* An Arduino.
* An LED.
* A 1k resistor.

### Wiring

Connect the positive (power) pin of the LED to pin 13 on the Arduino through a
1k(1 kilo-ohm) resistor, and connect the negative (ground) pin of the LED to the ground pin of the Arduino.

Arduino | LED
-------------|-------------
Pin 13 | Anode(power)
GND | Cathode(ground)

**Diagram:**

<img class="aligncenter wp-image-110 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/03/blink.png" alt="" width="497" height="607" />

### The code

type the following code into the Arduino IDE, or you can download the code [here](https://github.com/afshaan4/other_arduino_projects/blob/master/blink/blink.ino)

```
//blinks an LED

const int LED = 13;

//set LED as an output
void setup()
{
  pinMode(LED, OUTPUT);
}

//blink the LED
void loop()
{
  digitalWrite(LED, HIGH);
  delay(1000);
  digitalWrite(LED, LOW);
  delay(1000);
}
```

Now that the code is in your IDE, you need to compile it, this takes the code and
turns it into instructions that the Arduino understands, it also checks if the
code has any mistakes in it. To compile it press the "Verify" button in the IDE:

<img class="aligncenter wp-image-46 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/03/blink_verify.png" alt="verify" width="500" height="372" />

If everything is correct you should see "Done compiling" at the bottom of the IDE.

Now you can upload the code to the board: Press the "Upload" button which is right
next to the "Verify" button. This will force the board to stop what it's doing
and listen for the code coming from your computer, you will also see some messages
appear at the bottom of the Arduino IDE and once it's done you will
see "Done uploading" at the bottom of the IDE. There are two LED's on the board
marked "RX" & "TX" these keep flickering when the code is being uploaded to the board.

Once the code is uploaded to the board the LED should start blinking.

### Explaining the code

So first off, the Arduino executes code you write for it from the top to the
bottom, it starts from the line at the top then moves down.

Notice the curly brackets in the code, these are functions; these are used to
group together a bunch of instructions and give them a name. This is useful when
you have a bunch of code you want to execute multiple times, instead of writing
the same bunch of code *over and over* whenever you want it to be executed, you
just stick it in curly brackets and call it whenever you want it to run.

You can see two blocks of code defined this way here, and before them there are these lines:
*void setup()* and *void loop()*
These lines give a name to the function, if you want to write a block of code
that makes an Arduino open a door(for example), you would write:
```
void openDoor() {
  //instructions to open the door
}
```

and to call it you write:
```
openDoor();
```
from anywhere in your code, and the Arduino will execute the commands to open the door.

Now, Arduino expects two functions to exist: *setup()* and *loop()*. *setup()*
is where you put all the code you want to execute once at the beginning of the
program, hence the name *setup*, and *loop()* is where you put code to be executed
over and over until you turn off the Arduino, or the end of time.
This is done because the Arduino is not like a regular computer -it can't do
multiple things at once and programs can't quit. When you turn on the Arduino,
the code runs and when you want to stop it, you just turn it off.

### The Code, line by line

```
//blinks an LED
```
This is a comment, any line beginning with *//* is a comment and the Arduino
ignores it, comments are used to write notes explaining what the code does and
wherever necessary, how it works.

```
const int LED = 13;
```
here we are assigning LED to pin 13 on the Arduino, *const int* means *LED* is
an integer that can't be changed.

```
void setup()
```
This tells the Arduino that the next block of code is called *setup()*.

```
{
```
The opening of the *setup()* function.

```
pinMode(LED, OUTPUT);
```
*pinMode* tells the Arduino how to configure a given pin, here we are setting *LED*,
which is pin 13, to OUTPUT so we can drive the LED, pins can be set as INPUT,
to read from sensors, or OUTPUT to drive LED's and other things.

```
}
```
The end of the *setup()* function.

```
void loop()
```
This is the main function of the code, it repeats over and over until the Arduino is switched off.

```
{
```
The opening of the *void loop()* function.

```
digitalWrite(LED, HIGH);
```
*digitalWrite()* turns on and off any pin that has been configured as an *OUTPUT*.
The first argument that it takes is the pin it's controlling (which in this case is *LED*),
the second argument is either HIGH which turns the pin on, or LOW which turns the pin off.

```
delay(1000);
```
This line tells the Arduino to wait for a second, this is done so the LED stays on,
if we removed it the Arduino would immediately go from *digitalWrite(LED, HIGH);*
to *digitalWrite(LED, LOW);* and turn the LED off right after it's turned on.
Delay takes arguments in milliseconds, 1000 milliseconds = one second.

```
digitalWrite(LED, LOW);
```
Turns the LED off.

```
delay(1000);
```
Wait again, this time so the LED stays off for a second.

```
}
```
The end of the *void loop()* function.

Now that you understand what the code does, try changing around some things,
like the delay time, try lowering the delay time to really small amounts of time
and see what happens, it's pretty interesting.

## LED waves

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

## Taking input with buttons

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

## Driving motors
Now that you learned how to drive small loads like LEDs and take inputs,
lets drive bigger loads, like motors.

Each pin on the Arduino can output up to 20 milliamps, that is a really small amount of current,
and so if you try to power a motor directly with the pin the Arduinos processor will burn out
and it will most probably catch fire.
To drive bigger loads we will use a motor driver, the L293D specifically, it
allows us to switch on and off motors and is controlled by the Arduino.

**Parts:**

* An Arduino.
* A DC motor.
* An L293D motor driver board.
* A 9v battery and battery clip.

### Wiring

Arduino | Motor driver
------------|-------------------
Pin0 | Enable motor a
Pin1 | Motor a pin 1
Pin2 | Motor a pin 2
Pin3 | PWR
Pin4 | GND

9v battery | Motor driver
------------|------------------
9v | PWR to motors
GND | GND for motors

**Diagram:**

<img class="wp-image-112 size-full aligncenter" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/03/motors.png" alt="" width="600" height="599" />

### The code
The motor driver takes three inputs per motor: *motor enable*, *motor pin1*, and *motor pin2*.
If *motor enable* is HIGH the motor gets power and if it is LOW the motor does not get power.

*motor pin1* and *motor pin2* are used to control the direction the motor spins in:
if *motor pin1* is HIGH and *motor pin2* is LOW the motor spins in one direction;
if *motor pin1* is LOW and *motor pin2* is HIGH the motor spins the other way;
if *motor pin1* and *motor pin2* are both LOW the motor stops moving.
That way we can control which way the motor spins and turn it on and off.

```
//makes a DC motor spin one direction for a second then the other

//assign the motor controller pins
int ea = 0; //enable motor a
int a1 = 1; //motor a pin 1
int a2 = 2; //motor a pin 2
int V = 3; //power to the motor controller
int G = 4; //ground to the motor controller

void setup() {
  //setup the motor controller pins
  pinMode(ea, OUTPUT); //enable motor a is an output
  pinMode(a1, OUTPUT);
  pinMode(a2, OUTPUT);
  pinMode(V, OUTPUT);
  pinMode(G, INPUT);

  digitalWrite(ea, HIGH); //turn on motor a
  //power on the controller board
  digitalWrite(V, HIGH);
  digitalWrite(G, LOW);
}

void loop() {
  //turn on pin 1 and turn off pin 2
  //the motor spins one way
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  delay(1000); //let it spin for a second
  //turn off pin 1 and turn on pin 2
  //the motor spins the other way
  digitalWrite(a1, LOW);
  digitalWrite(a2, HIGH);
  delay(1000); //let it spin for a second
}
```
How does this work?

We enable one motor and set *motor pin1* and *motor pin2* as outputs, and use
pins 3 and 4 on the Arduino to power the motor driver, not the motors, since the
L293D chip only needs 20 milliamps to function:
```
void setup() {
  //setup the motor controller pins
  pinMode(ea, OUTPUT); //enable motor a is an output
  pinMode(a1, OUTPUT);
  pinMode(a2, OUTPUT);
  pinMode(V, OUTPUT);
  pinMode(G, INPUT);

  digitalWrite(ea, HIGH); //turn on motor a
  //power on the controller board
  digitalWrite(V, HIGH);
  digitalWrite(G, LOW);
}
```
Then in the loop we just spin the motor one way for a second, then the other
way for a second:
```
void loop() {
  //turn on pin 1 and turn off pin 2
  //the motor spins one way
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  delay(1000); //let it spin for a second
  //turn off pin 1 and turn on pin 2
  //the motor spins the other way
  digitalWrite(a1, LOW);
  digitalWrite(a2, HIGH);
  delay(1000); //let it spin for a second
}
```

## Reading analog signals
So far we have learned how to read if a sensor is *on* or *off*, but what if you
want to use a light sensor, which can tell us whether or not there is light, as well as
*how much* light there is, to read such a sensor we use the analog input pins on
an Arduino they're the ones on the lower right of the board, marked *A0* to *A5*,
using the *analogRead()* function we can read the voltage applied to one of these pins.
*analogRead()* returns a number between 0 and 1023 which represents voltages from
0 to 5 volts.

So for this example we'll have an Arduino read values form an LDR, which stands for
light dependent resistor, in the darkness an LDRs resistance is quite high.
When you shine light at it, it's resistance drops proportionally to how much light
shines on it.

And we'll blink an LED at a speed dependent on readings from the sensor.

**Parts:**

* An Arduino.
* An LDR.
* A 10k resistor.
* A breadboard.
* And optionally, an LED and accompanying 1k resistor.

### Wiring

If you are adding an external LED connect it to pin 13.

Arduino   |    LDR
----------|--------------
Analog0   |    pin1
PWR       |    pin2

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/03/analog_read.png" alt="" width="600" height="783" />

### The code
Notice how we don't set the analog-in pin as an input, that is because analog input
pins are set as inputs by default.

```
// blink an LED at a speed based on
// analog input from analog pin 0

const int LED = 13;

int val = 0;

void setup() {
	pinMode(LED, OUTPUT);

	//analog pins are input by default
}

void loop() {
	val = analogRead(0); // read the sensor value

	digitalWrite(LED, HIGH);
	delay(val); // wait, using the reading from val as the wait time
	digitalWrite(LED, LOW);
	delay(val); // wait, using the reading from val as the wait time
}
```

This code is pretty simple, we read the sensor and store the reading in val:

```
val = analogRead(0); // read the sensor value
```

Then we turn on and off the LED, and set the delay time in between on and off
to the reading we get from the sensor:
```
  digitalWrite(LED, HIGH);
  delay(val); // wait, using the reading from val as the wait time
  digitalWrite(LED, LOW);
  delay(val); // wait, using the reading from val as the wait time
}
```

## Dimming LEDs with analogWrite
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

## Serial communication
So far we have used LEDs and buzzers as output devices, but what if you wanted to
collect and read values from a sensor? For that we can use the USB port on the Arduino
to communicate with a computer.

So we're going to read from a light sensor and send the values to the computer
using serial communication.

**Parts:**

* An Arduino.
* An LDR (light sensor)
* A 10k resistor.
* A breadboard.

### Wiring

Arduino    |    LDR
-----------|------------
Analog0    |    pin1
PWR        |    pin2

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/serial_comms.png" alt="Serial" width="600" height="721" />

### The code
Once you have uploaded this to the board hit the *Serial monitor* button, which
is in the top right corner of the Arduino IDE, this opens a new window where all
the data coming from the Arduino is displayed.

```
// sends values read from analog input 0
// to a computer with serial print.

const int sensor = 0; // the pin for the analog sensor

int val = 0; // used to store the reading from the sensor

void setup() {
	// open the serial port to the computer
	// and set the transmission speed to 9600 baud
	Serial.begin(9600);

	// analog pins are input by default
}

void loop() {
	val = analogRead(sensor); // read the sensor
	Serial.println(val); // send the value read from the sensor to the computer

	delay(100); // wait between each send makes reading through the stuff easier
}
```
In the *void setup()* function we open the serial connection to the computer
and we set the transmission speed to 9600 baud, the baud rate is how many bits
per second are sent over the connection.
```
Serial.begin(9600);
```
Also notice that we don't set the analog input pin as an input, this is because
analog pins are set as inputs by default.

Then in *void loop()* we read the sensor and use *Serial.println()* to send *val*
to the computer, *Serial.println()* sends the value to the computer then makes a new
line that way we have a column of values and not just a line of all the values.
```
Serial.println(val);
```
We add a delay of 100 milliseconds so there is a pause between each time *val* is
sent to the computer, if you remove this the rate at which val is sent will be really
high and flood the serial monitor.

## Motion Alarm
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

## Roamer
Now that you know how to read sensors and control LEDs and motors, lets build a robot.
So the robot we will build is going to be an obstacle avoiding robot, it will drive around
and avoid things in it's way.

We will make it a "car" robot so it is going to move around on wheels,
and to sense it's surroundings we will give it an ultrasonic rangefinder.

**Powering the Arduino with a battery**

The processor on the Arduino can only take upto 5 volts, more than that and it can
burn up, so far we used a USB port connected to a computer to power it and USB ports
deliver 5v power so we were fine, but for the robot we use 9volt batteries,
so how do we power the Arduino with 9volt batteries and not burn it? Well to
power it with batteries we can use the *vin* on the Arduino, this pin is connected
to a power regulation circuit, so it can take 7 to 12 volts and conver it to 5 volts
for the processor.

So to power the Arduino with the 9volt battery plug the positive terminal into *vin*
and the negative terminal into *GND*.

**Parts:**

* An Arduino.
* An HC-SR04 ultrasonic sensor.
* An L293D motor driver board.
* A robot chassis.
* Two DC motors.
* A castor(used as a third wheel, it can move in any direction).
* Two wheels that fit the motor shafts.
* Two 9v batteries and accompanying battery clips.
* Various screws and bolts to mount all this stuff to the robot chassis.
* A screwdriver.

### Building the robot

* Unscrew the big nut on the motors:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly1.jpg" alt="assembly1" width="600" height="536" />

* And send the motors axle side through the mounting hole on the chassis:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly2.jpg" alt="assembly2" width="600" height="612" />

* Then screw on the nut back to the motors:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly3.jpg" alt="assembly3" width="600" height="496" />

* Now slide the wheels onto the motors axles and tighten the screws on the motors
  till the wheels are firmly connected to the motors,
  don't tighten too much or you could ruin the threading on the wheels:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly4.jpg" alt="assembly4" width="600" height="856" />

* Now with screws and nuts mount the castor to the bottom front of the chassis:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly5.jpg" alt="assembly5" width="702" height="600" />

* Put a layer of plastic or paper on the top of the chassis
  then put the Arduino and motor driver on it, the layer of paper is to stop the
  Arduino and motor driver from shorting on the metal chassis:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly6.jpg" alt="assembly6" width="601" height="600" />

* Use screws to mount the ultrasonic sensor to the front of the chassis:

  <img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/assembly7.jpg" alt="assembly1" width="746" height="600" />


## Wiring


Arduino     |     Motor driver
------------|-----------------
pin 0       |     Enable motor a
pin 1       |     Motor a pin 1
pin 2       |     Motor a pin 2
pin 3       |     PWR
pin 4       |     GND
pin 5       |     Enable motor b
pin 6       |     Motor b pin 2
pin 7       |     Motor b pin 1

Arduino     |     HC-SR04
------------|-----------------
pin 12      |     Trigger (ping)
pin 11      |     Echo (input)
GND         |     GND
PWR         |     PWR

Motor driver     |     Motors
-----------------|----------------
terminalA1       |     motorA wire 1
terminalA2       |     motorA wire 2
terminalB1       |     motorB wire 1
terminalB2       |     motorB wire 2

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/rover.png" alt="rover" width="600" height="600" />

### The code
```
// an obstacle avoiding robot

long randNumber; // used to store the random number.

// assign the motor controller pins
int ea = 0; //enable a
// motor one
int a1 = 1;
int a2 = 2;
int eb = 5; // enable b
// motor two
int b2 = 6;
int b1 = 7;
int V = 3; // power to the motor controller
int G = 4; // ground to the motor controller
// the ultrasonic sensors pins
const int trigger = 12;
const int echo = 11;

/*
function used to make the robot back up and then turn left
*/
void backLeft() {
  // back up for a second
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
  delay(1000);
  // stop the motors
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(10);
  // turn left for a second
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, HIGH);
  delay(1000);
  // stop the motors
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(10);
}

/*
function used to make the robot back up and then turn right
*/
void backRight() {
  // back up for a second
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
  delay(1000);
  // stop the motors
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(10);
  // turn right for a second
  digitalWrite(a1, LOW);
  digitalWrite(a2, HIGH);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
  delay(1000);
  // stop the motors
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(10);
}

// the setup stuff
void setup() {
  // setup the motor controller pins
  pinMode(ea, OUTPUT);
  pinMode(a1, OUTPUT);
  pinMode(a2, OUTPUT);
  pinMode(V, OUTPUT);
  pinMode(G, INPUT);
  pinMode(eb, OUTPUT);
  pinMode(b2, OUTPUT);
  pinMode(b1, OUTPUT);
  // turn the motor driver on and enable both motors
  digitalWrite(ea, HIGH);
  digitalWrite(eb, HIGH);
  digitalWrite(V, HIGH);
  digitalWrite(G, LOW);
  pinMode(trigger, OUTPUT);
  pinMode(echo, INPUT);
}

void loop() {
  // the ping stuff
  long duration, cm;
  // send a ping
  digitalWrite(trigger, LOW);
  delayMicroseconds(2);
  digitalWrite(trigger, HIGH);
  delayMicroseconds(5);
  digitalWrite(trigger, LOW);

  // listen for a return
  duration = pulseIn(echo, HIGH);
  // convert the time of the ping into cm
  cm = microsecondsToCentimeters(duration);
  // put random in a variable
  randNumber = random(0, 2);

  // the avoidance stuff
  // filter out values less than 1 and greater than 20 cm
  if (cm > 1){
    if (cm < 20){
    // based on randNumber turn left (0) or right(1)
      if (randNumber == 0){
        backLeft();
      }
      if (randNumber == 1){
        backRight();
      }
    } else { // if there isn't anything in front just go straight
      digitalWrite(a1, HIGH);
      digitalWrite(a2, LOW);
      digitalWrite(b1, HIGH);
      digitalWrite(b2, LOW);
      delay(150);
    }
  }
}

/*
function that converts the ping time reading to distance
*/
long microsecondsToCentimeters(long microseconds)
{
  // The speed of sound is 340 m/s or 29 microseconds per centimeter.
  // The ping travels out and back, so to find the distance of the
  // object we take half of the distance travelled.
  return microseconds /29 / 2;
}
```
For this project we want the robot to avoid objects by backing away from them then
turning left or right, and going forward again, to make the Arduino randomly decide
which way to turn after avoiding an object we use the *random()* function to choose
a random number from 0 and 1, and based on that the Arduino turns left(0) or right(1).

*random()* takes two arguments the first one is the lower limit of the random value
and the second one is the upper bound and is excluded from the random range, so
*random(0, 2)* generates a number from 0 to 1 not 0 to 2.

To turn left or right requires quite a bit of code so to keep things neat we put
those instructions in functions.
To do this we make the Arduino drive back for one second then spin one motor
back while the other stays off, turning the robot:
```
void backLeft() {
  // back up for a second
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
  delay(1000);
  // stop the motors
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(10);
  // turn left for a second
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, HIGH);
  delay(1000);
  // stop the motors
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(10);
}
```

Then in the loop we send a ping and store it's duration, this is also where we
generate our random number:
```
void loop() {
  // the ping stuff
  long duration, cm;
  // send a ping
  digitalWrite(trigger, LOW);
  delayMicroseconds(2);
  digitalWrite(trigger, HIGH);
  delayMicroseconds(5);
  digitalWrite(trigger, LOW);

  // listen for a return
  duration = pulseIn(echo, HIGH);
  // convert the time of the ping into cm
  cm = microsecondsToCentimeters(duration);
  // put random in a variable
  randNumber = random(0, 2);
```

This code does the obstacle avoiding:
```
// the avoidance stuff
// filter out values less than 1 and greater than 20 cm
if (cm > 1){
  if (cm < 20){
  // based on randNumber turn left (0) or right(1)
    if (randNumber == 0){
      backLeft();
    }
    if (randNumber == 1){
      backRight();
    }
  } else { // if there isn't anything in front just go straight
    digitalWrite(a1, HIGH);
    digitalWrite(a2, LOW);
    digitalWrite(b1, HIGH);
    digitalWrite(b2, LOW);
    delay(150);
  }
}
```
We use if statements to see if anything is in the way:
```
if (cm > 1){
  if (cm < 20){
```
if there is an obstacle we turn left if randNumber is 0, and right if randNumber is 1:
```
// based on randNumber turn left (0) or right(1)
  if (randNumber == 0){
    backLeft();
  }
  if (randNumber == 1){
    backRight();
  }
```
If there are no obstacles in the way we make the robot drive forward:
```
} else { // if there isn't anything in front just go straight
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
  delay(150);
}
```

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
