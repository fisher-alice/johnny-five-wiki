![](http://i.gyazo.com/68628475fed220178a90d19f85133f97.png)

The `Accelerometer` class constructs objects that represent a single Accelerometer sensor attached to the physical board.

Supported Accelerometers:

- Analog (Like the Tinkerkit 2/3-Axis Accelerometer)
  - ADXL335
    - [Adafruit](https://www.adafruit.com/product/163?utm_source=j5)
    - [SparkFun](https://www.sparkfun.com/products/9269?utm_source=j5)
  - ADXL362
    - [SparkFun](https://www.sparkfun.com/products/11446?utm_source=j5)
  - LIS344AL
  - MMA7361
    - [SparkFun](https://www.sparkfun.com/products/retired/9652?utm_source=j5) (Retired)
- MPU6050 (I2C IMU)
  - [Invensense](http://www.invensense.com/products/motion-tracking/6-axis/mpu-6050/)
  - [SparkFun](https://www.sparkfun.com/products/11028?utm_source=j5)
- ADXL345 (I2C)
  - [Adafruit](https://www.adafruit.com/products/1231?utm_source=j5)
  - [SparkFun](https://www.sparkfun.com/products/9836?utm_source=j5)

This list will continue to be updated as more component support is implemented.

## Parameters

- **General Options**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | controller    | string | ANALOG, MPU6050, ADXL345, ADXL335, MMA7361. The Name of the controller to use | “ANALOG” | no       |
  </span>

- **Analog Options (`controller: "ANALOG"`)**
  <span class="abbreviate-table">

  | Property | Type             | Value/Description                                                                                                                                                  | Default        | Required |
  |---------------|------------------|-------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|
  | pins          | Array of Strings | [“A\*”]. The String analog pins that X, Y, and Z (optional) are attached to                                                                                           | none           | yes      |
  | sensitivity   | Number           | Varies by device. This value can be identified in the device's datasheet.                                                                                                      | 96 (Tinkerkit) | no       |
  | aref          | Number           | Voltage reference. This is the value of the VCC pin                                                                                                                             | 5              | no       |
  | zeroV         | Number or Array  | 0-1023. The analog input when at rest, perpendicular to gravity. When an array, specifies the zeroV for the individual axes.                                         | 478            | no       |
  | autoCalibrate | Boolean          | `true`, `false`. Whether to auto-calibrate the `zeroV` values. The device must be initialized with X/Y axes perpendicular to the earth, and the Z axis pointing to the sky. | false          | no       |
  </span>

- **MPU6050 Options (`controller: "MPU6050"`)**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                                           | Default | Required |
  |---------------|--------|--------------|-------------------------------------------------------|---------|
  | sensitivity   | Number | 16 bit value. The sensitivity for the +- 250 setting of this device | 16384   | no       |
  </span>

- **MMA7361 Options (`controller: "MMA7361"`)**
  <span class="abbreviate-table">

  | Property | Type             | Value/Description                                                                                                                              | Default | Required |
  |---------------|------------------|---------------------|------------------------------------------------------------------------------------------------------------------------------------------|---------|
  | sleepPin      | Number or String | The digital pin that controls the sleep pin on the device. If you don't set this pin, you need to pull it up to Vcc with a 10k resistor. | null    | no       |
  </span>

## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `zeroV` | The current zeroV value (or values). May be different from initial values if auto-calibrated. | No |
| `pins` | The pins defined for X, Y, and Z. | No |
| `pitch` | The pitch angle in degrees. | Yes |
| `roll` | The roll angle in degrees. | Yes |
| `x` | Value of x axis in G forces. | Yes |
| `y` | Value of y axis in G forces. | Yes |
| `z` | Value of z axis in G forces. | Yes |
| `acceleration` | The magnitude of the acceleration in G forces. | Yes |
| `inclination` | The incline of the device in degrees. | Yes |
| `orientation` | The orientation of the device (-3, -2, -1, 1, 2, 3). | Yes |

## Component Initialization

#### Analog
```js
// Create an analog Accelerometer object:
//
//   - attach X and Y to "A0" and "A1" respectively
//
new five.Accelerometer({
  pins: ["A0", "A1"],
  sensitivity: 96, // optional
  aref: 5,         // optional
  zeroV: 478       // optional
});
```

![LIS344AL](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/accelerometer-lis344al.png)

#### MPU6050
```js
// Create an MPU-6050 Accelerometer object:
//
//  - attach SDA and SCL to the I2C pins on
//     your board (A4 and A5 for the Uno)
//  - specify the MPU6050 controller
new five.Accelerometer({
  controller: "MPU6050",
  sensitivity: 16384 // optional
});
```

![MPU6050](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/accelerometer-mpu6050.png)

#### ADXL345
```js
// Create an ADXL345 Accelerometer object:
//
//  - attach SDA and SCL to the I2C pins on
//     your board (A4 and A5 for the Uno)
//  - specify the ADXL345 controller
new five.Accelerometer({
  controller: "ADXL345"
});
```

![ADXL345](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/accelerometer-adxl345.png)

#### ADXL335
```js
// Create an ADXL335 Accelerometer object:
//
//  - attach Xout, Yout and Zout to A0, A1, A2
//  - specify the ADXL335 controller
new five.Accelerometer({
  controller: "ADXL335",
  pins: ["A0", "A1", "A2"]
});
```

![ADXL335](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/accelerometer-adxl335.png)

#### MMA7361
```js
// Create an MMA7361 Accelerometer object:
//
// - attach Xo, Yo, and Zo to A0, A1, A2
// - specify the MMA7361 controller
// - optionally, specify the sleepPin
// - optionally, set it to auto-calibrate
// - optionally, set the zeroV values.
//    This can be done when retrieving them
//    after an autoCalibrate
new five.Accelerometer({
  controller: "MMA7361",
  pins: ["A0", "A1", "A2"],
  sleepPin: 13,
  autoCalibrate: true,
  // override the zeroV values if you know what
  // they are from a previous autoCalibrate
  zeroV: [320, 365, 295]
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

  accelerometer.on("change", function() {
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

- **change** The "change" event is emitted whenever any reading changes.

- **data** The "data" event is fired as frequently as the user defined freq will allow in milliseconds.

<!--remove-start-->
## Examples

- [Tinkerkit Accelerometer](https://github.com/rwaldron/johnny-five/blob/master/docs/tinkerkit-accelerometer.md)
- [MPU6050 Accelerometer](https://github.com/rwaldron/johnny-five/blob/master/docs/accelerometer-mpu6050.md)
- [MMA7361 Accelerometer](https://github.com/rwaldron/johnny-five/blob/master/docs/accelerometer-mma7361.md)
- [Accelerometer](https://github.com/rwaldron/johnny-five/blob/master/docs/accelerometer.md)
- [Accelerometer Pan and Tilt](https://github.com/rwaldron/johnny-five/blob/master/docs/accelerometer-pan-tilt.md)

<!--remove-end-->
