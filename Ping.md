The 'Ping' class constructs an object that represents a single sonar ping sensor.
### Parameters

* **pin** A Number or String address for the ping sensor

### Usage

    //Create new Ping and show distance on change
    var ping = new five.Ping(7);
        ping.on("change", function( err, value ){

            console.log('Object is ' + this.cm + ' cm away');
            console.log('Object is ' + this.inches + ' inches away');

        });
    });    
      
### API
### Events

* change The "change" event is emitted whenever the distance detected by the sensor changes more then the threshold value allows. 

### Setup

Ping requires a specific version of firmata to be loaded onto the Arduino in order to work.

If you have the Arduino IDE open, you should close it before you start.

**Mac:**

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

Now open the Arduino IDE.

File > Examples > Firmata > Standard Firmata

Once that's loaded into the IDE, confirm it is the correct program by searching for "PULSE_IN".

You should now be able to Compile and upload this version of Standard Firmata to your Arduino.

If you receive an error about PULSE_IN not being in scope, then copy this:

    #define PULSE_IN                0x74 // send a pulse in command

And add it below the following line:

    #define REGISTER_NOT_SPECIFIED -1

### Examples
* [Ping](https://github.com/rwldrn/johnny-five/blob/master/docs/ping.md)