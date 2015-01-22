The `Temperature` class constructs objects that represent a single Temperature sensor attached to the physical board.

Johnny-Five currently supports several kinds of Temperatures:

- Analog (with user-supplied `toCelsius(raw)` function)
- [LM35](http://www.ti.com/product/lm35)
- [TMP36](http://www.analog.com/en/mems-sensors/digital-temperature-sensors/tmp36/products/product.html)
- [DS18B20](http://www.maximintegrated.com/en/products/analog/sensors-and-sensor-interface/DS18B20.html) (requires [ConfigurableFirmata](https://github.com/firmata/arduino/tree/configurable))
- [MPU6050](http://www.invensense.com/mems/gyro/mpu6050.html) IMU
- [Grove Temperature](http://www.seeedstudio.com/depot/Grove-Temperature-Sensor-p-774.html)

This list will continue to be updated as more devices are confirmed.

### Parameters

- **General Options**
<table>
  <thead>
    <tr>
      <th>Property Name</th>
      <th>Type</th>
      <th>Value(s)</th>
      <th>Description</th>
      <th>Default</th>
      <th>Required</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>controller</td>
      <td>string</td>
      <td>"ANALOG" | "LM35" | "TMP36" | "DS18B20" | "MPU6050" | "GROVE"</td>
      <td>The Name of the controller to use</td>
      <td>"ANALOG"</td>
      <td>no</td>
    </tr>
    <tr>
      <td>pin</td>
      <td>string or int</td>
      <td>"A0", 2, etc</td>
      <td>The Name of the controller to use</td>
      <td>none</td>
      <td>Yes, for analog devices. No for digitial devices (MPU-6050 or DS18B20)</td>
    </tr>
    <tr>
      <td>toCelsius</td>
      <td>function</td>
      <td>function toCelsius(raw) {}</td>
      <td>A raw-to-celsius transform override</td>
      <td>controller-dependent</td>
      <td>no</td>
    </tr>
  </tbody>
</table>

```js
// Create an analog Temperature object:
var temperature = new five.Temperature({
  pin: "A0",
  toCelsius: function(raw) { // optional
    return (raw / sensivity) + offset;
  }
});
```

```js
// Create an MPU-6050 Temperature object:
//
//  - attach SDA and SCL to the I2C pins on your board (A4 and A5 for the Uno)
//  - specify the MPU6050 controller
var temperature = new five.Temperature({
  controller: "MPU6050"
});
```

### Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pin: The pins defined for X, Y, and Z.
  celsius: The temperature in celsius degrees. READONLY
  fahrenheit: The temperature in fahrenheit degrees. READONLY
  kelvin: The temperature in kelvin degrees. READONLY
}
```



### Usage
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var temperature = new five.Temperature({
    pin: "A0"
  });

  temperature.on("change", function(err, data) {
    console.log("celsius: %d", data.celsius);
    console.log("fahrenheit: %d", data.fahrenheit);
    console.log("kelvin: %d", data.kelvin);
  });
});
```

## API

There are no special API functions for this class.

## Events

- **change** The "change" event is emitted whenever the value of the temperature changes more then the threshold value allows.

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the "read" event)
