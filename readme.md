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


How does it work
----------------

Modern distro does not have xorg.conf by default but it looks for a xorg.conf file in the /etc/X11/
 and if found  it loads and parses the contents and apply the changes in there.
If you write wrong information in xorg.conf or ask for some unavailable driver your desktop will 
most likely not be displayed and will drop to a command line.
The X11 will use TSLIB driver for the touch input and translate/convert the coordinates
to an approximate value if it finds the file /etc/pointercal with some values in it.

ts_calibrate populate the file /etc/pointercal with some values used to do a matrix transformation
from the coordinates returned by the driver (goodix) and guess an approximate value, but
Capacitive Touch screen passes the touch coordinates in ABS values, if you touch the center of the LCD 
screen for example you get the real coordinate like: (512,400), if TSLIB finds /etc/pointercal it tries to
convert this (x,y) point to an approximate value and will end with something like: (500,380), for
this reason we will not **calibrate** the touch, we want to get the real coordinate values from TSLIB.
In this case, we DON'T do a ts_calibrate, and our touch will work like a charm for X11.


	**IMPORTANT**

	You can always restore your previous working conf for your desktop, so make a backup of any files you change


Installation
------------

### Make sure you don't mix *evdev* driver, xinput and TSLIB


    a.  **If you tried to make touch work previoulsy, make sure you:**

	- Remove any attempt to load evdev or xinput method or this will fail.
	- Make sure you have assemled the TS screen correct and LCD is working.
	- Check dmesg if your Touch has been recognized.


    b.  Remove any *99-touch.conf* or similar .conf that may conflit with TSLIB


    c.  make a backup of your xorg.conf or any other conf you have 
        in case something goes wrong you an restore it and work again.



### Installation process

    a.  **After you made sure you removed any possible conflit, install the deb**

	sudo dpkg -i tsconf_1.0-12_all.deb
	sudo dpkg -i libts-0.0-0_1.0-12_arm64.deb
	sudo dpkg -i libts-bin_1.0-12_arm64.deb
	sudo dpkg -i libts-dev_1.0-12_arm64.deb 



    b. **Install tslib input deb:**

     	tslib_drv-0.0.6_1.0-1.deb



    c. **Add/Edit the file xorg.conf with:**

	# Pine64 TS - no need for calibration with TSLIB
	# and no need for evdev
	Section "InputClass"
		Identifier "Pine64-Touchscreen"
	#	MatchIsTouchscreen "on"
		MatchDevicePath "/dev/input/event*"
		MatchProduct "gt9xxf_ts"
		Driver "tslib"
	#	Option "Mode" "Absolute"
	EndSection



    d. **Reboot*

	sudo reboot



    d. **Now touch the LCD screen and enjoy**



### Troubleshooting

    a. **If your Desktop does not start, look for erros in /var/log/Xorg.0.log**

	[    57.306] (**) Option "xkb_options" "lv3:ralt_switch"
	[    57.307] (II) config/udev: Adding input device sunxi-ths (/dev/input/event5)
	[    57.307] (II) No input driver specified, ignoring this device.
	[    57.308] (II) This device may have been added with another device file.
	[    57.308] (II) config/udev: Adding input device gt9xxf_ts (/dev/input/event6)
	[    57.308] (**) gt9xxf_ts: Applying InputClass "evdev touchscreen catchall"
	[    57.308] (**) gt9xxf_ts: Applying InputClass "Pine64-Touchscreen"
	[    57.308] (II) LoadModule: "tslib"
	[    57.309] (II) Loading /usr/lib/xorg/modules/input/tslib_drv.so
	[    58.768] (II) Module tslib: vendor="X.Org Foundation"
	[    58.768] 	compiled for 1.18.4, module version = 0.0.1
	[    58.768] 	Module class: X.Org XInput Driver
	[    58.768] 	ABI class: X.Org XInput driver, version 22.1
	[    58.768] (II) Using input driver 'tslib' for 'gt9xxf_ts'
	[    58.768] (**) gt9xxf_ts: always reports core events
	[    58.768] (**) gt9xxf_ts: always reports core events
	[    58.779] (**) Option "config_info" "udev:/sys/devices/virtual/input/input6/event6"
	[    58.779] (II) XINPUT: Adding extended input device "gt9xxf_ts" (type: TOUCHSCREEN, id 12)
	[    58.779] xf86TslibControlProc
	[    58.780] (**) gt9xxf_ts: (accel) keeping acceleration scheme 1
	[    58.780] (**) gt9xxf_ts: (accel) acceleration profile 0
	[    58.780] (**) gt9xxf_ts: (accel) acceleration factor: 2.000
	[    58.780] (**) gt9xxf_ts: (accel) acceleration threshold: 4
	[    58.780] xf86TslibControlProc


    b. **Make sure the driver was recognized and loaded properly:**

	[   44.198350] <<-GTP-INFO->> [1686]GTP i2c test OK.
	[   44.228529] <<-GTP-INFO->> [2195]Chip Type: GOODIX_GT9
	[   44.296530] <<-GTP-INFO->> [1459]gt9xx:index = 4
	[   44.308667] <<-GTP-INFO->> [1469]CTP_CONFIG_GROUP5 used, config length: 186
	[   44.327658] <<-GTP-INFO->> [519]Driver send config.
	[   44.343162] <<-GTP-INFO->> [1613]X_MAX: 1024, Y_MAX: 600, TRIGGER: 0x01
	[   44.365511] input: gt9xxf_ts as /devices/virtual/input/input6
	[   44.375433] <<-GTP-INFO->> [1651]IC Version: 911_1060
	[   44.385540] <<-GTP-INFO->> [196]Applied memory size:2562.
	[   44.394412] <<-GTP-INFO->> [213]Create proc entry success!


    c. **Test the driver with:**

	sudo cat /dev/input/event6 | hexdump
	[sudo] password for ubuntu: 
	0000000 5ed5 589e 0000 0000 0ccf 0006 0000 0000
	0000010 0001 014a 0001 0000 5ed5 589e 0000 0000
	0000020 0ccf 0006 0000 0000 0003 0035 02dd 0000
	0000030 5ed5 589e 0000 0000 0ccf 0006 0000 0000
	0000040 0003 0036 0155 0000 5ed5 589e 0000 0000
	0000050 0ccf 0006 0000 0000 0003 0030 0012 0000
	0000060 5ed5 589e 0000 0000 0ccf 0006 0000 0000
	0000070 0003 0032 0012 0000 5ed5 589e 0000 0000
	0000080 0ccf 0006 0000 0000 0003 0039 0000 0000
	0000090 5ed5 589e 0000 0000 0ccf 0006 0000 0000
	00000a0 0000 0002 0000 0000 5ed5 589e 0000 0000
	00000b0 0ccf 0006 0000 0000 0000 0000 0000 0000
	00000c0 5ed5 589e 0000 0000 3562 0006 0000 0000


*** WIP ***

History Log:
===========
* initial commit (readme file) ** WIP **
