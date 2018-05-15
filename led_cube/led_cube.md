So for this project I wanted to make an LED cube, but as a "shield" for an Arduino
UNO, an Arduino shield is a board that plugs into the Arduinos pins and has
a bunch of circuitry on it, these are great when you want to add a complex thing
like a radio module to your Arduino, you just get a radio module shield and plug
it in no need to wire its pins to the Arduino with a breadboard.

Anyway, my finished cube looks like this:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/05/header_image.png" alt="" width="800" height="800" />

You will need to have moderate soldering skills and about
2 to 3 hours to build this thing.

**Parts:**

* 27 diffused LEDs, don't use the clear ones like I did
* 3 2k resistors.
* A piece of perf board big enough for you Arduino.
* 12 male breakaway headers, like [these](https://duckduckgo.com/?q=male+headers&t=ffab&atb=v100-7&iax=images&ia=images&iai=http%3A%2F%2Fktechnics.com%2Fwp-content%2Fuploads%2F2016%2F02%2F1x26_male_pin.jpg)
* Wires.

**Tools:**

* A soldering iron and all the soldering stuff.
* A pair of pliers and a rubber band as a makeshift
  vise to hold the LEDs in place while soldering,
  it looks like <a href="https://duckduckgo.com/?q=pliers+with+a+rubber+band&t=ffab&atb=v100-7&iax=images&ia=images&iai=http%3A%2F%2Fblog.espares.co.uk%2Fwp-content%2Fuploads%2Fsites%2F28%2F2017%2F01%2FPliers-With-A-Rubber-Band-Holding-Screws.jpg">this</a>
* Tweezers.


### Building the cube
To make the cube I made a bunch of bends and soldered the LEDs to each other, like so:

**Bend 1**

Bend both of the pins on each LED away from each other till they are horizontal,
like this:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/05/bend1.jpg" alt="bend 1" width="815" height="600" />

**Bend 2**

Then bend the anode of each LED downwards, like this:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/05/bend2.jpg" alt="bend 2" width="620" height="600" />

**Bend 3**

Then bend the cathode of 18 of the LEDs 90 degrees from where it was facing, so the
anode and cathode make a 90 degree angle between them:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/05/bend3.jpg" alt="bend 3" width="721" height="600" />

leave the remaining 9 LEDs the way they are.

Once you've bent the pins on all the LEDs sort them into groups of 3 with 2 LEDs
in each group being bent 90 degrees and the other one being from the 9 LEDs you
put aside.

**Soldering step 1**

Solder the cathodes of all the LEDs from a group in a line, like this:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/05/solder1.jpg" alt="solder 1" width="831" height="600" />

Do this to all the groups of LEDs.

**Soldering step 2**

Take 3 of the structures you built in the previous step and solder the anodes to
each other like this (I have no idea how to explain these things, just look at the picture):

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/05/single_layer.png" alt="solder 2" width="831" height="700" />

Now you will have three "layers" of LEDs like the one shown above, the connections of
the anodes and cathodes is shown here:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/05/led_connections.png" alt="connections" width="831" height="700" />

The lines in blue are the connections of the cathodes and the lines in red are
the connections of the anodes.

**Soldering step 3**

Here is where I made a mistake and realized that connecting the "layers" wont be
neat, so if you find a better way do it like that.

But anyway the idea is to solder all the cathodes of the LEDs in a horizontal plane
to each other:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/05/solder3.jpg" alt="solder 3" width="600" height="610" />

Now the connections of the anodes and cathodes looks like this:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/05/multiplexing2.jpg" alt="connections cube" width="632" height="610" />

The cube now has 3 planes of cathodes (blue) and 9 columns of anodes (red).

**Soldering step 4**

Now that we have a cube lets make it a shield, to do that send the anodes of the LEDs the bottom of the cube through the holes in the perf board:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/05/solder4.jpg" alt="connections cube" width="600" height="713" />

Also solder on the male breakaway headers onto the perf board such that they fit
into the pins of the Arduino.

**Soldering step 5**

Using the chart and diagram from the Wiring section, solder wires from the columns
of anodes on the cube to the pins on the shield.
Also solder the three 2k resistors to each plane of cathodes and connect the resistors
to the pins shown in the wiring diagram.

### Wiring

| Arduino        | The LED cube   |
| :------------- | :------------- |
| Digital pin7   | Plane 1        |
| Digital pin6   | Plane 2        |
| Digital pin5   | Plane 3        |
| Digital pin4   | Column 1       |
| Digital pin3   | Column 2       |
| Digital pin2   | Column 3       |
| Analog pin0    | Column 4       |
| Analog pin1    | Column 5       |
| Analog pin2    | Column 6       |
| Analog pin3    | Column 7       |
| Analog pin4    | Column 8       |
| Analog pin5    | Column 9       |


**Diagram:**

<img class="aligncenter wp-image-110 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/05/led_cube_wiring.png" alt="diagram" width="861" height="600" />

So as shown in the diagram the cube has 27 LEDs in a configuration of 9 vertical
columns by 3 horizontal planes.
To drive all those LEDs I used multiplexing (like everyone else), so as you know
all the LEDs on a plane have a common ground pin (cathode), and all the LEDs
in a column have a common power pin (anode).
This gives me 3 ground pins and 9 power pins, so to turn on an LED you just give
it's column power and pull it's plane to ground.


### The code

For the sake of simplicity the code below just turns on all the LEDs in the cube

```
/*
 This code turns on all the LEDs on a 3x3x3 LED cube.

 I'm using multiplexing to drive all the 27 LEDs this cube uses, so:

 * Each vertical column of LEDs has a common anode.
 * Each horizontal plane of LEDs has a common cathode.

 This way if I connect column 1 to power and plane 1 to ground
 the LED at 1,0,0 turns on(read: top corner)

 I'm using digital pins 2 to 7 and analog pins 0 to 5 since the spacing
 of the holes on my perf board doesn't allow me to use pins 8 to 13.
*/

// the planes of LEDs
const int plane1 = 7;
const int plane2 = 6;
const int plane3 = 5;

// the columns of LEDs
const int c1 = 4;
const int c2 = 3;
const int c3 = 2;
const int c4 = A0;
const int c5 = A1;
const int c6 = A2;
const int c7 = A3;
const int c8 = A4;
const int c9 = A5;


void setup() {
	// set everything as an output
	pinMode(plane1, OUTPUT);
	pinMode(plane2, OUTPUT);
	pinMode(plane3, OUTPUT);

	pinMode(c1, OUTPUT);
	pinMode(c2, OUTPUT);
	pinMode(c3, OUTPUT);
	pinMode(c4, OUTPUT);
	pinMode(c5, OUTPUT);
	pinMode(c6, OUTPUT);
	pinMode(c7, OUTPUT);
	pinMode(c8, OUTPUT);
	pinMode(c9, OUTPUT);

	// set the plane pins as 'HIGH' first this stops a weird effect where they
	// all go 'LOW' by default and all the LEDs turn on.
	digitalWrite(plane1, HIGH);
	digitalWrite(plane2, HIGH);
	digitalWrite(plane3, HIGH);
}

/*
Turn on all the LEDs in the cube
*/

void loop() {
	// pull all the planes to ground.
	digitalWrite(plane1, LOW);
	digitalWrite(plane2, LOW);
	digitalWrite(plane3, LOW);

	// power all the columns.
	digitalWrite(c1, HIGH);
	digitalWrite(c2, HIGH);
	digitalWrite(c3, HIGH);
	digitalWrite(c6, HIGH);
	digitalWrite(c9, HIGH);
	digitalWrite(c8, HIGH);
	digitalWrite(c7, HIGH);
	digitalWrite(c4, HIGH);
	digitalWrite(c5, HIGH);
}
```

At the bottom of the setup function I make all the planes go "HIGH", this is done
because the pins default to "LOW" and so the all the LEDs in the cube light up
the moment you power on the Arduino. Making them go HIGH turns them off.
```
digitalWrite(plane1, HIGH);
digitalWrite(plane2, HIGH);
digitalWrite(plane3, HIGH);
```
Then in loop we start by pulling the planes to ground:
```
// pull all the planes to ground.
digitalWrite(plane1, LOW);
digitalWrite(plane2, LOW);
digitalWrite(plane3, LOW);
```

And then giving power to all the columns:
```
// power all the columns.
digitalWrite(c1, HIGH);
digitalWrite(c2, HIGH);
digitalWrite(c3, HIGH);
digitalWrite(c6, HIGH);
digitalWrite(c9, HIGH);
digitalWrite(c8, HIGH);
digitalWrite(c7, HIGH);
digitalWrite(c4, HIGH);
digitalWrite(c5, HIGH);
```


##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/sit-on-cushions-we-must/led_cube/blob/master/LICENSE)
