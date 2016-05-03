The `Barometer` class constructs objects that represent a single Barometer sensor attached to the physical board.

Supported Barometers:

- BMP180
  - [Adafruit](https://www.adafruit.com/products/1603?utm_source=j5)
  - [SparkFun](https://www.sparkfun.com/products/11824?utm_source=j5)
  - [Grove](http://www.seeedstudio.com/depot/Grove-Barometer-Sensor-BMP180-p-1840.html?utm_source=j5)
- BMP280
  - [Adafruit](https://www.adafruit.com/products/2651?utm_source=j5)
- BME280
  - [Adafruit](https://www.adafruit.com/products/2652?utm_source=j5)
  - [Sparkfun](https://www.sparkfun.com/products/13676?utm_source=j5)
- MPL115A2
  - [Adafruit](https://www.adafruit.com/products/992?utm_source=j5)
  - [SparkFun](https://www.sparkfun.com/products/9721?utm_source=j5)
- MPL3115A2
  - [Adafruit](https://www.adafruit.com/products/1893?utm_source=j5)
  - [Sparkfun](https://www.sparkfun.com/products/11084?utm_source=j5)
    - [SparkFun Weather Shield](https://www.sparkfun.com/products/12081?utm_source=j5)
    - [SparkFun Photon Weather Shield](https://www.sparkfun.com/products/13630?utm_source=j5)
- MS5611
  - [Amazon](http://www.amazon.com/MS5611-High-resolution-Atmospheric-Pressure-Module/dp/B00F4P6LKE?utm_source=j5)

This list will continue to be updated as more component support is implemented.

## Parameters

- **General Options**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | controller    | string | BMP180, MPL115A2, MPL3115A2. The Name of the controller to use |  | Yes       |
  | address    | number | Address for I2C device. |  By Device | No       |
  | freq | number | Milliseconds. The rate in ms of data events. | 25 | No |
  </span>

- **BMP180 Options (`controller: "BMP180"`)**
  <span class="abbreviate-table">

  | Property | Type             | Value/Description     | Default | Required |
  |---------------|-------|------------|--------------------------------------------|---------|
  | mode | number | 0-3. Oversampling ratio mode. | 3 | No |
  </span>

## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `pressure` | The current pressure value in kPa. | Yes |

## Component Initialization



#### BMP180
```js
new five.Barometer({
  controller: "BMP180"
});
```

![BMP180](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-bmp180.png)


#### MPL115A2
```js
new five.Barometer({
  controller: "MPL115A2"
});
```

![MPL115A2](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-mpl115a2.png)


#### MPL3115A2
```js
new five.Barometer({
  controller: "MPL3115A2"
});
```

![MPL3115A2](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/barometer-mpl3115a2.png)


#### MS5611
```js
new five.Barometer({
  controller: "MS5611"
});
```

![MS5611](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-MS5611.png)

## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var barometer = new five.Barometer({
    controller: "MPL115A2"
  });

  barometer.on("data", function() {
    console.log("barometer");
    console.log("  pressure : ", this.pressure);
    console.log("--------------------------------------");
  });
});
```

## API

There are no `Barometer` specific methods.

## Events

- **change** The "change" event is emitted whenever the value of the barometric pressure sensor changes.

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds.
