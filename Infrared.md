The `IR` class constructs objects that represent a single (I2C or analog) Infrared sensor attached to the physical board. This class can produce several IR relevant device instances, including:

- Motion
    - [Passive Infra-Red by Parallax, Digital](http://www.parallax.com/tabid/768/productid/83/default.aspx)
- Proximity
    - [GP2Y0D805Z0F, I2C](http://www.pololu.com/catalog/product/1132) (Default)
    - [GP2Y0A21YK, Analog](https://www.sparkfun.com/products/242)
    - [GP2D120XJ00F, Analog](https://www.sparkfun.com/products/8959)
    - [GP2Y0A02YK0F, Analog](https://www.sparkfun.com/products/8958)

### Parameters

- **pin** A Number pin address for Motion sensor.
```js
var motion = new five.IR.Motion(7);
```
```js
// I2C proximity
var proximity = new five.IR.Proximity("GP2Y0D805Z0F");

// Analog proximity
var proximity = new five.IR.Proximity("A0");
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
  </tbody>
</table>


### Shape

```
{ 
  board: A reference to the board object the Sensor is attached to
  id: A user definable id value. Defaults to a generated uid
  pin: The pin address that the Sensor is attached to
  mode: The Pin.MODE, Defaults to INPUT
  freq: The frequency in ms of data reads. Defaults to 25ms
  value: Digital state reading (0 or 1). READONLY
  isCalibrated: Boolean flag indicating calibration state of motion sensor 
}

```



### Usage
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  // Create a new `motion` hardware instance.
  var motion = new five.IR.Motion(7);

  motion.on("motionstart", function() {
    console.log( "Something moved!" );
  });

  motion.on("motionend", function() {
    console.log( "All quiet now..." );
  });
});
```

```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {
 
  // Uses default GP2Y0D805Z0F
  var prox = new five.IR.Proximity();

  motion.on("motionstart", function() {
    console.log( "Something moved!" );
  });

  motion.on("motionend", function() {
    console.log( "All quiet now..." );
  });
});
```

## Events

- **data** The "data" event is fired as frequently as the user defined freq will allow in milliseconds. ("data" replaced the "read" event)

- **motionstart** The "motionstart" event is fired when motion occurs within the observable range of the PIR/IR.Motion/IR.Proximity sensor

- **motionend** The "motionend" event is fired when motion has ceased within the observable range of the PIR/IR.Motion/IR.Proximity sensor.

- **calibrated** The "calibrated" event is fired when PIR/IR.Motion sensor is ready to detect movement/motion in observable range. (PIR/IR.Motion ONLY)

