The 'Ping' class constructs an object that represents a single sonar ping sensor.

## Parameters

* **pin** A Number or String address for the ping sensor

## Usage

## API

## Events

* change The "change" event is emitted whenever the distance detected by the sensor changes more then the threshold value allows. 

## Setup

Ping requires a specific version of firmata to be loaded onto the Arduino in order to work.

	# Get yourself into this directory...
	cd /Applications/Arduino.app/Contents/Resources/Java/libraries/

	# make a backup of the existing Firmata
	cp -r Firmata Firmata_stable

	# rm Firmata
	rm -r Firmata

	# clone the experimental repo branch
	git clone git://github.com/jgautier/arduino-1.git Firmata

	# enter!
	cd Firmata

	# get the branch with pulseIn
	git checkout -b pulseIn origin/pulseIn


## Examples
* [Ping](https://github.com/rwldrn/johnny-five/blob/master/docs/ping.md)