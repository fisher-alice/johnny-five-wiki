The `Sonar` class constructs objects that represent a single analog or I2C Sonar sensor attached to the physical board. This class works with: 

- Maxbotix Analog Sonar 
  - Any

- Devantech I2C Sonar
  - SRF02
  - SRF08
  - SRF10


This list will continue to be updated as more Sonar devices are confirmed.

### Parameters


- **pin** A String address for the Sonar pin (analog only).
```js
var sonar = new five.Sonar("A0");
```



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
      <td>The Number or String address of the pin the sonar is attached to, ie. "A0" or "I1"</td>
      <td>yes (for analog)</td>
    </tr>
    <tr>
      <td>device</td>
      <td>String</td>
      <td>"SRF02", "SRF08"</td>
      <td>A String model id of a Sonar device (I2C only), ie. "SRF02", "SRF08", "SRF10"</td>
      <td>yes (for I2C)</td>
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




### Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pin: The pin address that the Sonar is attached to
  cm: Value of current reading in centimeters. READONLY
  inches: Value of current reading in inches. READONLY
  value: cm: Value of current reading.
}
```

## Component Initialization

```js
// Create an analog Sonar object:
// 
//   - attached to pin "A0"
//
var sonar = new five.Sonar({
  pin: "A0", 
});
```





```js
// Create an I2C Sonar object:
// 
//   - use device model "SRF10"

var sonar = new five.Sonar({
  controller: "SRF10"
});
```

![Sonar I2C](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/sonar-srf10.png)


### Usage
```js
var five = require("../lib/johnny-five.js");
var board = new five.Board();

board.on("ready", function() {
  var sonar = new five.Sonar("A0");

  sonar.on("data", function() {
    console.log("inches     : " + this.in);
    console.log("centimeters: " + this.cm);
    console.log("-----------------------");
  });
});

```


## API

- **within(range[, unit], handler)** When value is within the provided range, execute callback. 
```js
var sonar = new five.Sonar({
  device: "SRF10"
});

sonar.within([ 5, 10 ], "inches", function() {
  
  // This is called when the sonar detects an object 5-10 inches away

});

```

## Events

- **change** The "change" event is emitted whenever the value of the sonar changes more then the threshold value allows.

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the "read" event)

## Examples
