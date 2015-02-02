The `Accelerometer` class constructs objects that represent a single Accelerometer sensor attached to the physical board.

Supported Accelerometers:

- Analog (Like the Tinkerkit 2/3-Axis Accelerometer)
- MPU6050 (I2C IMU)
- ADXL345 (I2C)
- ADXL335 (Analog)
- MMA7361 (Analog)

This list will continue to be updated as more component support is implemented.

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
        <td>ANALOG, MPU6050, ADXL345, ADXL335, MMA7361</td>
        <td>The Name of the controller to use</td>
        <td>"ANALOG"</td>
        <td>no</td>
      </tr>
    </tbody>
  </table>

- **Analog Options (`controller: "ANALOG"`)** 
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
        <td>Number or Array</td>
        <td>0-1023</td>
        <td>The analog input when at rest, perpendicular to gravity.  When an array, specifies the zeroV for the individual axes.</td>
        <td>478</td>
        <td>no</td>
      </tr>
      <tr>
        <td>autoCalibrate</td>
        <td>Boolean</td>
        <td>true/false</td>
        <td>Whether to auto-calibrate the `zeroV` values.  The device must be initialized with X/Y axes perpendicular to the earth, and the Z axis pointing to the sky.</td>
        <td>false</td>
        <td>no</td>
      </tr>
    </tbody>
  </table>



- **MPU6050 Options (`controller: "MPU6050"`)** 
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
        <td>16 bit value</td>
        <td>The sensitivity for the +- 250 setting of this device</td>
        <td>16384</td>
        <td>no</td>
      </tr>
    </tbody>
  </table>

- **MMA7361 Options (`controller: "MMA7361"`)** 
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
        <td>sleepPin</td>
        <td>Number or String</td>
        <td>Digital Pin Address</td>
        <td>The digital pin that controls the sleep pin on the device.  If you don't set this pin, you need to pull it up to Vcc with a 10k resistor.</td>
        <td>null</td>
        <td>no</td>
      </tr>
    </tbody>
  </table>

## Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  zeroV: The current zeroV value (or values).  
          May be different from initial values if auto-calibrated.
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

## Component Initialization 

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

![LIS344AL](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/accelerometer-lis344al.png)

```js
// Create an MPU-6050 Accelerometer object:
//
//  - attach SDA and SCL to the I2C pins on your board (A4 and A5 for the Uno)
//  - specify the MPU6050 controller
var accelerometer = new five.Accelerometer({
  controller: "MPU6050",
  sensitivity: 16384 // optional
});
```

![MPU6050](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/accelerometer-mpu6050.png)

```js
// Create an ADXL345 Accelerometer object:
//
//  - attach SDA and SCL to the I2C pins on your board (A4 and A5 for the Uno)
//  - specify the ADXL345 controller
var accelerometer = new five.Accelerometer({
  controller: "ADXL345"
});
```

![ADXL345](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/accelerometer-adxl345.png)

```js
// Create an ADXL335 Accelerometer object:
//
//  - attach Xout, Yout and Zout to A0, A1, A2
//  - specify the ADXL335 controller
var accelerometer = new five.Accelerometer({
  controller: "ADXL335",
  pins: ["A0", "A1", "A2"]
});
```

![ADXL335](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/accelerometer-adxl335.png)

```js
// Create an MMA7361 Accelerometer object:
//
// - attach Xo, Yo, and Zo to A0, A1, A2
// - specify the MMA7361 controller
// - optionally, specify the sleepPin
// - optionally, set it to auto-calibrate
// - optionally, set the zeroV values.  This can be done when retrieving them after an autoCalibrate
var accelerometer = new five.Accelerometer({
    controller: "MMA7361",
    pins: ["A0", "A1", "A2"],
    sleepPin: 13,
    autoCalibrate: true,
    zeroV: [320, 365, 295] // override the zeroV values if you know what they are from a previous autoCalibrate
  });
```

![MMA7361](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/accelerometer-mma7361.png)

## Usage
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

- **enable()** Enable the device and events (enabled by default).  For devices that can be put to sleep (like the MMA7361), wake it up.
```js
accelerometer.enable();
```

- **disable()** Disable the device and events.  For devices that can be put to sleep (like the MMA7361), put it to sleep.
```js
accelerometer.disable();
```

## Events

- **change** The "change" event is emitted whenever the value of the accelerometer changes more then the threshold value allows.

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the "read" event)

## Examples

- [Tinkerkit Accelerometer](https://github.com/rwaldron/johnny-five/blob/master/docs/tinkerkit-accelerometer.md)
- [MPU6050 Accelerometer](https://github.com/rwaldron/johnny-five/blob/master/docs/accelerometer-mpu6050.md)
- [MMA7361 Accelerometer](https://github.com/rwaldron/johnny-five/blob/master/docs/accelerometer-mma7361.md)
- [Accelerometer](https://github.com/rwaldron/johnny-five/blob/master/docs/accelerometer.md)
- [Accelerometer Pan and Tilt](https://github.com/rwaldron/johnny-five/blob/master/docs/accelerometer-pan-tilt.md)