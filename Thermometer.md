The `Thermometer` class constructs objects that represents a single Thermometer/Temperature sensor attached to the physical board.

Johnny-Five currently supports several kinds of Thermometer/Temperature sensors:

- Analog (with user-supplied `toCelsius(raw)` function)
- LM35
  - [Mouser](http://www.mouser.com/search/ProductDetail.aspx?R=0virtualkey0virtualkeyLM35DZ-NOPB)
  - [TI](http://www.ti.com/product/lm35)
- TMP36
  - [Adafruit](https://www.adafruit.com/products/165?utm_source=j5)
  - [Analog Devices](http://www.analog.com/en/mems-sensors/digital-temperature-sensors/tmp36/products/product.html)
  - [SparkFun](https://www.sparkfun.com/products/10988?utm_source=j5)
- TMP102
  - [SparkFun](https://www.sparkfun.com/products/11931?utm_source=j5)
- DS18B20 (requires [ConfigurableFirmata](https://github.com/firmata/arduino/tree/configurable))
  - [Adafruit](https://www.adafruit.com/products/374?utm_source=j5)
  - [Maxim Integrated](http://www.maximintegrated.com/en/products/analog/sensors-and-sensor-interface/DS18B20.html)
  - [SparkFun](https://www.sparkfun.com/products/245?utm_source=j5)
- MPU6050
  - [Invensense](http://www.invensense.com/products/motion-tracking/6-axis/mpu-6050/)
  - [SparkFun](https://www.sparkfun.com/products/11028?utm_source=j5)
- Grove Thermometer
  - [Seeed Studio](http://www.seeedstudio.com/depot/Grove-Thermometer-Sensor-p-774.html)
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
- HTU21D
  - [Adafruit](https://www.adafruit.com/product/1899?utm_source=j5)
  - [Sparkfun](https://www.sparkfun.com/products/12064?utm_source=j5)
- MCP9808
  - [Adafruit](https://www.adafruit.com/product/1782?utm_source=j5)
- SI7020
  - [Tessel Climate](https://shop.tessel.io/Modules/Climate%20Module?utm_source=j5)
- MS5611
  - [Amazon](http://www.amazon.com/MS5611-High-resolution-Atmospheric-Pressure-Module/dp/B00F4P6LKE?utm_source=j5)
- TH02
  - [Grove](http://www.seeedstudio.com/depot/Grove-TemperatureHumidity-Sensor-HighAccuracy-Mini-p-1921.html?utm_source=j5)
- DHT11 (Via I2C Backpack)
  - [Google](https://www.google.com/search?q=DHT11?utm_source=j5)



This list will continue to be updated as more devices are confirmed.

## Parameters

- **General Options**
  <span class="abbreviate-table">

  | Property   | Type          | Value/Description                                                                                                     | Default  | Required        |
  |------------|---------------|-----------------------------------------------------------------------------------------------------------------------|----------|-----------------|
  | controller | string        | ANALOG, LM35, TMP36, DS18B20, MPU6050, GROVE, BMP180, MPL115A2, MPL3115A2, HTU21D, MCP9808, SI7020. The Name of the controller to use. | `ANALOG` | no              |
  | pin        | string or int | Analog Pin. Use with analog sensor.                                                                                   |          | Yes<sup>1</sup> |
  | toCelsius  | function      | `function toCelsius(raw) {}` A raw-to-celsius transform override                                                      |          | no              |
  | freq       | Number        | Milliseconds. The rate in milliseconds to emit the data event                                                         | 25ms     | no              |                                                              |
  </span>

<sup>1</sup> Yes for analog devices. No for digital devices (MPU6050 or DS18B20)

## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `celsius` | The temperature in celsius degrees. | Yes |
| `fahrenheit` | The temperature in fahrenheit degrees. | Yes |
| `kelvin` | The temperature in kelvin degrees. | Yes |
| `C` | A convenience alias for celsius. | Yes |
| `F` | A convenience alias for fahrenheit. | Yes |
| `K` | A convenience alias for kelvin. | Yes |


## Component Initialization

#### Analog

```js
// Create an analog Thermometer object:
new five.Thermometer({
  pin: "A0",
  toCelsius: function(raw) { // optional
    return (raw / sensivity) + offset;
  }
});
```

#### LM35

```js
new five.Thermometer({
  controller: "LM35",
  pin: "A0"
});
```

![LM35](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-lm35.png)


#### TMP36
```js
new five.Thermometer({
  controller: "TMP36",
  pin: "A0"
});
```

![TMP36](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-tmp36.png)

#### TMP102
```js
new five.Thermometer({
  controller: "TMP102"
});
```

![TMP102](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-tmp102.png)

#### DS18B20


```js
new five.Thermometer({
  controller: "DS18B20",
  pin: "A0"
});
```

![DS18B20](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-ds18b20.png)

#### MPU6050

```js
// Create an MPU-6050 Thermometer object:
//
//  - attach SDA and SCL to the I2C pins on your board (A4 and A5 for the Uno)
//  - specify the MPU6050 controller
new five.Thermometer({
  controller: "MPU6050"
});
```

![MPU6050](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-mpu6050.png)

#### MPL115A2
```js
new five.Thermometer({
  controller: "MPL115A2"
});
```

![MPL115A2](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-mpl115a2.png)

#### MPL3115A2
```js
new five.Thermometer({
  controller: "MPL3115A2"
});
```

![MPL3115A2](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/barometer-mpl3115a2.png)


#### BMP180
```js
new five.Thermometer({
  controller: "BMP180"
});
```

![BMP180](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-bmp180.png)


#### HTU21D
```js
new five.Thermometer({
  controller: "HTU21D"
});
```

![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-HTU21D-F.png)
![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-HTU21D.png)
![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/tessel-temperature-HTU21D-F.png)
![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/tessel-temperature-HTU21D.png)


#### MCP9808
```js
new five.Thermometer({
  controller: "MCP9808"
});
```

![MCP9808](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-MCP9808.png)
![MCP9808](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-MCP9808-tessel.png)


#### MS5611
```js
new five.Thermometer({
  controller: "MS5611"
});
```

![MS5611](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-MS5611.png)

#### TH02
```js
new five.Thermometer({
  controller: "TH02"
});
```

![TH02](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-TH02.png)


#### DHT11_I2C_NANO_BACKPACK
```js
new five.Multi({
  controller: "DHT11_I2C_NANO_BACKPACK"
});
```

![DHT11_I2C_NANO_BACKPACK](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-DHT11_I2C_NANO_BACKPACK.png)



## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var temperature = new five.Thermometer({
    pin: "A0"
  });

  temperature.on("data", function() {
    console.log("celsius: %d", this.C);
    console.log("fahrenheit: %d", this.F);
    console.log("kelvin: %d", this.K);
  });
});
```


## API

There are no special API functions for this class.

## Events

- **change** The "change" event is emitted whenever the value of the temperature changes.

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the "read" event)
