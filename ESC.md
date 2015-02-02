The `ESC` class constructs objects that represent a single ESC attached to the physical board. `ESC` objects are similar to `Servo` objects as they both use PWM pins to communicate with the physical ESC. Currently, this class assumes the ESC is [pre-calibrated](#wiki-calibrating-an-esc).

<img src="https://raw.github.com/rwaldron/johnny-five/master/docs/breadboard/esc-keypress.png">

## Parameters

- **pin** A Number or String address for the ESC pin (PWM).
```js
var esc = new five.ESC(9);
```
Tinkerkit: 
```js
// Attached to "Output 0"
var esc = new five.ESC("O0");
```


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
      <td>Any PWM Pin</td>
      <td>The address of the pin the esc is attached to, ie. 12 or "O1" (if using TinkerKit)</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>range</td>
      <td>Array</td>
      <td>[ lower, upper ]</td>
      <td>The range of speed in percent. Defaults to [0, 100]</td>
      <td>no</td>
    </tr>
    <tr>
      <td>startAt</td>
      <td>Number</td>
      <td>Any fractional number value from 0-100</td>
      <td>Initial speed percentage</td>
      <td>no</td>
    </tr>
  </tbody>
</table>


## Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pin: The pin address that the ESC is attached to
  range: The range of speed as an array of fractional percent values. Defaults to [0, 100]
  value: The value of the last/current speed. READONLY
}
```

## Controller Initialization

```js
// Create an esc...
// 
//   - attached to pin 12
//   - limited speed range to 0-80%
//
var esc = new five.ESC({
  pin: 12, 
  range: [ 0, 80 ]
});
```



## Usage

Standard ESC
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var esc = new five.ESC(11);

  // Set to top speed. (this can be physically dangerous, you've been warned.)
  esc.max();

});
```

## API

- **speed(value)** Set the speed of the ESC, any fractional number value from 0...100 as a percentage value. If the specified speed is the same as the current speed, no commands are sent.
```js
var esc = new five.ESC(11);

// Set the motor's speed to 50%
esc.to(50);
```

- **min()** Set ESC to minimum speed. Defaults to 0%, respects explicit range.
```js
var esc = new five.ESC(11);

esc.min();
```
Or 
```js
var esc = new five.ESC({
  pin: 11, 
  range: [ 10, 100 ]
});

// Minimum speed at 10%
esc.min();
```

- **max()** Set ESC to maximum speed. Defaults to 100%, respects specified range.
```js
var esc = new five.ESC(11);

esc.max();
```
Or 
```js
var esc = new five.ESC({
  pin: 11, 
  range: [ 0, 80 ]
});

// Max at 80%
esc.max();
```

- **stop()** Stop a moving esc. 
```js
var esc = new five.ESC(12);

esc.stop();
```



## Examples
- [Esc Keypress](https://github.com/rwldrn/johnny-five/blob/master/docs/esc-keypress.md)
- [Esc Dualshock](https://github.com/rwldrn/johnny-five/blob/master/docs/esc-dualshock.md)


## Calibrating an ESC

Load the following sketch onto your Arduino, via the Arduino IDE follow the instructions.

This requires that the ESC's power source is **off or disconnected** at the start of the calibration process. You will be prompted to turn on/reconnect the power during the process.

```c
#include <Servo.h>

#define MAX_SIGNAL 2000
#define MIN_SIGNAL 700
#define MOTOR_PIN 12

Servo motor;

void setup() {
  Serial.begin(9600);
  Serial.println("Program begin...");
  Serial.println("This program will calibrate the ESC.");

  motor.attach(MOTOR_PIN);

  Serial.println("Calibrating Maximum Signal");
  Serial.println("Turn on power source, then wait 2 seconds and press any key + <enter>");
  motor.writeMicroseconds(MAX_SIGNAL);

  // Wait for input
  while (!Serial.available());
  Serial.read();

  // Send min output
  Serial.println("Calibrating Minimum Signal");
  motor.writeMicroseconds(MIN_SIGNAL);

}

void loop() {  

}
```