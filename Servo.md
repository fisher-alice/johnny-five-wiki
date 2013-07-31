# Servo

The `Servo` class constructs objects that represent a single Servo attached to the physical board. This class is designed to work well with both Standard and Continous motion servos.

### Parameters

- **pin** A Number or String address for the Servo pin (PWM).
```js
var digital = new five.Servo(11);
```
Tinkerkit: 
```js
// Attached to "Output 0"
var digital = new five.Servo("O0");
```


- **options** An object of property parameters.
<table>
  <thead>
    <tr>
      <th>Property Name</th>
      <th>Description</th>
      <th>Required</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>pin</td>
      <td>The String address of the pin the servo is attached to, ie. 11 or "O1"</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>range</td>
      <td>The range of motion in degrees. Defaults to [0, 180]</td>
      <td>no</td>
    </tr>
    <tr>
      <td>type</td>
      <td>The type of servo, one of "standard" or "continuous". Defaults to "standard"</td>
      <td>no</td>
    </tr>
    <tr>
      <td>specs</td>
      <td>An object of datasheet specs. Not yet defined.</td>
      <td>no</td>
    </tr>
  </tbody>
</table>


### Shape

```
{ 
  board: A reference to the board object the Led is attached to
  id: A user definable id value. Defaults to a generated uid
  pin: The pin address that the Led is attached to
  mode: The Pin.MODE, Defaults to SERVO
  range: The range of motion in degrees. Defaults to [0, 180]
  history: An array containing records of each movement 
  interval: A reference to the current interval, if one exists
  isMoving: A boolean flag, true when moving, false when still
  last: The last movement record. READONLY
}
```



### Usage

Standard Servo
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var servo = new five.Servo(11);

  // Sweeo from 0-180 and repeat.
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

- **move(degrees 0-180 [, lapse])** Move a servo horn to specified position in degrees, 0-180 (or whatever the current valid range is). Support for a lapse param is reserved.
```js
var servo = new five.Servo(11);

// Set the horn to 90degrees
servo.move(90);
```

- **to(degrees 0-180 [, lapse])** Alias to _move_
```js
var servo = new five.Servo(11);

// Set the horn to 90degrees
servo.to(90);
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

- **center()** Sweep the servo between min and max or provided range
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

