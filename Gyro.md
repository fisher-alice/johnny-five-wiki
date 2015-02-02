The `Gyro` class constructs objects that represent a single Gyro sensor attached to the physical board.

We currently support two kinds of Gyros:

- Analog (Like the Tinkerkit 2-Axis Gyro)
- MPU-6050 I2C IMU

This list will continue to be updated as more Gyro devices are confirmed.

## Parameters

- **General Options**
<table>
  <thead>
    <tr>
      <th>Property Name</th>
      <th>Type</th>
      <th>Value(s)</th>
      <th>Description</th>
      <th>Default</th>
      <th>Required</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>controller</td>
      <td>string</td>
      <td>"ANALOG" | "MPU6050"</td>
      <td>The Name of the controller to use</td>
      <td>"ANALOG"</td>
      <td>no</td>
    </tr>
  </tbody>
</table>

- **Analog Options(`controller: "ANALOG"`)** 
  <table>
    <thead>
      <tr>
        <th>Property Name</th>
        <th>Type</th>
        <th>Value(s)</th>
        <th>Description</th>
        <th>Default</th>
        <th>Required</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>pins</td>
        <td>Array of Strings</td>
        <td>["A*"]</td>
        <td>The String analog pins that X, Y, and Z (optional) are attached to</td>
        <td>none</td>
        <td>yes</td>
      </tr>
      <tr>
        <td>sensitivity</td>
        <td>Number</td>
        <td>Varies by device. For Tinkerkit, use `Gyro.TK_4X` or `Gyro.TK_1X`</td>
        <td>This value can be identified in the device's datasheet.</td>
        <td>none</td>
        <td>yes</td>
      </tr>
      <tr>
        <td>resolution</td>
        <td>Number</td>
        <td>Varies by device</td>
        <td>This value can be identified in the device's datasheet</td>
        <td>4.88</td>
        <td>no</td>
      </tr>
    </tbody>
  </table>



- **MPU6050 Options(`controller: "MPU6050"`)** 
  <table>
    <thead>
      <tr>
        <th>Property Name</th>
        <th>Type</th>
        <th>Value(s)</th>
        <th>Description</th>
        <th>Default</th>
        <th>Required</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>sensitivity</td>
        <td>Number</td>
        <td>LSB/DegreesPerSecond</td>
        <td>The sensitivity of the device.  The MPU-6050 is currently configured at +/- 250 degrees per second</td>
        <td>131</td>
        <td>no</td>
      </tr>
    </tbody>
  </table>

## Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pins: The pins defined for X, Y, and Z.
  isCalibrated: The calibration state of the device. READONLY
  pitch: An object containing values for the pitch rate and angle. READONLY
  roll: An object containing values for the roll rate and angle. READONLY
  yaw: An object containing values for the yaw rate and angle. READONLY
  rate: And object containing the rate values of X, Y, and Z. READONLY
  x: Value of x axis. READONLY
  y: Value of y axis. READONLY
  z: Value of z axis. READONLY
}
```

## Component Initialization


```js
// Analog Gyro:
// 
//   - attach X and Y to "A0" and "A1" respectively
//   - Use the LPR5150AL 4X sensitivity rating
//
var gyro = new five.Gyro({
  pins: ["A0", "A1"],
  sensitivity: 0.67, // optional
  resolution: 4.88   // optional
});
```

![lpr5150l](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/gyro-lpr5150l.png)

```js
// Create an MPU-6050 Gyro object:
//
//  - attach SDA and SCL to the I2C pins on your board (A4 and A5 for the Uno)
//  - specify the MPU6050 controller
var gyro = new five.Gyro({
  controller: "MPU6050",
  sensitivity: 131 // optional
});
```

![MPU6050](https://github.com/rwaldron/johnny-five/blob/master/docs/breadboard/gyro-mpu6050.png)


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

## Examples

- [Tinkerkit Gyroscope](https://github.com/rwldrn/johnny-five/blob/master/docs/tinkerkit-gyroscope.md)
- [MPU6050 Gyroscope](https://github.com/rwaldron/johnny-five/blob/master/docs/gyro-lpr5150l.md)
- [Analog Gyroscope](https://github.com/rwaldron/johnny-five/blob/master/docs/gyro-mpu6050.md)
- [Gyro](https://github.com/rwldrn/johnny-five/blob/master/docs/gyro.md)
