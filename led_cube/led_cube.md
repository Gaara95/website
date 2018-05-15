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

Bend both of the pins on each LED away from each other til they are horizontal,
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
to each other

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

### The code
```

```

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
