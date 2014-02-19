The `ESC` class constructs objects that represent a single ESC attached to the physical board. `ESC` objects are similar to `Servo` objects as they both use PWM pins to communicate with the physical ESC with values between 0 and 180. Currently, this class assumes the ESC is calibrated.

<img src="https://raw.github.com/rwaldron/johnny-five/master/docs/breadboard/esc-keypress.png">

### Parameters

- **pin** A Number or String address for the ESC pin (PWM).
```js
var esc = new five.ESC(12);
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
      <td>The range of motion in speed. Defaults to [0, 180]</td>
      <td>no</td>
    </tr>
    <tr>
      <td>pwmRange</td>
      <td>Array</td>
      <td>[ lower, upper ]</td>
      <td>The range of motion in speed. Defaults to [600, 2400]. Overrides `range`</td>
      <td>no</td>
    </tr>
    <tr>
      <td>startAt</td>
      <td>Number</td>
      <td>Any fractional number between 0...1</td>
      <td>Initial speed</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
```js
// Create an esc...
// 
//   - attached to pin 12
//   - limited speed range of 45-135
//
var esc = new five.ESC({
  pin: 12, 
  range: [ 45, 135 ]
});
```

### Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pin: The pin address that the ESC is attached to
  range: The range of motion in speed. Defaults to [0, 180]
  history: An array containing records of each movement 
  interval: A reference to the current interval, if one exists
  last: The last movement record. READONLY
  speed: The last/current speed. READONLY
}
```



### Usage

Standard ESC
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var esc = new five.ESC(12);

  // Set to top speed. (this can be physically dangerous, you've been warned.)
  esc.max();

});
```

## API

- **to(speed)** Set the speed of the ESC, 0...1 as a fractional value. If the specified speed is the same as the current speed, no commands are sent.
```js
var esc = new five.ESC(12);

// Set the motor's speed to half
esc.to(0.50);
```

- **min()** Set ESC to minimum speed. Defaults to 0, respects explicit range.
```js
var esc = new five.ESC(12);

esc.min();
```
Or 
```js
var esc = new five.ESC({
  pin: 12, 
  pwmRange: [ 1000, 1900 ]
});

esc.min();
```

- **max()** Set ESC to maximum speed. Defaults to 1, respects explicit range.
```js
var esc = new five.ESC(12);

esc.max();
```
Or 
```js
var esc = new five.ESC({
  pin: 12, 
  range: [ 1000, 1900 ]
});

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
