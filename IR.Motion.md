The `IR.Motion` class constructs an object that represents a single Passive Infrared Motion sensor.

- Motion
    - [Passive Infra-Red by Parallax, Digital](http://www.parallax.com/tabid/768/productid/83/default.aspx)

## Parameters

- **pin** A Number pin address for Motion sensor.

- **options** An object of property parameters.

  | Property | Type           | Value(s)                   | Description                                                                    | Required |
  |---------------|----------------|----------------------------|--------------------------------------------------------------------------------|----------|
  | pin           | Number, String | 9, “D7” (Any pin on board) | The Number or String address of the pin the sensor is attached to, ie. 9, “D7” | yes      |


## Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pin: The pin address that the Sensor is attached to
  value: Digital state reading (0 or 1). READONLY
  isCalibrated: Boolean flag indicating calibration state of motion sensor 
}

```


## Component Initialization

#### Basic

```js
// Pin only
var motion = new five.IR.Motion(7);

// Options object with pin property
var motion = new five.IR.Motion({
  pin: 7
});
```

![IR.Motion](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/ir-motion.png)

## Usage
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  // Create a new `motion` hardware instance.
  var motion = new five.IR.Motion(7);

  // "calibrated" occurs once, at the beginning of a session,
  motion.on("calibrated", function() {
    console.log("calibrated");
  });

  // "motionstart" events are fired when the "calibrated"
  // proximal area is disrupted, generally by some form of movement
  motion.on("motionstart", function() {
    console.log("motionstart");
  });

  // "motionend" events are fired following a "motionstart" event
  // when no movement has occurred in X ms
  motion.on("motionend", function() {
    console.log("motionend");
  });
});
```

## Events

- **data** The "data" event is fired as frequently as the user defined freq will allow in milliseconds. ("data" replaced the "read" event)

- **motionstart** The "motionstart" event is fired when motion occurs within the observable range of the PIR/IR.Motion/IR.Proximity sensor

- **motionend** The "motionend" event is fired when motion has ceased within the observable range of the PIR/IR.Motion/IR.Proximity sensor.

- **calibrated** The "calibrated" event is fired when PIR/IR.Motion sensor is ready to detect movement/motion in observable range. (PIR/IR.Motion ONLY)