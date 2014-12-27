The `IMU` class constructs objects that represent a single IMU module attached to the physical board.  An IMU is an Inertial Measurement Unit.  IMUs come in all shapes and DOFs (Degrees of Freedom).  They are often made up of several components like Accelerometers, Gyros, Temperature Sensors, and Magnetometers.

Johnny-Five currently supports one kind of IMU:

- [MPU-6050](http://www.invensense.com/mems/gyro/mpu6050.html)

This list will continue to be updated as more devices are confirmed.

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
      <td>"MPU6050"</td>
      <td>The Name of the controller to use</td>
      <td>"MPU6050"</td>
      <td>no</td>
    </tr>
    <tr>
      <td>freq</td>
      <td>Number</td>
      <td>ms</td>
      <td>The delay in reporting data from this device</td>
      <td>100</td>
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
      <td>address</td>
      <td>Number</td>
      <td>8-bit value</td>
      <td>The address of the component (can be switched via ADO pin)</td>
      <td>0x68</td>
      <td>no</td>
    </tr>
  </tbody>
</table>

```js
// Create an MPU-6050 IMU object:
//
//  - attach SDA and SCL to the I2C pins on your board (A4 and A5 for the Uno)
//  - specify the MPU6050 controller
var accelerometer = new five.IMU({
  controller: "MPU6050",
  address: 0x68, // optional
  freq: 100      // optional
});
```

### Shape 
Some of these properties may or may not exist depending on whether the IMU supports it.

```
{ 
  accelerometer: An instance of `Accelerometer` class. READONLY
  gyro: An instance of `Gyro` class. READONLY
  temperature: An instance of `Temperature` class. READONLY
}
```



### Usage
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var accelerometer = new five.IMU({
    controller: "MPU6050"
  });

  accelerometer.on("data", function(err, data) {
    console.log("Accelerometer: %d, %d, %d", this.accelerometer.x, this.accelerometer.z, this.accelerometer.z);
    console.log("Gyro: %d, %d, %d", this.gyro.x, this.gyro.z, this.gyro.z);
    console.log("Temperature: %d", this.temperature.celsius);
  });
});
```

## API
The IMU does not have an explicit API.  Refer to the individual components for their APIs

## Events

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds.

- **change** The IMU does not emit "change" events directly.  You can wath the aggregate components for "change" events.

## Examples

- [MPU-6050 IMU](https://github.com/rwaldron/johnny-five/blob/master/docs/imu-mpu6050.md)
