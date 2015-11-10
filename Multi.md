The `Multi` class constructs objects that represent a single "breakout" module attached to the physical board. The "breakout" module will itself contain 2 or more components, such as a thermistor and a hygrometer, or an altimeter and a pressure sensor. 

Supported multi sensor modules: 

- BMP180
  - [Adafruit](https://www.adafruit.com/products/1603)
  - [Sparkfun](https://www.sparkfun.com/products/11824)
- HTU21D
  - [Adafruit](https://www.adafruit.com/products/1899)
  - [Sparkfun](https://www.sparkfun.com/products/12064)
- MPL115A2
  - [Adafruit](https://www.adafruit.com/products/992)
  - [Sparkfun](https://www.sparkfun.com/products/9721)
- MPL3115A2
  - [Adafruit](https://www.adafruit.com/products/1893)
  - [Sparkfun](https://www.sparkfun.com/products/11084)
    - [SparkFun Weather Shield](https://www.sparkfun.com/products/12081)
    - [SparkFun Photon Weather Shield](https://www.sparkfun.com/products/13630)
- SI7020
  - [Tessel](http://start.tessel.io/modules/climate)

This list will continue to be updated as more component support is implemented.

## Parameters

- **General Options**

  | Property | Type   | Value/Description                                  | Default   | Required |
  |---------------|--------|-----------|-------------------------------------|-----------|
  | controller    | string | BMP180, HTU21D, MPL115A2, MPL3115A2, SI7020. The Name of the controller to use            |  | yes       |


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
| ~~`temperature`~~ | An instance of `Thermometer` class. | Yes |


## Component Initialization

#### BMP180

```js
new five.Multi({
  controller: "BMP180"
});
```

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `barometer` | An instance of `Barometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |
| ~~`temperature`~~ | An instance of `Thermometer` class. | Yes |


![](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-bmp180.png)   


#### HTU21D

```js
new five.Multi({
  controller: "HTU21D"
});
```

##### Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `hygrometer` | An instance of `Hygrometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |
| ~~`temperature`~~ | An instance of `Thermometer` class. | Yes |




![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-HTU21D-F.png)
![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/temperature-HTU21D.png)
![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/tessel-temperature-HTU21D-F.png)
![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/tessel-temperature-HTU21D.png)



#### MPL115A2

```js
new five.Multi({
  controller: "MPL115A2"
});
```

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `barometer` | An instance of `Barometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |
| ~~`temperature`~~ | An instance of `Thermometer` class. | Yes |



![](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-mpl115a2.png)   

#### MPL3115A2

```js
new five.Multi({
  controller: "MPL3115A2"
});
```

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `altimeter` | An instance of `Altimeter` class. | Yes |
| `barometer` | An instance of `Barometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |
| ~~`temperature`~~ | An instance of `Thermometer` class. | Yes |


![](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/barometer-mpl3115a2.png)   

#### SI7020

```js
new five.Multi({
  controller: "SI7020"
});
```

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `hygrometer` | An instance of `Hygrometer` class. | Yes |
| `thermometer` | An instance of `Thermometer` class. | Yes |
| ~~`temperature`~~ | An instance of `Thermometer` class. | Yes |

![](http://johnny-five.io/img/breadboard/temperature-SI7020.png)   


## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var multi = new five.Multi({
    controller: "MPL115A2"
  });

  multi.on("data", function() {
    console.log("Barometer: %d", this.barometer.pressure);
    console.log("Temperature: %d", this.temperature.celsius);
  });
});
```



```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var multi = new five.Multi({
    controller: "HTU21D"
  });

  multi.on("change", function() {
    console.log("temperature");
    console.log("  celsius           : ", this.temperature.celsius);
    console.log("  fahrenheit        : ", this.temperature.fahrenheit);
    console.log("  kelvin            : ", this.temperature.kelvin);
    console.log("--------------------------------------");

    console.log("Hygrometer");
    console.log("  relative humidity : ", this.hygrometer.relativeHumidity);
    console.log("--------------------------------------");
  });
});
```

## API

The Multi class does not have an explicit API.  Refer to the individual components for their APIs

## Events

- **data** The "data" event is fired as frequently as new data becomes available.

- **change** The "change" event is fired whenever a corresponding "change" is fired from a component.

