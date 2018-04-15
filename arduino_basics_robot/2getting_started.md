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

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
