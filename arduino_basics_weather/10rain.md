## Rain sensors
We want the weather we are going to build to be able to tell if it is raining or not,
for that we will use a "rain sensor" this is basically a pad with metal traces on
it, when water falls on it current flows through the metal traces.

The pad with the metal traces is connected to a board which is used to tune the
sensitivity of the sensor and change whether it sends a *HIGH* or *LOW* when it
detects rain, this board has a *AO* pin and a *DO* pin, these are for analog out
and digital out, since we want to see if there is rain or not we connect the DO
pin to the Arduino and use *digitalRead()* to see if there is rain or not.

The sensor:

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/" alt="" width="600" height="783" />

**Parts:**

* An Arduino.
* A rain sensor.
* A breadboard(optional).

### Wiring

Arduino    |    rain sensor
-----------|---------------
5v PWR     |    VIN
GND        |    GND
Pin8       |    DO

**Diagram:**

<img class="aligncenter wp-image-147 size-full" src="https://aaalearn.mystagingwebsite.com/wp-content/uploads/" alt="" width="600" height="783" />
