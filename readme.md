Pine64 Touch Screen with TSLIB driver for X11 Desktops
======================================================

This is TSLIB driver (deb packages) to work on any X11 Desktop Environment, so you are able to use 
your touch screen on X11 desktop environment.

This driver does not need calibration since goodix return the touch coordinates in ABS mode.
There is also no need to use evdev driver or xinput.

This driver has been tested on Ubuntu Xenial 16.04 running X11, not sure if will run on Unity
or anything that does not run X11 directly, if you can test it and let me know, would be great.

TSLIB and X11
![tslib/X11](https://github.com/avafinger/pine64-touchscreen/raw/master/img/pine64-ts.png)


Pre-requisites
--------------

- X11
- libc6 (>= 2.17). (deb package will tell you!)
- Modern distro
- xorg.conf with "InputClass" to load TSLIB driver


Source code
-----------

	- https://launchpad.net/ubuntu/+source/xf86-input-tslib
	- https://launchpad.net/ubuntu/+source/tslib/1.0-12


Installation
------------

	- Remove any attempt to load evdev or xinput method or this will fail.
	- Make sure you have assemled the TS screen correct and LCD is working.
	- (to be finished)...


*** WIP ***

History Log:
===========
* initial commit (readme file) ** WIP **
