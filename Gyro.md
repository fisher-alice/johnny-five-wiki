The `Gyro` class constructs objects that represent a single analog Gyro sensor attached to the physical board.

This list will continue to be updated as more Gyro devices are confirmed.

### Parameters


- **pins** An Array of String address values for the Gyro's x and y pin (analog only).
```js
var gyro = new five.Gyro(["A0", "A1"]);

var gyro = new five.Gyro({
  pins: ["A0", "A1"]
});
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
      <td>pins</td>
      <td>Array of Strings</td>
      <td>["A*"]</td>
      <td>The String analog pins that X and Y are attached to</td>
      <td>yes</td>
    </tr>

    <tr>
      <td>amplify</td>
      <td>Number</td>
      <td>--</td>
      <td>Currently unused</td>
      <td>no</td>
    </tr>
    <tr>
      <td>sensitivity</td>
      <td>Number</td>
      <td>Varies by device</td>
      <td>This value can be identified in the device's datasheet</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
```js
// Create an analog Gyro object:
// 
//   - attach X and Y to "A0" and "A1" respectively
//   - Use the LPR5150AL 4X sensitivity rating
//
var gyro = new five.Gyro({
  pins: ["A0", "A1"],
  sensitivity: 0.67 
});
```

### Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pins: The pins defined for X and Y.
  pitch: An object containing values for the pitch rate and angle. READONLY
  roll: An object containing values for the roll rate and angle. READONLY
  rate: And object containing the rate values of X and Y. READONLY
  x: Value of x axis. READONLY
  y: Value of y axis. READONLY
}
```



### Usage
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var gyro = new five.Gyro({
    pins: ["I0", "I1"],
    sensitivity: 0.67 // LPR5150AL 4X
  });

  gyro.on("change", function(err, data) {
    console.log("X raw: %d rate: %d", this.x, this.rate.x);
    console.log("Y raw: %d rate: %d", this.y, this.rate.y);
  });
});
```

## Events

- **change** The "change" event is emitted whenever the value of the gyro changes more then the threshold value allows.

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the "read" event)

## Examples

- [Tinkerkit Gyroscope](https://github.com/rwldrn/johnny-five/blob/master/docs/tinkerkit-gyroscope.md)

