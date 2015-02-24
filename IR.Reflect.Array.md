The `IR.Reflect.Array` class constructs an array of analog (digital arrays are not currently supported) reflectance sensors like the [QTR-8A](http://www.pololu.com/product/960) from Pololu.

### Parameters

- **pins** An array Numbers or Strings for the analog sensor pins.

- **emitter** The pin where the emitter LED is attached.  For these arrays, there is one pin that controls all of the emitters.

```js
var eyes = new five.IR.Reflect.Array({
  emitter: 13,
  pins: ["A0", "A1", "A2", "A3", "A4", "A5"]
});
```

- **options** An object of property parameters.

  | Property | Type                    | Value(s)                           | Description                                          | Required |
  |---------------|-------------------------|------------------------------------|------------------------------------------------------|----------|
  | pins          | Array of Numbers, Strings | `[“A0”, “I1”, 5]` (Any pin on board) | The pins that the sensors are connected to           | yes      |
  | emitter       | Number, String          | “A0”, 13 (Any pin on board)        | The pin that the light emitter is connected to       | yes      |
  | freq          | Number                  | Milliseconds                       | The frequency in ms of data events. Defaults to 25ms | no       |


### Shape

```
{
  id: A user definable id value. Defaults to a generated uid
  pins: The pin addresses that the sensors are attached to

  isOn: Are the emitters on? READONLY
  isCalibrated: Does the array have calibration data? READONLY
  isOnLine: Is the array currently tracking a line? READONLY, isCalibrated===true
  sensors: An array of sensor objects that represent each sensor in the array. READONLY
  calibration: The current calibration data. READONLY
  raw: An array of raw values from all the sensors. READONLY
  values: If calibrated, an array of calibrated values (between 0 and 1000).  If not calibrated, raw data (between 0 and 1023). READONLY
  line: If calibrated, a value between 0 and (n-1)*1000+1. For example, 6 sensors will have a line between 0 and 5001. READONLY
}
```



### Usage
```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {

  // Create a new `reflectance` hardware instance.
  var eyes = new five.IR.Reflect.Array({
    emitter: 13,
    pins: ["A0", "A1", "A2"], // any number of pins
    freq: 25
  });

  eyes.on('data', function(err, rawValues) {
    console.log( "Raw Values: ", rawValues );
  });

  eyes.on('line', function(err, lineValue) {
    console.log( "Line Position: ", lineValue);
  });

  eyes.enable();
});
```


## API

- **enable()** Turn the light emitters on.

```js
var eyes = new five.Sensor({
  emitter: 13,
  pins: ["A0", "A1"]
});

eyes.enable();
```

- **disable()** turn the light emitters off

```js
var eyes = new five.Sensor({
  emitter: 13,
  pins: ["A0", "A1"]
});

eyes.disable();
```

- **calibrate()** Read all of the sensors and store the min/max values.  Used to calculate calibrated values and line values.  You should call `calibrate()` multiple times.

```js
var eyes = new five.Sensor({
  emitter: 13,
  pins: ["A0", "A1"]
});

for(var i=0; i<100; i++) {
  eyes.calibrate();
}
```

- **calibrateUntil(predicate)** A convenience function that will call calibrate until a predicate function returns true.  This allows you to decide when calibration is done.  This trigger may be user input, time, or execution count.  You decide.

```js
var eyes = new five.Sensor({
  emitter: 13,
  pins: ["A0", "A1"]
});

var calibrating = true;
eyes.calibrateUntil(function() {
  return !calibrating;
});

setTimeout(function() { calibrating = false; }, 1000 ); // calibrate for one second
```

- **loadCalibration(calibration)** Prime the array with calibration data.  This allows you to load calibration data from a file.  You can get it from the device using the `calibration` after it has been calibrated.

```js
var eyes = new five.Sensor({
  emitter: 13,
  pins: ["A0", "A1"]
});

eyes.loadCalibration({
  min: [37, 39],
  max: [1010, 1015]
});
```

## Events

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. It includes an array of the RAW data from the sensors.

- **calibratedData** The "calibratedData" event is fired as frequently as the user defined `freq` will allow in milliseconds.  **It also requires that the device has been calibrated**.  It includes an array of calibrated values between 0 and 1000.

- **line** The "line" event is fired as frequently as the user defined `freq` will allow in milliseconds.  **It also requires that the devide has been calibrated**.  It is a single value between 0 and (n-1)*1000+1.  An array with 6 sensors will yield a value between 0 and 5001.

<!--remove-start-->
## Examples
- [Line Follower](https://github.com/rwaldron/johnny-five/blob/master/eg/line-follower.js)
- [Line Follower Video](https://www.youtube.com/watch?v=i6n4CwqQer0)

<!--remove-end-->
