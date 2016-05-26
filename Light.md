The `Light` class constructs objects that represent a single analog or digital light light component attached to the physical board. Light sensors are used to measure ambient or direct light intensity.

Supported Light sensors:

- Photoreistors
  - Sparkfun
    - [Mini Photocell](https://www.sparkfun.com/products/9088?utm_source=j5)
- TSL2561
  - [Adafruit](https://www.adafruit.com/product/439?utm_source=j5)
  - [Sparkfun](https://www.sparkfun.com/products/12055?utm_source=j5)
- EV3 Color & Light Sensor
  - [Lego](http://shop.lego.com/en-US/EV3-Color-Sensor-45506?utm_source=j5)
- NXT Color Sensor
  - [Lego](http://shop.lego.com/en-US/Light-Sensor-9844?utm_source=j5)


This list will continue to be updated as more devices are confirmed.


## Parameters

- **pin** A Number or String address for the Light pin (analog).

- **options** An object of property parameters.
  <span class="abbreviate-table">

  | Property | Type           | Value/Description                                                                         | Default | Required |
  |---------------|----------------|----------------------------------|-------------------------------------------------------------------------------------|----------|
  | pin           | Number, String | Analog Pin. The Number or String address of the pin the light is attached to, ie. "A0" | | No      |
  | controller    | string        | DEFAULT, EVS_EV3, TSL2561. The Name of the controller to use. |  | no |
  | freq          | Number        | Milliseconds. The rate in milliseconds to emit the data event         | 25ms | no                                                                     |
  | threshold     | Number         | Any. The change threshold (+/- value). | 1  | no       |
  </span>

## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `pin` | The pin address that the Light is attached to | No |
| `threshold` | The change threshold (+/- value). Defaults to 1 | No |
| `value` | Raw value | Yes |
| `level` | Intensity level 0-1 | Yes |

## Component Initialization

#### Analog Light (Most common case)

```js
new five.Light("A0");
```

![HTU21D](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/photoresistor.png)



## Usage

#### Analog Light 

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var light = new five.Light("A0");
  light.on("change", function() {
    console.log(this.level);
  });
});
```



## API


- **within(range, handler)** When value is within the provided range, execute callback. 
  ```js
  var light = new five.Light("A0");

  light.within([ 0.10, 0.20 ], function() {
    
    // This is called when the light's value property falls within 100-200

  });
  ```

## Events

- **change** The "change" event is emitted whenever the value of the light changes more than the threshold value allows. 
- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds.



