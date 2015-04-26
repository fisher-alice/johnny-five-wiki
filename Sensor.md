The `Sensor` class constructs objects that represent a single analog sensor component attached to the physical board. This class is intentionally generic and will work well with many types of sensors, including (but not limited to):

- Linear Potentiometer
- Rotary Potentiometer
- Flex Sensitive Resistor
- Pressure Sensitive Resistor
- Force Sensitive Resistor
- Hall Sensor
- Tilt Sensor
- Photoresistor/Light Dependent Resistor

...And more, see [examples](#examples)

## Parameters

- **pin** A Number or String address for the Sensor pin (analog).

- **options** An object of property parameters.

  | Property | Type           | Value/Description                                                                         | Default | Required |
  |---------------|----------------|----------------------------------|-------------------------------------------------------------------------------------|----------|
  | pin           | Number, String | Analog Pin. The Number or String address of the pin the sensor is attached to, ie. “A0” or “I1” | | yes      |
  | freq          | Number         | Milliseconds. The frequency in ms of data events. | 25ms                                | no       |
  | threshold     | Number         | Any. The change threshold (+/- value). | 1                                     | no       |

- **options (experimental)** These options can be used with the `Sensor` class, but are considered experimental.

  | Property | Type           | Value/Description                                                                         | Default | Required |
  |---------------|----------------|----------------------------------|-------------------------------------------------------------------------------------|----------|
  | type     | String         | "digital", "analog". Specify that this is a sensor attached to a digital pin. Allows using a digital pin as the value of the `pin` option| | no       |
  

## Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pin: The pin address that the Sensor is attached to
  threshold: The change threshold (+/- value). Defaults to 1
  boolean: ADC value scaled to a boolean. READONLY
  raw: ADC value (0-1023). READONLY
  analog: ADC reading _scaled_ to 8 bit values (0-255). READONLY
  constrained: ADC reading _constrained_ to 8 bit values (0-255). READONLY
  value: ADC reading, scaled. READONLY
}
```

## Component Initialization

#### Analog Sensor (Most common case)

```js
new five.Sensor("A0");
```

#### Digital Sensor (Less common, but supported experimentally)

```js
new five.Sensor({
  pin: 2, 
  type: "digital"
});

// Can also be written as: 
new five.Sensor.Digital(2);
```


#### Common Options

```js
//   - attached to pin "A0"
//   - emits data events every 250ms
//   - emits change events when the ADC value has changed by +5/-5
//
var temp = new five.Sensor({
  pin: "A0", 
  freq: 250, 
  threshold: 5
});
```


## Usage

#### Analog Sensor 

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var sensor = new five.Sensor("A0");
  
  // Scale the sensor's data from 0-1023 to 0-10 and log changes
  sensor.scale(0, 10).on("change", function() {
    console.log(this.value);
  });
});
```

#### Digital Sensor 

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var sensor = new five.Sensor.Digital(2);

  sensor.on("change", function() {
    console.log(this.value);
  });
});
```


## API

- **scale(low, high)** Scale the sensor's value to a new value within a specified range.
  ```js
  var sensor = new five.Sensor("A0");

  sensor.scale(0, 180).on("change", function() {
    // this.value will reflect a scaling from 0-1023 to 0-180
    console.log( this.value );
  });
  ```

- **scale([low, high])** Same as `scale(low, high)`. 


- **booleanAt(barrier)** Set a midpoint barrier value used to calculate returned value of the .boolean property. Defaults to 528
  ```js
  var sensor = new five.Sensor("A0");

  // ADC readings less than 100 will result 
  //  in the value of the `boolean` property being false.
  // 
  // ADC readings greater than 100 will result 
  //  in the value of the `boolean` property being true.
  // 
  sensor.booleanAt(100);

  ```

- **within(range, handler)** When value is within the provided range, execute callback. 
  ```js
  var sensor = new five.Sensor("A0");

  sensor.within([ 100, 200 ], function() {
    
    // This is called when the sensor's value property falls within 100-200

  });
  ```

## Events

- **change** The "change" event is emitted whenever the value of the sensor changes more then the threshold value allows. The "change" event has aliases that can be used to better reason about the program being written: "slide", "touch", "force", "bend".

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the deprecated "read" event)


<!--remove-start-->

## Examples

- [Sensor Component](https://github.com/rwaldron/johnny-five/blob/master/docs/sensor.md)
- [Sensor - Photoresistor](https://github.com/rwaldron/johnny-five/blob/master/docs/photoresistor.md)
- [Sensor - Potentiometer](https://github.com/rwaldron/johnny-five/blob/master/docs/potentiometer.md)
- [Sensor - Force Sensitive Servo Controller](https://github.com/rwaldron/johnny-five/blob/master/docs/sensor-fsr-servo.md)
- [Sensor - Slide Potentiometer](https://github.com/rwaldron/johnny-five/blob/master/docs/sensor-slider.md)
- [Sensor - Slide Potentiometer Servo Controller](https://github.com/rwaldron/johnny-five/blob/master/docs/slider-servo-control.md)

<!--remove-end-->
