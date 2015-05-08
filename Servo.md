![](http://i.gyazo.com/d3bb767e9fb54c220fd4f6391e135600.png)


The `Servo` class constructs objects that represent a single Servo attached to the physical board. This class is designed to work well with both **Standard** and **Continuous** rotation servos.

## Parameters

- **pin** A Number or String address for the Servo pin.

- **General options** An object of property parameters.

  | Property | Type           | Value/Description                                                                           | Default | Required |
  |---------------|----------------|---------------------------------------------------------------------------------------------|----------|----------|
  | pin           | Number, String | The address of the pin the servo is attached to                                             | | yes      |
  | range         | Array          | `[ lower, upper ]` The range of motion in degrees. | `[0, 180]`                   | no       |
  | type          | String         | “standard”, “continuous”. The type of servo being created. | “standard”           | no       |
  | startAt       | Number         | Any number between 0-180. Degrees to initialize the servo at.                               | no       |
  | offset        | Number        | A positive or negative value to adjust the servo. | `false`              | no       |
  | invert        | Boolean        | `true` or `false`. Optionally Invert servo movement.| `false`              | no       |
  | center        | Boolean        | `true` or `false`. Optionally center the servo on initialization. | `false` | no       |
  | controller    | String  | "DEFAULT", "PCA9685" | Controller interface type.  | `"DEFAULT"`.                                           | no       |



- **PCA9685 Options (`controller: "PCA9685"`)**

  | Property | Type           | Value/Description                                                                           | Default | Required |
  |---------------|----------------|---------------------------------------------------------------------------------------------|----------|----------|
  | address           | Number | I2C device address.                                             | `0x40` | yes      |


### Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pin: The pin address that the Servo is attached to
  range: The range of motion in degrees. Defaults to [0, 180]
  invert: A boolean, indicates whether servo values are inverted
  history: An array containing records of each movement 
  interval: A reference to the current interval, if one exists
  isMoving: A boolean flag, true when moving, false when still
  last: The last movement record. READONLY
  position: The angle the servo is at in the physical world. READONLY
  value: The angle the servo was last set to.
}
```

## Component Initialization

#### Basic

```js
// Create a standard servo...
// 
//   - attached to pin 10
//
new five.Servo(10);
```

#### Auto Centered

```js
// Create a standard servo...
// 
//   - attached to pin 10
//   - centered
//
new five.Servo({
  pin: 10,
  center: true
});
```

#### Specify Range

```js
// Create a standard servo...
// 
//   - attached to pin 10
//   - limited range of 45-135 degrees
//
new five.Servo({
  pin: 10,
  range: [45, 135]
});
```

#### Specify Starting Position

```js
// Create a standard servo...
// 
//   - attached to pin 10
//   - starts at 120°
//
new five.Servo({
  pin: 10,
  startAt: 120
});
```

#### Specify Both Range and Starting Position

```js
// Create a standard servo...
// 
//   - attached to pin 10
//   - limited range of 45-135 degrees
//   - starts at 120°
//
new five.Servo({
  pin: 10,
  range: [45, 135],
  startAt: 120
});
```

#### Continuous 

```js
// Direct Constructor
new five.Servo.Continuous(10);


// Options object with type property
new five.Servo({
  pin: 10,
  type: "continuous"
});
```

![Standard or Continuous](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/servo.png)


#### Servo PCA9685

```js
// Create a standard servo controlled by a PCA9685...
// 
//   - attached to pin 0 (of the PCA9685)
//
new five.Servo({
  controller: "PCA9685",
  pin: 0
});
```

#### Continuous PCA9685

```js
// Create a continuous servo controlled by a PCA9685...
// 
//   - attached to pin 0 (of the PCA9685)
//
new five.Servo.Continuous({
  controller: "PCA9685",
  pin: 0
});
```

![PCA9685](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/servo-PCA9685.png)


## Usage

Standard Servo
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var servo = new five.Servo(10);

  // Sweep from 0-180 and repeat.
  servo.sweep();
});
```


Continuous Servo
```js
var five = require("johnny-five");
var board = new five.Board();


board.on("ready", function() {

  var servo = new five.Servo({
    pin: 10, 
    type: "continuous"
  });

  // Clockwise, top speed.
  servo.cw(1);
});
```

I2C Servo (i.e. via Adafruit PWM controller)
```js
var five = require("johnny-five");
var board = new five.Board();


board.on("ready", function() {

  var servo = new five.Servo({
    pin: 0, 
    controller: "PCA9685",
    address: 0x41 // Defaults to 0x40
  });

  // Sweep from 0-180.
  servo.sweep();
});
```


## API

- **to(degrees 0-180 [, ms [, rate]])** Move a servo horn to specified position in degrees, 0-180 (or whatever the current valid range is). If `ms` is specified, the servo will take that amount of time to move to the position. If `rate` is specified, the angle change will be split into distance/rate steps for the `ms` option. If the specified angle is the same as the current angle, no commands are sent.
  ```js
  var servo = new five.Servo(10);

  // Set the horn to 90degrees
  servo.to(90);

  // Angle change takes 500ms to complete
  servo.to(90, 500);

  // Angle change takes 500ms to complete over 10 steps
  servo.to(90, 500, 10);
  ```

- **min()** Set Servo to minimum degrees. Defaults to 0deg, respects explicit range.
  ```js
  var servo = new five.Servo(10);

  // Set horn to 0degrees
  servo.min();
  ```
  Or 
  ```js
  var servo = new five.Servo({
    pin: 10, 
    range: [ 45, 135 ]
  });

  // Set horn to 45°
  servo.min();
  ```

- **max()** Set Servo to maximum degrees. Defaults to 180deg, respects explicit range.
  ```js
  var servo = new five.Servo(10);

  // Set horn to 135°
  servo.max();
  ```
  Or 
  ```js
  var servo = new five.Servo({
    pin: 10, 
    range: [ 45, 135 ]
  });

  // Set horn to 135°
  servo.max();
  ```


- **center()** Set Servo to center point. Defaults to 90deg, respects explicit range.
  ```js
  var servo = new five.Servo(10);

  // Set horn to 90degrees
  servo.center();
  ```
  Or 
  ```js
  var servo = new five.Servo({
    pin: 10, 
    range: [ 40, 80 ]
  });

  // Set horn to 60degrees
  servo.center();
  ```

- **sweep()** Sweep the servo between `min` and `max`, repeatedly.
  ```js
  var servo = new five.Servo(10);

  servo.sweep();
  ```
- **sweep([ low, high ])** Sweep the servo between an explicit range, repeatedly.
  ```js
  var servo = new five.Servo(10);

  // Repeated full range movement
  servo.sweep([45, 135]);
  ```

- **sweep(opts)** Sweep the servo between an (optional) explicit range, within an (optional) explicit interval, and  (optional) explicit steps (in degrees), repeatedly.
  - `opts`: An object containing: 
    - `range`: An array containing `[low, high]` boundaries to sweep between. 
    - `interval`: The interval of a half sweep (eg. `low -> high`; a full sweep is `low -> high -> low`)
    - `step`: The size of each step in degrees.
    ```js
    var servo = new five.Servo(10);
    servo.sweep({
      range: [45, 135]
    });

    servo.sweep({
      range: [45, 135], 
      interval: 1000,
    });

    servo.sweep({
      range: [45, 135], 
      interval: 1000,
      step: 10
    });
    ```

- **stop()** Stop a moving servo. 
  ```js
  var servo = new five.Servo(10);

  servo.stop();
  ```


- **cw(speed)** (Continuous only) Move a **continuous** servo clockwise at speed, 0-1
  ```js
  var servo = new five.Servo({
    pin: 10, 
    type: "continuous"
  });

  servo.cw(1);
  ```

- **ccw(speed)** (Continuous only) Move a **continuous** servo counter clockwise at speed, 0-1
  ```js
  var servo = new five.Servo({
    pin: 10, 
    type: "continuous"
  });

  servo.ccw(1);
  ```

## Events

It's recommended to use a rotary potentiometer as the mechanism for determining servo movement.

- **move:complete** this is emitted when a timed move is completed.


## Additional Information

Although servos can run on digital pins, this can sometimes cause issues. For this reason servos are forced to use only PWM under debug mode and will emit an error if used on a digital pin.

**Servo debug option:**
<table>
  <thead>
    <thead>
      <tr>
        <th>Property</th>
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

## Troubleshooting

If you are experiencing memory leak crashes when using your servo, make sure that you are not powering the servo directly from your board. Excessive power draw on the USB port causes such crashes. The following diagram shows an example of how to provide external power for servos: 

<img src="https://dl.dropboxusercontent.com/u/3531958/external-servo-power.png">

If your continuous servo moves when it should be stopped, it likely needs to be calibrated. Find instructions [here](http://bocoup.com/weblog/assembling-preparing-robotsconf-sumobot-with-johnny-five/).


<!--remove-start-->

## Examples

- [Servo](https://github.com/rwldrn/johnny-five/blob/master/docs/servo.md)
- [Servo Options](https://github.com/rwldrn/johnny-five/blob/master/docs/servo-options.md)
- [Servo Array](https://github.com/rwldrn/johnny-five/blob/master/docs/servo-array.md)
- [Servo Digital](https://github.com/rwldrn/johnny-five/blob/master/docs/servo-digital.md)
- [Servo Dual](https://github.com/rwldrn/johnny-five/blob/master/docs/servo-dual.md)
- [I2C Controlled Servo](https://github.com/rwaldron/johnny-five/blob/master/docs/servo-PCA9685.md)
- [Servo Tutorial](https://github.com/rwldrn/johnny-five/blob/master/docs/servo-tutorial.md)
- [Continuous Clock](https://github.com/rwldrn/johnny-five/blob/master/docs/continuous-clock.md)
- [Continuous](https://github.com/rwldrn/johnny-five/blob/master/docs/continuous.md)

<!--remove-end-->