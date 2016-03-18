The `Hygrometer` class constructs objects that represent a single Hygrometer sensor attached to the physical board. Hygrometers are used to measure absolute and relative humidity.

Supported Hygrometers:

- HTU21D
  - [Adafruit](https://www.adafruit.com/products/1899?utm_source=j5)
  - [Sparkfun](https://www.sparkfun.com/products/12064?utm_source=j5)

- TH02
  - [Grove](http://www.seeedstudio.com/depot/Grove-TemperatureHumidity-Sensor-HighAccuracy-Mini-p-1921.html?utm_source=j5)

This list will continue to be updated as more devices are confirmed.

## Parameters

- **General Options**
  <span class="abbreviate-table">

  | Property | Type          | Value/Description                                             | Default | Required                                                               |
  |---------------|---------------|---------------------------------------------------------------|---------------------------------------------------------|------------------------------------------------------------------------|
  | controller    | string        | HTU21D. The Name of the controller to use. |  | no |
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


#### TH02

```js
new five.Hygrometer({
  controller: "TH02"
});
```

![TH02](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-TH02.png)

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
