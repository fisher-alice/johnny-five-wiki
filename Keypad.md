![](http://i.imgur.com/Il23mnI.png)

The `Keypad` class constructs an object that represents a single Keypad attached to the board. `Touchpad` is an alias for `Keypad`.

Supported Keypads:

- VKEY
  - [SparkFun](https://www.sparkfun.com/products/12080?utm_source=j5)
- Waveshare AD (Analog)
  - [Amazon](http://www.amazon.com/Waveshare-Accessory-buttons-controlled-keyboard/dp/B00KM6UXVS?utm_source=j5)
- MPR121
  - [SparkFun](https://www.sparkfun.com/products/9695?utm_source=j5) (Breakout)
  - [SparkFun](https://www.sparkfun.com/products/12017?utm_source=j5) (Keypad)
  - [Adafruit](https://www.adafruit.com/products/1982?utm_source=j5) (Breakout)
  - [Adafruit](https://www.adafruit.com/products/2024?utm_source=j5) (Shield)
- MPR121QR2
  - [SparkFun](https://www.sparkfun.com/products/12013?utm_source=j5)
- Grove QTouch (AT42QT1070)
  - [Seeed Studio](http://www.seeedstudio.com/depot/Grove-Touch-Sensor-p-747.html?utm_source=j5)
- 12 Button Keypad (requires I2C backpack)
  - [Adafruit](https://www.adafruit.com/products/1824?utm_source=j5)
    - This likely works with the 3x4 membrane as well, as long as the signal lines are connected correctly
  - [Sparkfun](https://www.sparkfun.com/products/8653?utm_source=j5)


## Parameters

- **General Options**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | controller    | string | AT42QT1070, MPR121, MPR121_\* (variants below), QTOUCH, VKEY, ANALOG. The Name of the controller to use | ANALOG | Yes       |
  | keys    | array | Mapping of key values |  By Device | No       |
  | holdtime      | Number         | Time in milliseconds that the button must be held until emitting a "hold" event. | 500ms                                                    | no       |
  </span>

- **MPR121 Options (`controller: "MPR121" | "MPR121_*"`)**

  In addition to the General Options, the MPR121 supports the following: 

  <span class="abbreviate-table">

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | sensitivity | Object | `{ press: 0-1, release: 0-1 }`. Use a generic sensitivity for all pads | \* | no |
  | sensitivity | Array | An array of `{ press: 0-1, release: 0-1 }` objects | \* | no |
  </span>




  ##### \* Sensitivity
  
  The sensitivity setting allows the calibration of the keypad to compensate for factors such as humidity, temperature, etc. A too high sensitivity % might result in false positives; in some cases, just getting close to it might be enough to trigger a press.

  | Type | Default |
  |------|------|
  | press | 0.95 |
  | release | 0.975 |

  Note: `release` should always have a higher sensitivity than `press`. 

  ##### MPR121 Variants

  | For this MPR121 variant... | Use this controller |
  |----------------------------|---------------------|
  | ![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/images/mpr121_breakout_blue.jpg) | `MPR121` |
  | ![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/images/mpr121_breakout_red.jpg) | `MPR121` |
  | ![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/images/mpr121_keypad.jpg) | `MPR121_KEYPAD` |
  | ![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/images/mpr121_shield.jpg) | `MPR121_SHIELD` |
  | ![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/images/mpr121qr2_touch_shield.jpg) | `MPR121QR2_SHIELD` |

- **VKEY Options (`controller: "VKEY"`)**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | pin    | number, string | Analog pin | | Yes |
  </span>

- **ANALOG Options (`controller: "ANALOG"`)**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | pin    | number, string | Analog pin | | Yes |
  | length | number | Number of keys (required only if `keys` is not specified) | | Yes |
  </span>

## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `value` | Index of the pressed key | No |
| `target` | Mapped key value of pressed key | No |

## Component Initialization

#### ANALOG

```javascript
new five.Keypad({
  pin: "A0",
  length: 16
});
```

#### VKEY

```javascript
new five.Keypad({
  controller: "VKEY",
  pin: "A0",
});
```

#### MPR121

```javascript
new five.Keypad({
   controller: "MPR121"
});
```

#### MPR121QR2_SHIELD

```javascript
new five.Keypad({
   controller: "MPR121QR2_SHIELD"
});
```

![](http://johnny-five.io/img/breadboard/keypad-MPR121QR2.png)

#### MPR121_KEYPAD

```javascript
new five.Keypad({
   controller: "MPR121_KEYPAD"
});
```

![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/keypad-MPR121_KEYPAD.png)

#### QTOUCH / AT42QT1070

```javascript
new five.Keypad({
  controller: "QTOUCH", // or "AT42QT1070"
});
```

Note: the [Adafruit: Standalone 5-Pad Capacitive Touch Sensor Breakout - AT42QT1070](https://www.adafruit.com/products/1362?utm_source=j5) will not work with the `Keypad` class, it has i2c disabled and must be used with the `Button` class instead.

#### 3x4 Keypad

This requires using a Nano Backpack. It's possible to use a 3x4 membrane, as long as it's pins are connected correctly: 

| Row | Nano Pin |
| --- | -------- |
| 1 | 7 |
| 2 | 2 |
| 3 | 3 |
| 4 | 5 |


| Col | Nano Pin |
| --- | -------- |
| 1 | 6 |
| 2 | 8 |
| 3 | 4 |


```javascript
new five.Keypad({
  controller: "3X4_I2C_NANO_BACKPACK"
});
```

![](http://johnny-five.io/img/breadboard/keypad-3X4_I2C_NANO_BACKPACK.png)

#### 4x4 Keypad

Similar to the 3x4 Keypad, this requires a Nano Backpack. However, it uses an extra pin for the 4th column of keys. Connect the pins according to the rows and columns to get the correct output.

| Row | Nano Pin |
| --- | -------- |
| 1 | 7 |
| 2 | 2 |
| 3 | 3 |
| 4 | 5 |


| Col | Nano Pin |
| --- | -------- |
| 1 | 6 |
| 2 | 8 |
| 3 | 4 |
| 4 | 9 |


```javascript
new five.Keypad({
  controller: "4X4_I2C_NANO_BACKPACK"
});
```

![](http://johnny-five.io/img/breadboard/keypad-4X4_I2C_NANO_BACKPACK.png)

### Usage

```javascript
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  // MPR121 3x4 Capacitive Touch Pad
  var keypad = new five.Keypad({
    controller: "MPR121",
    keys: [
      ["!", "@", "#"],
      ["$", "%", "^"],
      ["&", "-", "+"],
      ["_", "=", ":"]
    ]
  });

  ["change", "press", "hold", "release"].forEach(function(eventType) {
    keypad.on(eventType, function(data) {
      console.log("Event: %s, Target: %s", eventType, data.which);
    });
  });
});
```

#### A Note About The `keys` Option

The `keys` option allows the assignment of _any_ value to _any_ key/button on the device. The value of `keys` may be a flat array of strings, or an array of arrays where the nested array are "rows" on the device. The values given in the `keys` array are the values that the instance will report for key/button presses.

Using the `MPR121QR2` to illustrate `keys`:

![](http://johnny-five.io/img/breadboard/keypad-MPR121QR2.png)


The buttons are labelled `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8`, `9`. If the instance of `Keypad` is created **without** an explicit `keys` property, the output will be _exactly_ those numbers; ie. pressing `1` will result in an event whose data object's `which` property is the value `1` (and so on):


```js
var keypad = new five.Keypad({
  controller: "MPR121QR2"
});

keypad.on("press", function(event) {
  console.log("Which button?", event.which);
});
```

However, the `keys` property allows a program to assign **any** value to be the resulting value (because who knows&mdash;you may want to use symbols!)

```js
var keypad = new five.Keypad({
  controller: "MPR121QR2",
  keys: [
    ["", "???", ""],
    ["???", "???", "???"],
    ["", "???", ""]
  ]
});

keypad.on("press", function(event) {
  if (event.which) {
    console.log(event.which);
  }
});
```

Pressing `2`, `8`, `4`, `6`, `5` would result in the following output:

```
$ node eg/keypad-symbol-MPR121QR2.js
1448040971577 Device(s) /dev/cu.usbmodem1411
1448040971586 Connected /dev/cu.usbmodem1411
1448040975171 Repl Initialized
>> ???
???
???
???
???
```

If your program prefers a flat array, the equivalent program would be:

```js
var keypad = new five.Keypad({
  controller: "MPR121QR2",
  keys: ["", "???", "", "???", "???", "???", "", "???", ""]
});

keypad.on("press", function(event) {
  if (event.which) {
    console.log(event.which);
  }
});
```


## Events

- **change** Emitted for **hold**, **down**, **up**

- **hold** The key or keys have been held for `holdtime` milliseconds

- **down**, **press** The key or keys have been pressed.

- **up**, **release** The key or keys have been released.

The following data object is emitted with each event:

| Key Name | Description |
|---------------| ----------- |
| `which` | An array containing keys of _which_ buttons or pads are pressed. |
| `timestamp` | Timestamp at time of event |

<!--remove-start-->

## Examples

- [Atmel AT42QT1070 (Q Touch)](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-AT42QT1070.md)
- [MPR121](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-MPR121.md)
- [MPR121QR2](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-MPR121QR2.md)
- [Grove QTouch](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-QTOUCH.md)
- [Waveshare AD](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-analog-ad.md)
- [VKEY](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-analog-vkey.md)

<!--remove-end-->
