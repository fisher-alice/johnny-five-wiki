The `Hygrometer` class constructs objects that represent a single Hygrometer sensor attached to the physical board. Hygrometers are used to measure absolute and relative humidity.

Supported Hygrometers:

- BME280
  - [Adafruit](https://www.adafruit.com/products/2652?utm_source=j5)
  - [Sparkfun](https://www.sparkfun.com/products/13676?utm_source=j5)
- HTU21D
  - [Adafruit](https://www.adafruit.com/products/1899?utm_source=j5)
  - [Sparkfun](https://www.sparkfun.com/products/12064?utm_source=j5)
- HIH6130
  - [Sparkfun](https://www.sparkfun.com/products/11295?utm_source=j5)
- TH02
  - [Grove](http://www.seeedstudio.com/depot/Grove-TemperatureHumidity-Sensor-HighAccuracy-Mini-p-1921.html?utm_source=j5)
- SI7020
  - [Tessel Climate](http://www.seeedstudio.com/depot/Tessel-Climate-Module-p-2225.html?utm_source=j5)
- SHT31D
  - [Adafruit](https://www.adafruit.com/products/2857?utm_source=j5)
- DHT11 (Via I2C Backpack)
  - [Google](https://www.google.com/search?q=DHT11?utm_source=j5)
- DHT21 (Via I2C Backpack)
  - [Google](https://www.google.com/search?q=DHT21?utm_source=j5)
- DHT22 (Via I2C Backpack)
  - [Google](https://www.google.com/search?q=DHT22?utm_source=j5)



This list will continue to be updated as more devices are confirmed.

## Parameters

- **General Options**
  <span class="abbreviate-table">

  | Property | Type          | Value/Description                                             | Default | Required                                                               |
  |---------------|---------------|---------------------------------------------------------------|---------------------------------------------------------|------------------------------------------------------------------------|
  | controller    | string        | BME280, HTU21D, HIH6130, TH02, SI7020, SHT31D. The Name of the controller to use. |  | no |
  | freq          | Number        | Milliseconds. The rate in milliseconds to emit the data event         | 25ms | no                                                                     |
  </span>


## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `relativeHumidity` | The relative humidity in percent. | Yes |
| `RH` | A convenience alias for relativeHumidity. | Yes |

## Component Initialization


#### HTU21D

```js
new five.Hygrometer({
  controller: "HTU21D"
});
```

![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-HTU21D-F.png)
![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-HTU21D.png)
![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/tessel-temperature-HTU21D-F.png)
![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/tessel-temperature-HTU21D.png)


#### HIH6130
```js
new five.Hygrometer({
  controller: "HIH6130"
});
```

![HIH6130](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-HIH6130.png)

#### TH02

```js
new five.Hygrometer({
  controller: "TH02"
});
```

![TH02](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-TH02.png)



#### DHT11_I2C_NANO_BACKPACK
```js
new five.Hygrometer({
  controller: "DHT11_I2C_NANO_BACKPACK"
});
```

![DHT11_I2C_NANO_BACKPACK](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-DHT11_I2C_NANO_BACKPACK.png)



#### DHT21_I2C_NANO_BACKPACK
```js
new five.Hygrometer({
  controller: "DHT21_I2C_NANO_BACKPACK"
});
```

![DHT21_I2C_NANO_BACKPACK](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-DHT21_I2C_NANO_BACKPACK.png)



#### DHT22_I2C_NANO_BACKPACK
```js
new five.Hygrometer({
  controller: "DHT22_I2C_NANO_BACKPACK"
});
```

![DHT22_I2C_NANO_BACKPACK](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-DHT22_I2C_NANO_BACKPACK.png)





## Usage
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var hygrometer = new five.Hygrometer({
    controller: "HTU21D"
  });

  hygrometer.on("change", function() {
    console.log(this.relativeHumidity + " %");
  });
});
```

## API

There are no special API functions for this class.

## Events

- **change** The "change" event is emitted whenever the value of the relative humidity changes.

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the "read" event)
