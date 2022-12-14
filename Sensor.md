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
  <span class="abbreviate-table">

  | Property | Type           | Value/Description                                                                         | Default | Required |
  |---------------|----------------|----------------------------------|-------------------------------------------------------------------------------------|----------|
  | pin           | Number, String | Analog Pin. The Number or String address of the pin the sensor is attached to, ie. `"A0"` or `0` mean the same thing.  | | yes      |
  | freq          | Number         | Milliseconds. The frequency in ms of data events. | 25ms                                | no       |
  | threshold     | Number         | Any. The change threshold (+/- value). | 1                                     | no       |
  | enabled    | boolean       | Whether to start emitting events right away (>= [v0.9.12](https://github.com/rwaldron/johnny-five/releases/tag/0.9.12)) | `true` | no |
  </span>

- **options (experimental)** These options can be used with the `Sensor` class, but are considered experimental.
  <span class="abbreviate-table">

  | Property | Type           | Value/Description                                                                         | Default | Required |
  |---------------|----------------|----------------------------------|-------------------------------------------------------------------------------------|----------|
  | type     | String         | "digital", "analog". Specify that this is a sensor attached to a digital pin. Allows using a digital pin as the value of the `pin` option| | no       |
  </span>  

## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `pin` | The pin address that the Sensor is attached to | No |
| `threshold` | The change threshold (+/- value). Defaults to 1 | No |
| `boolean` | ADC value scaled to a boolean. | Yes |
| `raw` | ADC value (0-1023). | Yes |
| `analog` | ADC reading _scaled_ to 8 bit values (0-255). | Yes |
| `constrained` | ADC reading _constrained_ to 8 bit values (0-255). | Yes |
| `value` | ADC reading, scaled. | Yes |
| `freq` | The rate in milliseconds to emit the data event. Disables the event if set to `null`. (>= [v0.9.12](https://github.com/rwaldron/johnny-five/releases/tag/v0.9.12)) | No |

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
  sensor.on("change", function() {
    console.log(this.scaleTo(0, 10));
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

If you prefer arrow functions...


```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", () => {
  var sensor = new five.Sensor("A0");
  
  // Scale the sensor's data from 0-1023 to 0-10 and log changes
  sensor.on("change", () => {
    console.log(sensor.scaleTo(0, 10));
  });
});
```

#### Digital Sensor 

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", () => {

  var sensor = new five.Sensor.Digital(2);

  sensor.on("change", () => {
    console.log(sensor.value);
  });
});
```



## API


- **scaleTo(low, high)** (integer) Return the sensor's present value, scaled to a new value within the specified low/high range. 
- **fscaleTo(low, high)** (float) Return the sensor's present value, scaled to a new value within the specified low/high range.
  ```js
  var sensor = new five.Sensor("A0");

  sensor.on("change", function() {
    // this.value will reflect a scaling from 0-1023 to 0-180
    console.log(this.scaleTo(0, 180)); // integer
    console.log(this.fscaleTo(0, 180)); // float
  });
  ```
  ```js
  var sensor = new five.Sensor("A0");

  sensor.on("change", () => {
    // this.value will reflect a scaling from 0-1023 to 0-180
    console.log(sensor.scaleTo(0, 180)); // integer
    console.log(sensor.fscaleTo(0, 180)); // float
  });
  ```

- **scaleTo([low, high])** (integer) Return the sensor's present value, scaled to a new value within the specified low/high range. 
- **fscaleTo([low, high])** (float) Return the sensor's present value, scaled to a new value within the specified low/high range.
  ```js
  var sensor = new five.Sensor("A0");

  sensor.on("change", function() {
    // this.value will reflect a scaling from 0-1023 to 0-180
    console.log(this.scaleTo([0, 180])); // integer
    console.log(this.fscaleTo([0, 180])); // float
  });
  ```
  ```js
  var sensor = new five.Sensor("A0");

  sensor.on("change", () => {
    // this.value will reflect a scaling from 0-1023 to 0-180
    console.log(sensor.scaleTo([0, 180])); // integer
    console.log(sensor.fscaleTo([0, 180])); // float
  });
  ```


- **scale(low, high)** Scale the sensor's value to a new value within a specified range.
  `scale(...)` is deprecated, use `scaleTo(...)`
- **scale([low, high])** Same as `scale(low, high)`. 
  `scale(...)` is deprecated, use `scaleTo(...)`

- **booleanAt(barrier)** Set a midpoint barrier value used to calculate returned value of the .boolean property. The barrier is based on the scaled value, not the raw value. Defaults to 50% (512 when unscaled).
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

- **enable()** Start emitting "data" and "change" events, at a rate determined by the user defined `freq` in milliseconds.  No-op if Sensor is already enabled. (>= [v0.9.12](https://github.com/rwaldron/johnny-five/releases/tag/v0.9.12))

- **disable()** Stop emitting "data" and "change" events.  No-op if Sensor is already disabled. (>= [v0.9.12](https://github.com/rwaldron/johnny-five/releases/tag/v0.9.12))

## Events

- **change** The "change" event is emitted whenever the value of the sensor changes more than the threshold value allows. 
- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the deprecated "read" event)


## Collection

`Sensor` supports a `Sensors` collection class, which allows multiple `Sensor` instances to be controlled via a single instance object. Events emitted by instances of the `Sensor` class are forwarded through instances of the `Sensors` class. The handler receives the instance that emitted the event as the first parameter.

```js
new five.Sensors([ 2, 3, 4, 5 ]);
new five.Sensors([ { pin: 2 }, { pin: 3 }, { pin: 4 }, { pin: 5 } ]);
new five.Sensors([ sensor1, sensor2, sensor3 ]);
```



<!--remove-start-->

## Examples

- [Sensor Component](https://github.com/rwaldron/johnny-five/blob/master/docs/sensor.md)
- [Sensor - Photoresistor](https://github.com/rwaldron/johnny-five/blob/master/docs/photoresistor.md)
- [Sensor - Potentiometer](https://github.com/rwaldron/johnny-five/blob/master/docs/potentiometer.md)
- [Sensor - Force Sensitive Servo Controller](https://github.com/rwaldron/johnny-five/blob/master/docs/sensor-fsr-servo.md)
- [Sensor - Slide Potentiometer](https://github.com/rwaldron/johnny-five/blob/master/docs/sensor-slider.md)
- [Sensor - Slide Potentiometer Servo Controller](https://github.com/rwaldron/johnny-five/blob/master/docs/slider-servo-control.md)

<!--remove-end-->
