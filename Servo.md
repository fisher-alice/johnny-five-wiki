The `Servo` class constructs objects that represent a single Servo attached to the physical board. This class is designed to work well with both **Standard** and **Continuous** motion servos.

### Parameters

- **pin** A Number or String address for the Servo pin (PWM).
```js
var servo = new five.Servo(11);
```
Tinkerkit: 
```js
// Attached to "Output 0"
var servo = new five.Servo("O0");
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
      <td>The address of the pin the servo is attached to, ie. 11 or "O1" (if using TinkerKit)</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>range</td>
      <td>Array</td>
      <td>[ lower, upper ]</td>
      <td>The range of motion in degrees. Defaults to [0, 180]</td>
      <td>no</td>
    </tr>
    <tr>
      <td>type</td>
      <td>String</td>
      <td>"standard", "continuous"</td>
      <td>The type of servo being created. Defaults to "standard"</td>
      <td>no</td>
    </tr>
    <tr>
      <td>startAt</td>
      <td>Number</td>
      <td>Any number between 0-180</td>
      <td>Degrees to initialize the servo at.</td>
      <td>no</td>
    </tr>
    <tr>
      <td>center</td>
      <td>Boolean</td>
      <td>true or false</td>
      <td>Optionally center the servo on initialization. Defaults to `false`</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
```js
// Create a standard servo...
// 
//   - attached to pin 12
//   - limited range of 45-135 degrees
//
var temp = new five.Servo({
  pin: 12,
  range: [45, 135]
});
```

### Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pin: The pin address that the Servo is attached to
  range: The range of motion in degrees. Defaults to [0, 180]
  history: An array containing records of each movement 
  interval: A reference to the current interval, if one exists
  isMoving: A boolean flag, true when moving, false when still
  last: The last movement record. READONLY
  position: The last/current position. READONLY
}
```



### Usage

Standard Servo
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var servo = new five.Servo(11);

  // Sweep from 0-180 and repeat.
  servo.sweep();
});
```

Continuous Servo
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var servo = new five.Servo({
    pin: 11, 
    type: "continuous"
  });

  // Clockwise, top speed.
  servo.cw(1);
});
```


## API

- **to(degrees 0-180 [, ms [, rate]])** Move a servo horn to specified position in degrees, 0-180 (or whatever the current valid range is). If `ms` is specified, the servo will take that amount of time to move to the position. If `rate` is specified, the angle change will be split into distance/rate steps for the `ms` option. If the specified angle is the same as the current angle, no commands are sent.
```js
var servo = new five.Servo(11);

// Set the horn to 90degrees
servo.to(90);

// Angle change takes 500ms to complete
servo.to(90, 500);

// Angle change takes 500ms to complete over 10 steps
servo.to(90, 500, 10);
```

- **min()** Set Servo to minimum degrees. Defaults to 0deg, respects explicit range.
```js
var servo = new five.Servo(11);

// Set horn to 0degrees
servo.min();
```
Or 
```js
var servo = new five.Servo({
  pin: 11, 
  range: [ 45, 135 ]
});

// Set horn to 45degrees
servo.min();
```

- **max()** Set Servo to maximum degrees. Defaults to 180deg, respects explicit range.
```js
var servo = new five.Servo(11);

// Set horn to 135degrees
servo.max();
```
Or 
```js
var servo = new five.Servo({
  pin: 11, 
  range: [ 45, 135 ]
});

// Set horn to 135degrees
servo.max();
```


- **center()** Set Servo to center point. Defaults to 90deg, respects explicit range.
```js
var servo = new five.Servo(11);

// Set horn to 90degrees
servo.center();
```
Or 
```js
var servo = new five.Servo({
  pin: 11, 
  range: [ 40, 80 ]
});

// Set horn to 60degrees
servo.center();
```

- **sweep()** Sweep the servo between min and max or provided range
```js
var servo = new five.Servo(11);

// Repeated full range movement
servo.sweep();
```

- **stop()** Stop a moving servo. 
```js
var servo = new five.Servo(11);

servo.stop();
```


- **cw(speed)** Move a **continuous** servo clockwise at speed, 0-1
```js
var servo = new five.Servo({
  pin: 11, 
  type: "continuous"
});

servo.cw(1);
```

- **ccw(speed)** Move a **continuous** servo counter clockwise at speed, 0-1
```js
var servo = new five.Servo({
  pin: 11, 
  type: "continuous"
});

servo.ccw(1);
```

## Events

It's recommended to use a rotary potentiometer as the mechanism for determining servo movement.

## Examples
- [Servo](https://github.com/rwldrn/johnny-five/blob/master/docs/servo.md)
- [Servo Options](https://github.com/rwldrn/johnny-five/blob/master/docs/servo-options.md)
- [Servo Array](https://github.com/rwldrn/johnny-five/blob/master/docs/servo-array.md)
- [Servo Digital](https://github.com/rwldrn/johnny-five/blob/master/docs/servo-digital.md)
- [Servo Dual](https://github.com/rwldrn/johnny-five/blob/master/docs/servo-dual.md)
- [Servo Tutorial](https://github.com/rwldrn/johnny-five/blob/master/docs/servo-tutorial.md)
- [Continuous Clock](https://github.com/rwldrn/johnny-five/blob/master/docs/continuous-clock.md)
- [Continuous](https://github.com/rwldrn/johnny-five/blob/master/docs/continuous.md)


## Additional Information

Although servos can run on digital pins, this can sometimes cause issues. For this reason servos are forced to use only PWM under debug mode and will emit an error if used on a digital pin.

**Servo debug option:**
<table>
  <thead>
    <thead>
      <tr>
        <th>Property Name</th>
        <th>Type</th>
        <th>Value(s)</th>
        <th>Description</th>
        <th>Required</th>
      </tr>
    </thead>
    <tr>
      <td>debug</td>
      <td>Boolean</td>
      <td>true or false</td>
      <td>Set servo to debug mode</td>
      <td>no</td>
    </tr>
  </tbody>
</table>