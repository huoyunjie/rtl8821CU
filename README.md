# rtl8821CU
[![Build Status](https://travis-ci.org/whitebatman2/rtl8821CU.svg?branch=master)](https://travis-ci.org/whitebatman2/rtl8821CU)

Drivers for rtl8811CU and rtl8821CU Wi-Fi chipsets. This repository is based on soruce code found on a CD shipped with a rtl8811CU based card. It's updated to build on newer kernel versions.

## Build and install with DKMS

DKMS is a system which will automatically recompile and install a kernel module when a new kernel gets installed or updated. To make use of DKMS, install the dkms package, which on Debian (based) systems is done like this:

    apt-get install dkms

To make use of the DKMS feature with this project, do the following:

    DRV_NAME=rtl8821CU
    DRV_VERSION=5.2.5.3
    sudo mkdir /usr/src/${DRV_NAME}-${DRV_VERSION}
    git archive master | sudo tar -x -C /usr/src/${DRV_NAME}-${DRV_VERSION}
    sudo dkms add -m ${DRV_NAME} -v ${DRV_VERSION}
    sudo dkms build -m ${DRV_NAME} -v ${DRV_VERSION}
    sudo dkms install -m ${DRV_NAME} -v ${DRV_VERSION}

If you later on want to remove it again, do the following:

    DRV_NAME=rtl8821CU
    DRV_VERSION=5.2.5.3
    sudo dkms remove ${DRV_NAME}/${DRV_VERSION} --all

## Build and install without DKMS
Use following commands in source directory:
```
make
sudo make install
sudo modprobe 8821cu
```
## Raspberry Pi
To build this driver on Raspberry Pi you need to set correct platform in Makefile.
Change
```
CONFIG_PLATFORM_I386_PC = y
CONFIG_PLATFORM_ARM_RPI = n
CONFIG_PLATFORM_ARM_RPI3 = n
```
to
```
CONFIG_PLATFORM_I386_PC = n
CONFIG_PLATFORM_ARM_RPI = y
CONFIG_PLATFORM_ARM_RPI3 = n
```
For the Raspberry Pi 3 you need to change it to
```
CONFIG_PLATFORM_I386_PC = n
CONFIG_PLATFORM_ARM_RPI = n
CONFIG_PLATFORM_ARM_RPI3 = y
```
## Other operation
>>lsusb
'''
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 004: ID 0b05:1872 ASUSTek Computer, Inc. 
Bus 001 Device 003: ID 046d:c534 Logitech, Inc. Unifying Receiver
Bus 001 Device 006: ID 0bda:1a2b Realtek Semiconductor Corp. 
Bus 001 Device 005: ID 24ae:4025  
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hu
'''
>>sudo usb_modeswitch -KW -v 0bda -p 1a2b
'''
Take all parameters from the command line


 * usb_modeswitch: handle USB devices with multiple modes
 * Version 2.5.2 (C) Josua Dietze 2017
 * Based on libusb1/libusbx

 ! PLEASE REPORT NEW CONFIGURATIONS !

DefaultVendor=  0x0bda
DefaultProduct= 0x1a2b

StandardEject=1

Look for default devices ...
  found USB ID 1d6b:0003
  found USB ID 1d6b:0002
  found USB ID 1d6b:0003
  found USB ID 0b05:1872
  found USB ID 046d:c534
  found USB ID 0bda:1a2b
   vendor ID matched
   product ID matched
  found USB ID 24ae:4025
  found USB ID 1d6b:0002
 Found devices in default mode (1)
Access device 006 on bus 001
Get the current device configuration ...
Current configuration number is 1
Use interface number 0
 with class 8
Use endpoints 0x0b (out) and 0x8a (in)

USB description data (for identification)
-------------------------
Manufacturer: Realtek
     Product: DISK
  Serial No.: not provided
-------------------------
Sending standard EJECT sequence
Looking for active drivers ...
 OK, driver detached
Set up interface 0
Use endpoint 0x0b for message sending ...
Trying to send message 1 to endpoint 0x0b ...
 OK, message successfully sent
Read the response to message 1 (CSW) ...
 Response successfully read (13 bytes), status 1
Trying to send message 2 to endpoint 0x0b ...
 OK, message successfully sent
Read the response to message 2 (CSW) ...
 Response successfully read (13 bytes), status 0
Trying to send message 3 to endpoint 0x0b ...
 Sending the message returned error -1. Try to continue
Read the response to message 3 (CSW) ...
 Device seems to have vanished after reading. Good.
 Device is gone, skip any further commands
-> Run lsusb to note any changes. Bye!
'''
