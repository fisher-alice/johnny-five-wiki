The `Sensor` class constructs objects that represent a single analog Sensor attached to the physical board. This class is intentionally generic and will work well with many types of sensors, including:

- Linear Potentiometer (Slider)
- Rotary Potentiometer (Knob)
- Thermistor (Temperature)
- Flex Sensitive Resistor
- Pressure Sensitive Resistor
- Force Sensitive Resistor
- Hall Sensor
- Tilt Sensor
- Photoresistor 
- Light Dependent Resistor

...And more, see [examples](https://github.com/rwldrn/johnny-five/wiki/Sensor#examples)

### Parameters

- **pin** A Number or String address for the Sensor pin (analog).
```js
var sensor = new five.Sensor("A0");
```
Tinkerkit: 
```js
// Attached to "Output 0"
var sensor = new five.Sensor("O0");
```


- **options** An object of property parameters.
<table>
  <thead>
    <tr>
      <th>Property Name</th>
      <th>Description</th>
      <th>Required</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>pin</td>
      <td>The String address of the pin the sensor is attached to, ie. "A0" or "O1"</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>freq</td>
      <td>The frequency in ms of data events. Defaults to 25ms</td>
      <td>no</td>
    </tr>
    <tr>
      <td>threshold</td>
      <td>The change threshold (+/- value). Defaults to 1</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
```js
// Create a temperature sensor...
// 
//   - attached to pin "A0"
//   - emits data events every 250ms
//   - emits change events when the temperature has changed 5 degrees.
//
var temp = new five.Sensor({
  pin: "A0", 
  freq: 250, 
  threshold: 5
});
```

### Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pin: The pin address that the Sensor is attached to
  threshold: The change threshold (+/- value). Defaults to 1

  boolean: Voltage value scaled to a boolean. READONLY
  raw: Voltage value (0-1023). READONLY
  constrained: Voltage reading constrained to 8 bit values (0-255). READONLY
  scaled: Voltage reading scaled to user defined range. READONLY
  value: Voltage reading, either raw or scaled. READONLY
}
```



### Usage
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  // Create a new `sensor` hardware instance.
  var sensor = new five.Sensor("A0");

  sensor.scale([ 0, 10 ]).on("read", function() {
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


- **booleanAt(barrier)** Set a midpoint barrier value used to calculate returned value of the .boolean property. Defaults to 528
```js
var sensor = new five.Sensor("A0");

// Voltage readings less then 100 will result in the value of the `boolean` property being false.
// Voltage readings greater then 100 will result in the value of the `boolean` property being true.
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

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the "read" event)

## Examples
- [Accelerometer](https://github.com/rwldrn/johnny-five/blob/master/docs/accelerometer.md)
- [Accelerometer Pan Tilt](https://github.com/rwldrn/johnny-five/blob/master/docs/accelerometer-pan-tilt.md)
- [Photoresistor](https://github.com/rwldrn/johnny-five/blob/master/docs/photoresistor.md)
- [Potentiometer](https://github.com/rwldrn/johnny-five/blob/master/docs/potentiometer.md)
- [Sensor](https://github.com/rwldrn/johnny-five/blob/master/docs/sensor.md)
- [Sensor Fsr Servo](https://github.com/rwldrn/johnny-five/blob/master/docs/sensor-fsr-servo.md)
- [Sensor Fsr](https://github.com/rwldrn/johnny-five/blob/master/docs/sensor-fsr.md)
- [Sensor Ir Led Receiver](https://github.com/rwldrn/johnny-five/blob/master/docs/sensor-ir-led-receiver.md)
- [Sensor Slider](https://github.com/rwldrn/johnny-five/blob/master/docs/sensor-slider.md)
- [Slider Log](https://github.com/rwldrn/johnny-five/blob/master/docs/slider-log.md)
- [Slider Pan](https://github.com/rwldrn/johnny-five/blob/master/docs/slider-pan.md)
- [Slider Servo Control](https://github.com/rwldrn/johnny-five/blob/master/docs/slider-servo-control.md)
- [Sensor Temperature](https://github.com/rwldrn/johnny-five/blob/master/docs/sensor-temperature.md)
