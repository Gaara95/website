
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

##### Licensing:

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

All the code in this tutorial is licensed under the MIT license, the exact terms for which can be found [here](https://github.com/afshaan4/other_arduino_projects/blob/master/LICENSE)
