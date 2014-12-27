The `Accelerometer` class constructs objects that represent a single Accelerometer sensor attached to the physical board.

Johnny-Five currently supports two kinds of Accelerometers:

- Analog (Like the Tinkerkit 2/3-Axis Accelerometer)
- MPU-6050 I2C IMU

This list will continue to be updated as more Analog devices are confirmed.

### Parameters

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
      <td>Varies by device</td>
      <td>This value can be identified in the device's datasheet.</td>
      <td>96 (Tinkerkit)</td>
      <td>no</td>
    </tr>
    <tr>
      <td>aref</td>
      <td>Number</td>
      <td>Voltage reference</td>
      <td>This is the value of the VCC pin</td>
      <td>5</td>
      <td>no</td>
    </tr>
    <tr>
      <td>zeroV</td>
      <td>Number</td>
      <td>0-1023</td>
      <td>The analog input when at rest, perpendicular to gravity.</td>
      <td>478</td>
      <td>no</td>
    </tr>
  </tbody>
</table>



- **MPU-6050 Options(`controller: "MPU6050"`)** 
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
      <td>maxValue</td>
      <td>Number</td>
      <td>16 bit value</td>
      <td>The raw value of the device when pointing at gravity</td>
      <td>17300</td>
      <td>no</td>
    </tr>
  </tbody>
</table>

```js
// Create an analog Accelerometer object:
// 
//   - attach X and Y to "A0" and "A1" respectively
//
var accelerometer = new five.Accelerometer({
  pins: ["A0", "A1"],
  sensitivity: 96, // optional
  aref: 5,         // optional
  zeroV: 478       // optional
});
```

```js
// Create an MPU-6050 Accelerometer object:
//
//  - attach SDA and SCL to the I2C pins on your board (A4 and A5 for the Uno)
//  - specify the MPU6050 controller
var accelerometer = new five.Accelerometer({
  controller: "MPU6050",
  maxValue: 17300 // optional
});
```

### Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pins: The pins defined for X, Y, and Z.
  pitch: The pitch angle in degrees. READONLY
  roll: The roll angle in degrees. READONLY
  x: Value of x axis in G forces. READONLY
  y: Value of y axis in G forces. READONLY
  z: Value of z axis in G forces. READONLY
  acceleration: The magnitude of the acceleration in G forces. READONLY
  inclination: The incline of the device in degrees. READONLY
  orientation: The orientation of the device (-3, -2, -1, 1, 2, 3). READONLY 
}
```



### Usage
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var accelerometer = new five.Accelerometer({
    pins: ["A0", "A1"]
  });

  accelerometer.on("change", function(err, data) {
    console.log("X: %d", this.x);
    console.log("Y: %d", this.y);
  });
});
```

## API

- **hasAxis(name)** Does the device support the axis? 
```js
if (accelerometer.hasAxis("z")) {
  console.log(accelerometer.z)
}
```

## Events

- **change** The "change" event is emitted whenever the value of the accelerometer changes more then the threshold value allows.

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the "read" event)

## Examples

- [Tinkerkit Accelerometer](https://github.com/rwaldron/johnny-five/blob/master/docs/tinkerkit-accelerometer.md)
- [MPU6050 Accelerometer](https://github.com/rwaldron/johnny-five/blob/master/docs/accelerometer-mpu6050.md)
- [Accelerometer](https://github.com/rwaldron/johnny-five/blob/master/docs/accelerometer.md)
- [Accelerometer Pan and Tilt](https://github.com/rwaldron/johnny-five/blob/master/docs/accelerometer-pan-tilt.md)
