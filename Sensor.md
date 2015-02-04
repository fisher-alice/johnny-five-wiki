The `Sensor` class constructs objects that represent a single analog sensor component attached to the physical board. This class is intentionally generic and will work well with many types of sensors, including (but not limited to):

- Linear Potentiometer
- Rotary Potentiometer
- Flex Sensitive Resistor
- Pressure Sensitive Resistor
- Force Sensitive Resistor
- Hall Sensor
- Tilt Sensor
- Photoresistor/Light Dependent Resistor

...And more, see [examples](https://github.com/rwaldron/johnny-five/wiki/Sensor#examples)

## Parameters

- **pin** A Number or String address for the Sensor pin (analog).

- **options** An object of property parameters.
  <table>
    <thead>
      <tr>
        <th>Property Name</th>
        <th>Type</th>
        <th>Value(s)</th>
        <th>Description</th>
        <th>Required</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>pin</td>
        <td>Number, String</td>
        <td>"A0", "I1", 5 (Any pin on board)</td>
        <td>The Number or String address of the pin the sensor is attached to, ie. "A0" or "I1"</td>
        <td>yes</td>
      </tr>
      <tr>
        <td>freq</td>
        <td>Number</td>
        <td>Milliseconds</td>
        <td>The frequency in ms of data events. Defaults to 25ms</td>
        <td>no</td>
      </tr>
      <tr>
        <td>threshold</td>
        <td>Number</td>
        <td>Any</td>
        <td>The change threshold (+/- value). Defaults to 1</td>
        <td>no</td>
      </tr>
    </tbody>
  </table>


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

```js
var sensor = new five.Sensor("A0");
```


```js
// Create a sensor...
// 
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


### Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var sensor = new five.Sensor("A0");

  sensor.scale([ 0, 10 ]).on("data", function() {
    console.log( this.value );
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

## Examples

- [Sensor Component](https://github.com/rwaldron/johnny-five/blob/master/docs/sensor.md)
- [Sensor - Photoresistor](https://github.com/rwaldron/johnny-five/blob/master/docs/photoresistor.md)
- [Sensor - Potentiometer](https://github.com/rwaldron/johnny-five/blob/master/docs/potentiometer.md)
- [Sensor - Force Sensitive Servo Controller](https://github.com/rwaldron/johnny-five/blob/master/docs/sensor-fsr-servo.md)
- [Sensor - Slide Potentiometer](https://github.com/rwaldron/johnny-five/blob/master/docs/sensor-slider.md)
- [Sensor - Slide Potentiometer Servo Controller](https://github.com/rwaldron/johnny-five/blob/master/docs/slider-servo-control.md)
