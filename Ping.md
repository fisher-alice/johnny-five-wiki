The 'Ping' class constructs an object that represents a single sonar ping sensor.

**It is absolutely REQUIRED to flash your board with a special version of StandardFirmata. Instructions are [here](https://github.com/rwaldron/johnny-five/wiki/Ping#setup)**

### Parameters

- **pin** A Number or String address for the ping sensor

- **options** An object of property parameters.
<table>
  <thead>
    <tr>
      <th>Property Name</th>
      <th>Type</th>
      <th>Value(s)</th>
      <th>Description</th>
      <th>Required</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>pin</td>
      <td>Number, String</td>
      <td>9, "I1" (Any digital pin on board)</td>
      <td>The Number or String address of the pin the sensor is attached to, ie. 7</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>freq</td>
      <td>Number</td>
      <td>Milliseconds</td>
      <td>The frequency in ms of `data` events. Defaults to 100ms</td>
      <td>no</td>
    </tr>
    <tr>
      <td>pulse</td>
      <td>Number</td>
      <td>Milliseconds</td>
      <td>The frequency in ms to wait for a pulse. Defaults to 500ms</td>
      <td>no</td>
    </tr>

  </tbody>
</table>
```js
// Create a Ping sensor...
// 
//   - attached to pin 7
//   - emits data events every 250ms
//
var ping = new five.Ping({
  pin: 7, 
  freq: 250
});
```

### Shape

```js
{
  pin: The pin address that the Ping Sensor is attached to.
  freq: The frequency of read events.
  pulse: The frequency of pulses.

  inches: The detected distance in inches. READONLY
  cm: The detected distance in centimetres. READONLY
}
```
## Usage

```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {

  //Create new Ping and show distance on change
  var ping = new five.Ping(7);

  ping.on("change", function() {
    console.log("Object is " + this.cm + " cm away");
    console.log("Object is " + this.inches + " inches away");
  });
});  
```
## API

Ping does not have any API functions

### Events

- **change** The "change" event is emitted whenever the distance detected by the sensor changes more then the threshold value allows. 

- **data** The "data" event is fired as frequently as the user defined freq will allow in milliseconds. ("data" replaced the "read" event)

## Setup

Ping requires a specific version of firmata to be loaded onto the Arduino in order to work.

If you have the Arduino IDE open, you should close it before you start.

First get to the following directory with your Terminal or Git command line:

**OS X:**
`/Applications/Arduino.app/Contents/Resources/Java/libraries/`

**Linux:**
`/usr/share/arduino/libraries/`

**Windows:**
`C:\Program Files (x86)\Arduino\libraries\`

Then proceed with the following in your command line:
```bash

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
```
Now open the Arduino IDE.

- File > Examples > Firmata > Standard Firmata

- Once that's loaded into the IDE, confirm it is the correct program by searching for "PULSE_IN".

- You should now be able to Compile and upload this version of Standard Firmata to your Arduino.

If you receive an error about 'PULSE_IN' not being in scope, then find the following line:

```c
#define REGISTER_NOT_SPECIFIED -1
```

And below it, add this:

```c
#define PULSE_IN                0x74 // send a pulse in command
```

So that it reads:

```c
#define REGISTER_NOT_SPECIFIED -1
#define PULSE_IN                0x74 // send a pulse in command
```
## Examples
* [Ping](https://github.com/rwldrn/johnny-five/blob/master/docs/ping.md)
* [Hc Sr04](https://github.com/rwaldron/johnny-five/blob/master/docs/hc-sr04.md)