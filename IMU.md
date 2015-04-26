The `IMU` class constructs objects that represent a single IMU module attached to the physical board.  An IMU is an Inertial Measurement Unit.  IMUs come in all shapes and DOFs (Degrees of Freedom).  They are often made up of several components like Accelerometers, Gyros, Temperature Sensors, and Magnetometers.

Johnny-Five currently supports one kind of IMU:

- [MPU-6050](http://www.invensense.com/mems/gyro/mpu6050.html)

This list will continue to be updated as more devices are confirmed.

## Parameters

- **General Options**

  | Property | Type   | Value/Description                                  | Default   | Required |
  |---------------|--------|-----------|----------------------------------------------|-----------|----------|
  | controller    | string | “MPU6050”. The Name of the controller to use            | “MPU6050” | no       |
  | freq          | Number | Milliseconds. The frequency in ms of data events. Defaults to 25ms | 100       | no       |

- **MPU6050 Options(`controller: "MPU6050"`)** 

  | Property | Type   | Value/Description                                                | Default | Required |
  |---------------|--------|-------------|------------------------------------------------------------|---------|----------|
  | address       | Number | 8-bit value. The address of the component (can be switched via ADO pin) | 0x68    | no       |


## Shape 
Some of these properties may or may not exist depending on whether the IMU supports it.

```
{ 
  accelerometer: An instance of `Accelerometer` class. READONLY
  gyro: An instance of `Gyro` class. READONLY
  temperature: An instance of `Temperature` class. READONLY
}
```

## Component Initialization

#### MPU6050

```js
// Create an MPU-6050 IMU object:
//
//  - attach SDA and SCL to the I2C pins on your board (A4 and A5 for the Uno)
//  - specify the MPU6050 controller
new five.IMU({
  controller: "MPU6050",
  address: 0x68, // optional
  freq: 100      // optional
});
```


![imu-mpu6050.png](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/imu-mpu6050.png)   


## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

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

- **change** The "change" event is fired whenever a corresponding "change" is fired from a component.

<!--remove-start-->

## Examples

- [MPU6050 IMU](https://github.com/rwaldron/johnny-five/blob/master/docs/imu-mpu6050.md)

<!--remove-end-->