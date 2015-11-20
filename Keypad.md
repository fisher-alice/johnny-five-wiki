![](http://i.imgur.com/Il23mnI.png)

The `Keypad` class constructs an object that represents a single Keypad attached to the board.

Supported Keypads:

- VKEY
  - [SparkFun](https://www.sparkfun.com/products/12080)
- Waveshare AD (Analog)
  - [Amazon](http://www.amazon.com/Waveshare-Accessory-buttons-controlled-keyboard/dp/B00KM6UXVS)
- MPR121
  - [SparkFun](https://www.sparkfun.com/products/12017)
  - [Adafruit](https://www.adafruit.com/products/2024)
- MPR121QR2
  - [SparkFun](https://www.sparkfun.com/products/12013)
- Grove QTouch (AT42QT1070)
  - [Seeed Studio](http://www.seeedstudio.com/depot/Grove-Touch-Sensor-p-747.html)
  - [Adafruit](https://www.adafruit.com/products/1362)

## Parameters

- **General Options**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | controller    | string | AT42QT1070, MPR121, MPR121QR2, QTOUCH, VKEY, ANALOG. The Name of the controller to use | ANALOG | Yes       |
  | keys    | array | Mapping of key values |  By Device | No       |
  | holdtime      | Number         | Time in milliseconds that the button must be held until emitting a "hold" event. | 500ms                                                    | no       |
  </span>

- **VKEY Options (`controller: "VKEY"`)** 

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | pin    | number, string | Analog pin | | Yes |

- **ANALOG Options (`controller: "ANALOG"`)** 

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | pin    | number, string | Analog pin | | Yes |
  | length | number | Number of keys (required only if `keys` is not specified) | | Yes |

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

#### MPR121QR2

```javascript
new five.Keypad({
   controller: "MPR121QR2"
});
```

![](http://johnny-five.io/img/breadboard/keypad-MPR121QR2.png)

#### QTOUCH / AT42QT1070

```javascript
new five.Keypad({
  controller: "QTOUCH", // or "AT42QT1070"
});
```

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
    ["", "△", ""],
    ["◁", "☐", "▷"],
    ["", "▽", ""]
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
>> △
▽
◁
▷
☐
```

If your program prefers a flat array, the equivalent program would be: 

```js
var keypad = new five.Keypad({
  controller: "MPR121QR2",
  keys: ["", "△", "", "◁", "☐", "▷", "", "▽", ""]
});

keypad.on("press", function(event) {
  if (event.which) {
    console.log(event.which);
  }
});
```


## Events

- **hold** The key has been held for `holdtime` milliseconds

- **down**, **press** The key has been pressed.

- **up**, **release** The key has been released.

The following data object is emitted with each event:

| Key Name | Description |
|---------------| ----------- |
| `which` | Value of key pressed from `keys` array |
| `timestamp` | Timestamp at time of key press |

<!--remove-start-->

## Examples

- [Atmel AT42QT1070 (Q Touch)](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-AT42QT1070.md)
- [MPR121](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-MPR121.md)
- [MPR121QR2](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-MPR121QR2.md)
- [Grove QTouch](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-QTOUCH.md)
- [Waveshare AD](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-analog-ad.md)
- [VKEY](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-analog-vkey.md)

<!--remove-end-->
