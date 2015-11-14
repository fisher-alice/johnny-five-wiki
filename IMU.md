The `IMU` class constructs objects that represent a single IMU module attached to the physical board.  An IMU is an Inertial Measurement Unit.  IMUs come in all shapes and DOFs (Degrees of Freedom).  They are often made up of several components like Accelerometers, Gyros, Temperature Sensors, and Magnetometers.

Supported modules: 

- MPU6050
  - [Invensense](http://www.invensense.com/products/motion-tracking/6-axis/mpu-6050/)
  - [SparkFun](https://www.sparkfun.com/products/11028)

This list will continue to be updated as more component support is implemented.

## Parameters

- **General Options**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                                  | Default   | Required |
  |---------------|--------|-----------|-------------------------------------|-----------|
  | controller    | string | MPU6050. The Name of the controller to use            | “MPU6050” | no       |

  </span>

- **MPU6050 Options(`controller: "MPU6050"`)** 
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                                                | Default | Required |
  |---------------|--------|-------------|-------------------------------------|----------|
  | address       | Number | 8-bit value. The address of the component (can be switched via ADO pin) | `0x68`    | no       |
  
  </span>

## Shape 
Some of these properties may or may not exist depending on whether the IMU supports it.

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `accelerometer` | An instance of `Accelerometer` class. | Yes |
| `gyro` | An instance of `Gyro` class. | Yes |
| `temperature` | An instance of `Temperature` class. | Yes |

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