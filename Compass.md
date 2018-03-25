![](http://i.gyazo.com/5808690439afa3eb111d82234ed97f76.png)

The `Compass` class constructs an object that represents a single Compass or Magnetometer.

Supported Compass/Magnetometer:

- HMC6352
  - No longer in production, but we assume people may still have them.
- HMC5883L
  - [Triple-axis Magnetometer (Compass) Board - HMC5883L](https://www.adafruit.com/products/1746?utm_source=j5)
  - [SparkFun Triple Axis Magnetometer Breakout - HMC5883L](https://www.sparkfun.com/products/10530?utm_source=j5)
- BNO055
  - [Adafruit 9-DOF Absolute Orientation IMU Fusion Breakout - BNO055](https://www.adafruit.com/product/2472)
- MAG3110
  - [SparkFun Triple Axis Magnetometer Breakout - MAG3110](https://www.sparkfun.com/products/12670)
- LSM303C
  - [Adafruit](https://www.adafruit.com/product/1120?utm_source=j5)
  - [SparkFun](https://www.sparkfun.com/products/13303?utm_source=j5)

This list will continue to be updated as more component support is implemented.



## Parameters

- **Options** An object of property parameters.
  <span class="abbreviate-table">

  | Property | Type   | Value/Description | Default | Required |
  |----------|--------|-------------------|---------|----------|
  | controller    | String | BNO055, HMC6352, HMC5883L, MAG3110, LSM303C. The name of the controller to use | | yes      |
  | gauss         | Number | cgs units. Set the scale gauss for compass readings. | 1.3 | no       |
  </span>

## Shape


| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `raw`     | The raw x/y/z values. | Yes |
| `heading` | The current heading in degrees, 0-360° | Yes |
| `bearing` | An object of properties whose values are relevant bearing information ★ | Yes |

- **bearing** ★

  | Property Name | Description | Read Only |
  |---------------| ----------- | ----------|
  | `point`       | A cardinal direction, eg. "north", "south", "east", "west" | No |
  | `abbr`        | Abbreviated `point`, eg. "N", "NE", "NEbE" | No |
  | `low`         | Low end of cardinal range in degrees | No |
  | `mid`         | Middle end of cardinal range in degrees | No |
  | `high`        | High end of cardinal range in degrees | No |



## Component Initialization

#### BNO055

```js
new five.Compass({
  controller: "BNO055"
});
```
![BNO055](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/imu-bno055.png)


#### HMC6352

```js
new five.Compass({
  controller: "HMC6352"
});
```
![HMC6352](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/compass-hmc6352.png)

#### HMC5883L

```js
new five.Compass({
  controller: "HMC5883L"
});
```
![HMC5883L](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/compass-hmc5883l.png)

#### MAG3110

```js
new five.Compass({
  controller: "MAG3110"
});
```
![MAG3110](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/compass-MAG3110.png)


#### LSM303C

```js
// Create an LSM303C Compass/Magnetometer object:
//
//  - attach SDA and SCL to the I2C pins on your board (A4 and A5 for the Uno)
//  - specify the LSM303C controller
new five.Compass({
  controller: "LSM303C"
});
// Or...
new five.Magnetometer({
  controller: "LSM303C"
});
```


![imu-lsm303c.png](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/imu-lsm303c.png)

## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var compass = new five.Compass({
    controller: "HMC6352"
  });

  compass.on("change", function() {
    console.log("change");
    console.log("  heading : ", Math.floor(this.heading));
    console.log("  bearing : ", this.bearing.point);
    console.log("--------------------------------------");
  });

  compass.on("data", function() {
    console.log("  heading : ", Math.floor(this.heading));
    console.log("  bearing : ", this.bearing.point);
    console.log("--------------------------------------");
  });
});
```

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var compass = new five.Compass({
    controller: "HMC5883L"
  });

  compass.on("change", function() {
    console.log("change");
    console.log("  heading : ", Math.floor(this.heading));
    console.log("  bearing : ", this.bearing.point);
    console.log("--------------------------------------");
  });

  compass.on("data", function() {
    console.log("  heading : ", Math.floor(this.heading));
    console.log("  bearing : ", this.bearing.point);
    console.log("--------------------------------------");
  });
});
```

## API

There are no special API functions for this class.


## Events

- **change** The "change" event is emitted whenever the heading of the compass has changed from it's last position
- **data** The "data" event is fired as frequently as the user defined freq will allow in milliseconds.

<!--remove-start-->

## Examples

<!--remove-end-->
