# Octoprint-Farm
Detailed scripts and steps for creating a 3D printer farm

# How to set up a farm

Install the plug-in "M33 Fio" in Octoprint. Just search for "M33 Fio" under Settings > Plugin Manager > Get More

Create a rule that maps serial numbers only: (becasue we might change ports and hubs in the future)
````
# do not edit this file, it will be overwritten on update

ACTION=="remove", GOTO="serial_end"
SUBSYSTEM!="tty", GOTO="serial_end"

SUBSYSTEMS=="pci", ENV{ID_BUS}="pci", ENV{ID_VENDOR_ID}="$attr{vendor}", ENV{ID_MODEL_ID}="$attr{device}"
SUBSYSTEMS=="pci", IMPORT{builtin}="hwdb --subsystem=pci"
SUBSYSTEMS=="usb", IMPORT{builtin}="usb_id", IMPORT{builtin}="hwdb --subsystem=usb"

# /dev/serial/by-serial/ for USB devices
KERNEL!="ttyUSB[0-9]*|ttyACM[0-9]*", GOTO="serial_end"

SUBSYSTEMS=="usb-serial", ENV{.ID_PORT}="$attr{port_number}"

IMPORT{builtin}="usb_id"
ENV{.ID_PORT}=="", SYMLINK+="serial/by-serial/$env{ID_SERIAL}"
ENV{.ID_PORT}=="?*", SYMLINK+="serial/by-serial/$env{ID_SERIAL}"

LABEL="serial_end"
````

Go into Octoprint settings under Settings > Serial Connection > General > Additional serial ports
 and add:
 ````
/dev/serial/by-serial/*
````

Then under each instance you can map just the serial number

... TODO more to come!
