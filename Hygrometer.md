The `Hygrometer` class constructs objects that represent a single Hygrometer sensor attached to the physical board. Hygrometers are used to measure absolute and relative humidity. 

Supported Hygrometers:

- HTU21D
  - [Adafruit](https://www.adafruit.com/products/1899)
  - [Sparkfun](https://www.sparkfun.com/products/12064)


This list will continue to be updated as more devices are confirmed.

## Parameters

- **General Options**

  | Property | Type          | Value/Description                                             | Default | Required                                                               |
  |---------------|---------------|---------------------------------------------------------------|---------------------------------------------------------|------------------------------------------------------------------------|
  | controller    | string        | HTU21D. The Name of the controller to use. |  | no |
  | freq          | Number        | Milliseconds. The rate in milliseconds to emit the data event         | 25ms | no                                                                     |



## Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  relativeHumidity: The relative humidity in percent. READONLY
  RH: A convenience alias for relativeHumidity. READONLY
}
```

## Component Initialization


#### MPU6050

```js
new five.Hygrometer({
  controller: "HTU21D"
});
```

![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/humidity-htu21d.png)


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