For this segment we will be building a device that beeps a buzzer when something
comes near it, it is going to be designed to be used by people who can't see.
You can have the device designed however you want, it can be a stick, a cap, or
whatever else.

I made mine cap mounted: it has 3 ultrasonic sensors one facing forward one to
the left and the other to the right, and 3 buzzers one in front one on the back
and one on the right, this way the user can tell where the obstacle is.

**Parts:**

* An Arduino.
* 3 ultrasonic sensors.
* 3 buzzers.
* A whole lot of wires.

### Wiring

| Arduino        | ultrasonic sensors |
| :------------- | :-------------     |
| pin12          | Trigger sensor 1   |
| pin11          | Echo sensor 1      |
| pin10          | Trigger sensor 2   |
| pin9           | Echo sensor 2      |
| pin8           | Trigger sensor 3   |
| pin7           | Echo sensor 3      |
| GND            | GND                |
| 5v             | PWR                |

| Arduino        | Buzzers        |
| :------------- | :------------- |
| pin5           | buzzer1        |
| pin3           | buzzer2        |
| pin7           | buzzer3        |
| GND            | GND            |
| 5v             | PWR            |

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/2018/04/walking_assist.png" alt="motion" width="600" height="600" />

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
