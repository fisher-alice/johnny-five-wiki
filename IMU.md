The `IMU` class constructs objects that represent a single IMU module attached to the physical board.  An IMU is an Inertial Measurement Unit.  IMUs come in all shapes and DOFs (Degrees of Freedom).  They are often made up of several components like Accelerometers, Gyros, Thermometer, and Compass/Magnetometers.

Supported modules:

- BNO055
  - [Adafruit](https://www.adafruit.com/products/2472?utm_source=j5)
  - [Atmel](http://www.atmel.com/tools/ATBNO055-XPRO.aspx?utm_source=j5)
  - [Tindie](https://www.tindie.com/products/onehorse/bno-055-9-axis-motion-sensor-with-hardware-sensor-fusion/?utm_source=j5)
- LSM303C
  - [Adafruit](https://www.adafruit.com/product/1120?utm_source=j5)
  - [SparkFun](https://www.sparkfun.com/products/13303?utm_source=j5)
- MPU6050
  - [Invensense](http://www.invensense.com/products/motion-tracking/6-axis/mpu-6050/?utm_source=j5)
  - [SparkFun](https://www.sparkfun.com/products/11028?utm_source=j5)


This list will continue to be updated as more component support is implemented.

## Parameters

- **General Options**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                                  | Default   | Required |
  |---------------|--------|-----------|-------------------------------------|-----------|
  | controller    | string | BNO055, LSM303C, MPU6050. The Name of the controller to use            | "MPU6050" | no       |
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
| `compass` | An instance of `Compass` class. | Yes |
| `gyro` | An instance of `Gyro` class. | Yes |
| `orientation` | An instance of `Orientiation` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |

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


#### BNO055

```js
// Create an BNO055 IMU object:
//
//  - attach SDA and SCL to the I2C pins on your board (A4 and A5 for the Uno)
//  - specify the BNO055 controller
new five.IMU({
  controller: "BNO055"
});
```


![imu-bno055.png](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/imu-bno055.png)

#### LSM303C

```js
// Create an LSM303C IMU object:
//
//  - attach SDA and SCL to the I2C pins on your board (A4 and A5 for the Uno)
//  - specify the LSM303C controller
new five.IMU({
  controller: "LSM303C"
});
```


![imu-lsm303c.png](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/imu-lsm303c.png)

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

- **calibrated** The "calibrated" event is fired whenever a corresponding "calibrated" is fired from a component.

<!--remove-start-->

## Examples

- [MPU6050 IMU](https://github.com/rwaldron/johnny-five/blob/master/docs/imu-mpu6050.md)

<!--remove-end-->
