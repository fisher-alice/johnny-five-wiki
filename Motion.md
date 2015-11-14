The `Motion` class constructs an object that represents a single Motion Detection sensor.


Supported Motion sensors: 

- [Passive Infra-Red by Parallax](http://www.parallax.com/tabid/768/productid/83/default.aspx)
- [Generic Passive Infra-Red, HC-SR501](http://www.amazon.com/HC-SR501-Sensor-Module-Pyroelectric-Infrared/dp/B007XQRKD4)
- [OSEPP IR Proximity Sensor](http://osepp.com/products/sensors-arduino-compatible/osepp-ir-proximity-sensor-module/)
  - Note: despite its name, this **is not** a proximity sensor that outputs a measured distance to an obstruction.
- Sharp IR Motion Detection
  - [GP2Y0D810Z0F](https://www.pololu.com/product/1134)
  - [GP2Y0D815Z0F](https://www.pololu.com/product/1133)

## Parameters

- **pin** A Number pin address for Motion sensor.

- **options** An object of property parameters.
  <span class="abbreviate-table">

  | Property | Type  | Value/Description | Default | Required |
  |----------|-------|-------------------|---------|----------|
  | pin      | Number, String | Analog or Digital Pin. Use for non-I2C sensors | | Yes (non-I2C) |
  | controller | String | PIR, HCSR501, GP2Y0D805Z0F, GP2Y0D810Z0F, GP2Y0D815Z0F. See aliases | `PIR` | No |
  </span>

  ##### Controller Alias Table

  | Controller | Alias |
  |------------|-------|
  | HC-SR501 | PIR |
  | HCSR501 | PIR |
  | GP2Y0D805Z0F | 0D805 |
  | GP2Y0D805Z0F | 805 |
  | GP2Y0D810Z0F | 0D810 |
  | GP2Y0D810Z0F | 810 |
  | GP2Y0D815Z0F | 0D815 |
  | GP2Y0D815Z0F | 815 |


## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `pin` | The pin address that the Sensor is attached to | No |
| `value` | Sensor value. | Yes |
| `detectedMotion` | Boolean value. | Yes |
| `isCalibrated` | Boolean flag indicating calibration state. | Yes |


## Component Initialization

#### Passive InfraRed Motion 

This is the default controller. 

```js
// Pin only
new five.Motion(7);

// Options object with pin property
var motion = new five.Motion({
  pin: 7
});
```

![Motion](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/motion.png)

#### GP2Y0D805Z0F

```js

var motion = new five.Motion({
  controller: "GP2Y0D805Z0F"
});
```

![Motion](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/motion-gp2y0d805z0f.png)


#### GP2Y0D810Z0F and GP2Y0D815Z0F

```js
var motion = new five.Motion({
  controller: "GP2Y0D810Z0F", 
  pin: "A0"
});

var motion = new five.Motion({
  controller: "GP2Y0D815Z0F", 
  pin: "A0"
});
```

![Motion](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/GP2Y0D810Z0F.png)


## Usage
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  // Create a new `motion` hardware instance.
  var motion = new five.Motion(7);

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

- **change** The "change" event is fired whenever a change within the motion detection field is observed. 

- **motionstart** The "motionstart" event is fired when motion occurs within the observable range of the PIR/Motion/IR.Proximity sensor

- **motionend** The "motionend" event is fired when motion has ceased within the observable range of the PIR/Motion/IR.Proximity sensor.

- **calibrated** The "calibrated" event is fired when PIR/Motion sensor is ready to detect movement/motion in observable range. (PIR/Motion ONLY)