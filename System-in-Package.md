The `SIP` class constructs objects that represent a single ["SiP" or "System-In-Package"](https://en.wikipedia.org/wiki/System_in_package?utm_source=j5) module (eg. [IMU](https://en.wikipedia.org/wiki/Inertial_measurement_unit?utm_source=j5), [MEMS](https://en.wikipedia.org/wiki/Microelectromechanical_systems?utm_source=j5)) attached to the physical board. An SiP may be made up of several components like Accelerometers, Gyroscopes, Thermometers, and Compass/Magnetometers.

Supported SiP sensor modules:


- BMP180
  - [Adafruit](https://www.adafruit.com/products/1603?utm_source=j5)
  - [SparkFun](https://www.sparkfun.com/products/11824?utm_source=j5)
  - [Grove](http://www.seeedstudio.com/depot/Grove-Barometer-Sensor-BMP180-p-1840.html?utm_source=j5)
- BMP280
  - [Adafruit](https://www.adafruit.com/products/2651?utm_source=j5)
- BME280
  - [Adafruit](https://www.adafruit.com/products/2652?utm_source=j5)
  - [Sparkfun](https://www.sparkfun.com/products/13676?utm_source=j5)
- HTU21D
  - [Adafruit](https://www.adafruit.com/products/1899?utm_source=j5)
  - [Sparkfun](https://www.sparkfun.com/products/12064?utm_source=j5)
- HIH6130
  - [Sparkfun](https://www.sparkfun.com/products/11295?utm_source=j5)
- MPL115A2
  - [Adafruit](https://www.adafruit.com/products/992?utm_source=j5)
  - [Sparkfun](https://www.sparkfun.com/products/9721?utm_source=j5)
- MPL3115A2
  - [Adafruit](https://www.adafruit.com/products/1893?utm_source=j5)
  - [Sparkfun](https://www.sparkfun.com/products/11084?utm_source=j5)
    - [SparkFun Weather Shield](https://www.sparkfun.com/products/12081?utm_source=j5)
    - [SparkFun Photon Weather Shield](https://www.sparkfun.com/products/13630?utm_source=j5)
- SI7020
  - [Tessel](http://start.tessel.io/modules/climate)
- SI7021
  - [Sparkfun](https://www.sparkfun.com/products/13763?utm_source=j5)
- MS5611
  - [Amazon](http://www.amazon.com/MS5611-High-resolution-Atmospheric-Pressure-Module/dp/B00F4P6LKE?utm_source=j5)
- TH02
  - [Grove](http://www.seeedstudio.com/depot/Grove-TemperatureHumidity-Sensor-HighAccuracy-Mini-p-1921.html?utm_source=j5)
- DHT11 (Via I2C Backpack)
  - [Google](https://www.google.com/search?q=DHT11?utm_source=j5)
- DHT21 (Via I2C Backpack)
  - [Google](https://www.google.com/search?q=DHT21?utm_source=j5)
- DHT22 (Via I2C Backpack)
  - [Google](https://www.google.com/search?q=DHT22?utm_source=j5)



This list will continue to be updated as more component support is implemented.

## Parameters

- **General Options**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                                  | Default   | Required |
  |---------------|--------|-----------|-------------------------------------|-----------|
  | controller    | string | BMP180, BMP280, BME280, HTU21D, HIH6130, MPL115A2, MPL3115A2, SI7020, SI7021, MS5611, TH02. The Name of the controller to use            |  | yes       |
  | freq          | Number | In milliseconds, how often `data` events should fire | 20 | no |

  </span>

In addition, anything in the `options` object passed to the `SIP` constructor will be passed along to each of its constituent component sensors (e.g. `Thermometer`, `Altimeter`, etc.). See relevant sensor component class documentation for more details about what options each takes.

## Shape
Some of these properties may or may not exist depending on whether the multi sensor supports it.

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `accelerometer` | An instance of `Accelerometer` class. | Yes |
| `altimeter` | An instance of `Altimeter` class. | Yes |
| `barometer` | An instance of `Barometer` class. | Yes |
| `gyro` | An instance of `Gyro` class. | Yes |
| `hygrometer` | An instance of `Hygrometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |
| ~~`temperature`~~ | ~~An instance of `Thermometer` class.~~ | ~~Yes~~ |


## Component Initialization

#### BMP180

```js
new five.SIP({
  controller: "BMP180"
});
```

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `barometer` | An instance of `Barometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |
| ~~`temperature`~~ | ~~An instance of `Thermometer` class.~~ | ~~Yes~~ |


![](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-bmp180.png)


#### HTU21D

```js
new five.SIP({
  controller: "HTU21D"
});
```

##### Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `hygrometer` | An instance of `Hygrometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |
| ~~`temperature`~~ | ~~An instance of `Thermometer` class.~~ | ~~Yes~~ |




![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-HTU21D-F.png)
![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-HTU21D.png)
![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/tessel-temperature-HTU21D-F.png)
![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/tessel-temperature-HTU21D.png)


#### HIH6130
```js
new five.SIP({
  controller: "HIH6130"
});
```

![HIH6130](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-HIH6130.png)


#### MPL115A2

```js
new five.SIP({
  controller: "MPL115A2"
});
```

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `barometer` | An instance of `Barometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |
| ~~`temperature`~~ | ~~An instance of `Thermometer` class.~~ | ~~Yes~~ |



![](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-mpl115a2.png)

#### MPL3115A2

```js
new five.SIP({
  controller: "MPL3115A2"
});
```

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `altimeter` | An instance of `Altimeter` class. | Yes |
| `barometer` | An instance of `Barometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |
| ~~`temperature`~~ | ~~An instance of `Thermometer` class.~~ | ~~Yes~~ |


![](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/barometer-mpl3115a2.png)

#### SI7020

```js
new five.SIP({
  controller: "SI7020"
});
```

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `hygrometer` | An instance of `Hygrometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |

![](http://johnny-five.io/img/breadboard/temperature-SI7020.png)

#### SI7021

```js
new five.SIP({
  controller: "SI7021"
});
```

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `hygrometer` | An instance of `Hygrometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |

![](http://johnny-five.io/img/breadboard/multi-SI7021.png)


#### MS5611
```js
new five.SIP({
  controller: "MS5611"
});
```

![MS5611](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-MS5611.png)

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `altimeter` | An instance of `Altimeter` class. | Yes |
| `barometer` | An instance of `Barometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |


#### TH02
```js
new five.SIP({
  controller: "TH02"
});
```

![TH02](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-TH02.png)

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `hygrometer` | An instance of `Hygrometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |


#### DHT11_I2C_NANO_BACKPACK
```js
new five.SIP({
  controller: "DHT11_I2C_NANO_BACKPACK"
});
```

![DHT11_I2C_NANO_BACKPACK](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-DHT11_I2C_NANO_BACKPACK.png)

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `hygrometer` | An instance of `Hygrometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |


#### DHT21_I2C_NANO_BACKPACK
```js
new five.SIP({
  controller: "DHT21_I2C_NANO_BACKPACK"
});
```

![DHT21_I2C_NANO_BACKPACK](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-DHT21_I2C_NANO_BACKPACK.png)

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `hygrometer` | An instance of `Hygrometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |


#### DHT22_I2C_NANO_BACKPACK
```js
new five.SIP({
  controller: "DHT22_I2C_NANO_BACKPACK"
});
```

![DHT22_I2C_NANO_BACKPACK](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-DHT22_I2C_NANO_BACKPACK.png)

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `hygrometer` | An instance of `Hygrometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |


## Usage

```js
const five = require("johnny-five");
const board = new five.Board();

board.on("ready", () => {

  const sip = new five.SIP({
    controller: "MPL115A2"
  });

  sip.on("data", () => {
    console.log("Barometer: %d", sip.barometer.pressure);
    console.log("Thermometer: %d", sip.thermometer.celsius);
  });
});
```



```js
const five = require("johnny-five");
const board = new five.Board();

board.on("ready", () => {
  const sip = new five.SIP({
    controller: "HTU21D"
  });

  sip.on("change", () => {
    console.log("Thermometer");
    console.log("  celsius           : ", sip.thermometer.celsius);
    console.log("  fahrenheit        : ", sip.thermometer.fahrenheit);
    console.log("  kelvin            : ", sip.thermometer.kelvin);
    console.log("--------------------------------------");

    console.log("Hygrometer");
    console.log("  relative humidity : ", sip.hygrometer.relativeHumidity);
    console.log("--------------------------------------");
  });
});
```

## API

The SIP class does not have an explicit API.  Refer to the individual components for their APIs

## Events

- **data** The "data" event is fired as frequently as new data becomes available.

- **change** The "change" event is fired whenever a corresponding "change" is fired from any constituent component.

_Note_: You may also bind to events on `SIP` object component sensors directly, e.g. `sip.thermometer.on('change')`

