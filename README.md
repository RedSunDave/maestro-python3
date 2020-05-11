# Maestro Board with Python 3

## Summary

The following information outlines all the necessary steps in order to configure linux and manipulate pololu maestro with Python

### Section 1: Pololu Maestro USB Servo Controller Linux Software

    This section explains how to setup linux to work with pololu maestro

### Section 2: maestro.py - Python3 Support for Pololu Maestro

    This section explains how to use python with the Pololu Maestro

### *** BEGIN DOCUMENTATION ***

# Section 1: Pololu Maestro USB Servo Controller Linux Software

## This reference contains information from Pololu's Official Website

Release Date: 2015-01-16
http://www.pololu.com/

## Summary

The following section describes actions required to use pololu maestro in conjunction with Linux

## Prerequisites

You will need to download and install these packages:

    libusb-1.0-0-dev mono-runtime mono-reference-assemblies-2.0 mono-devel

In Ubuntu, you can do this with the command:

    sudo apt-get install libusb-1.0-0-dev mono-runtime mono-reference-assemblies-2.0 mono-devel

## USB Configuration

You will need to copy the file 99-pololu.rules to /etc/udev/rules.d/

    sudo cp 99-pololu.rules /etc/udev/rules.d/

in order to grant permission for all users to use Pololu USB devices.
Then, run

  sudo udevadm control --reload-rules

to make sure the rules get reloaded.  If you already plugged in
a Pololu USB device, you should unplug it at this point so the new
permissions will get applied later when you plug it back in.

## Running the programs

You can run the programs by typing one of the following commands:

   ./MaestroControlCenter
   ./UscCmd

## Make sure to run ./MaestroControlCenter go to the "Serial Settings" tab and change the "Serial Mode" to "USB DUAL PORT"

## The Board will not recieve commands if you do not change this.

## *** End Section 1 ***

# Section 2: maestro.py - Python3 Support for Pololu Maestro

Python Support for the Pololu Maestro line of servo controllers. The original 
code was written by Steven Jacobs (Aug 2013) and located at- https://github.com/FRC4564/Maestro/
These functions provide access to many of the Maestro's capabilities using the
Pololu serial protocol

Code contained in this repository was edited by Dave Foran -- Jan 2020 to fit 
into Python3 and PEP 8 Conventions. I felt it was important to update this file 
in order to bring the original code up to standard since Python2 is discontinued. 
It was also good practice for reformatting Python2 code to Python3 code.

## Getting Started / Setup

### Setting Up Pololu Maestro

To get started, you will need to install the Maestro Control Center either on Windows (https://www.pololu.com/docs/0J40/3.a) or of course, my personal favorite, Linux (https://www.pololu.com/docs/0J40/3.b)

Pololu's Maestro Windows installer sets up the Maestro Control Center, used to configure, test and program the controller.  Be sure the Maestro is configured for "USB Dual Port" serial mode, which is [not the default](https://www.pololu.com/docs/0J40/3.c).

Once you've downloaded and unzipped the package on Linux, plug in your Pololu Maestro
to your computer via USB cable and run:

    cd /maestro-linux-150116/maestro-linux/

    ./MaestroControlCenter

This will allow you to bring up the control center. Check your controller, if there is a red light, you can use "Device" and then "Restart Device", to ensure that your connection to the controller is initializing correctly. Finally, go to "Serial Settings" and under "Serial Mode:" select "USB Dual Port".

Now you're ready to start programming.

### Setting Up Python Environment

As usual, I recommend you first set up a Virtual Environment in your main folder and enter into it with a set of commands like:

    python3 -m venv .venv

    source .venv/bin/activate

You'll need to have the 'pyserial' Python module installed to use maestro.py.

Using pip3 for python3

    python3 -m pip install pyserial

Finally, download the maestro.py into your directory from which you are going to be running the python scripts from. Now you are ready to begin writing code.

## Example Script

Example usage of maestro.py:

    import maestro
    servo = maestro.Controller()
    servo.set_acceleration(0,4)   #set servo 0 acceleration to 4
    servo.set_target(0,6000)      #set servo to move to center position
    servo.set_speed(1,10)         #set speed of servo 1
    x = servo.get_position(1)     #get the current position of servo 1
    servo.close()

There are other methods provided by the module. The code is well documented, if you'd like to learn more.

For use on Windows, you'll need to provide the COM port assigned to the Maestro Command Port. You can identify the port by starting Device Manager and looking under Ports (COM & LPT). Here's how to instantiate the controller for Windows for COM port 3.

    import maestro
    m = maestro.Controller('COM3')

## Permission issue

If you find that Linux complains about permissions trying to access the ttyACM device, just add your user to the 'dialout' group by issuing the following:

    sudo adduser $USER dialout

You'll need to reboot for the change to take effect.

## Authors

* **Steven Jacobs** - *maestro.py* - [FRC4564](https://github.com/FRC4564)

* **Dave Foran** - *Refactored to PEP8 Standards* - [RedSunDave](https://github.com/RedSunDave)

## Acknowledgments

* Hat tip to Steven Jacobs for his excellent maestro code, I love using it and it works really well with the pololu maestro controller

* Please reach out to me with any questions that you have at dave@redsunis.com, I am always excited to see projects you are working on! If you have any recommendations on improving the code or formatting, please let me know as well.
