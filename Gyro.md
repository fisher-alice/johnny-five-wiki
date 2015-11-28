![](http://i.gyazo.com/d886380feb6d3a929772ad0e89c6dd18.png)

The `Gyro` class constructs objects that represent a single Gyro sensor attached to the physical board.

We currently support two kinds of Gyros:

- LPR5150 (Analog)
  - Used on the TinkerKit Gyro breakout
- MPU6050 (I2C)
  - [Invensense](http://www.invensense.com/products/motion-tracking/6-axis/mpu-6050/)
  - [SparkFun](https://www.sparkfun.com/products/11028?utm_source=j5)

This list will continue to be updated as more Gyro devices are confirmed.

## Parameters

- **General Options**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|-----------------|-----------------------------------|----------|
  | controller    | string | "ANALOG", "MPU6050". The Name of the controller to use | "ANALOG" | no       |
  </span>

- **Analog Options (`controller: "ANALOG"`)**
  <span class="abbreviate-table">

  | Property | Type             | Value/Description  | Default | Required |
  |---------------|------------------|-------------------------------------------------------------------------|--------------------------------------------------------------------|---------|
  | pins          | Array of Strings | `["A*"]`. The String analog pins that X, Y, and Z (optional) are attached to |    | yes      |
  | sensitivity   | Number           | Varies by device. For Tinkerkit, use `Gyro.TK_4X` or `Gyro.TK_1X`. This value can be identified in the device's datasheet.            |    | yes      |
  | resolution    | Number           | Varies by device. This value can be identified in the device's datasheet             | 4.88    | no       |
  </span>

- **MPU6050 Options (`controller: "MPU6050"`)**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                      | Default | Required |
  |---------------|--------|----------------------|---------------------------------------------------------------------------------------------------|---------|
  | sensitivity   | Number | LSB/DegreesPerSecond. The sensitivity of the device. The MPU-6050 is currently configured at +/- 250 degrees per second | 131     | no       |
  </span>

## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `pins` | The pins defined for X, Y, and Z. | No |
| `isCalibrated` | The calibration state of the device. | Yes |
| `pitch` | An object containing values for the pitch rate and angle. | Yes |
| `roll` | An object containing values for the roll rate and angle. | Yes |
| `yaw` | An object containing values for the yaw rate and angle. | Yes |
| `rate` | And object containing the rate values of X, Y, and Z. | Yes |
| `x` | Value of x axis. | Yes |
| `y` | Value of y axis. | Yes |
| `z` | Value of z axis. | Yes |

## Component Initialization

#### Analog

```js
// Analog Gyro:
//
//   - attach X and Y to "A0" and "A1" respectively
//   - Use the LPR5150AL 4X sensitivity rating
//
new five.Gyro({
  pins: ["A0", "A1"],
  sensitivity: 0.67, // optional
  resolution: 4.88   // optional
});
```

![lpr5150l](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/gyro-lpr5150l.png)

#### MPU6050

```js
// Create an MPU-6050 Gyro object:
//
//  - attach SDA and SCL to the I2C pins on your board (A4 and A5 for the Uno)
//  - specify the MPU6050 controller
new five.Gyro({
  controller: "MPU6050",
  sensitivity: 131 // optional
});
```

![MPU6050](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/gyro-mpu6050.png)


## Usage
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var gyro = new five.Gyro({
    controller: "MPU6050"
  });

  gyro.on("change", function() {
    console.log("gyro");
    console.log("  x            : ", this.x);
    console.log("  y            : ", this.y);
    console.log("  z            : ", this.z);
    console.log("  pitch        : ", this.pitch);
    console.log("  roll         : ", this.roll);
    console.log("  yaw          : ", this.yaw);
    console.log("  rate         : ", this.rate);
    console.log("  isCalibrated : ", this.isCalibrated);
    console.log("--------------------------------------");
  });
});
```

## API

* **recalibrate()** Tell the device to recalibrate

## Events

- **change** The "change" event is emitted whenever the value of the gyro changes more then the threshold value allows.

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the "read" event)

<!--remove-start-->

## Examples

- [Tinkerkit Gyroscope](https://github.com/rwldrn/johnny-five/blob/master/docs/tinkerkit-gyroscope.md)
- [MPU6050 Gyroscope](https://github.com/rwaldron/johnny-five/blob/master/docs/gyro-mpu6050.md)
- [Analog Gyroscope](https://github.com/rwaldron/johnny-five/blob/master/docs/gyro-lpr5150l.md)
- [Gyro](https://github.com/rwldrn/johnny-five/blob/master/docs/gyro.md)

<!--remove-end-->
