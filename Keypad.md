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
| `which` | Value of key pressed from `keys` array | No |
| `timestamp` | Timestamp at time of key press | No |

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

## Events

- **hold** The key has been held for `holdtime` milliseconds

- **down**, **press** The key has been pressed.

- **up**, **release** The key has been released.

<!--remove-start-->

## Examples

- [Atmel AT42QT1070 (Q Touch)](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-AT42QT1070.md)
- [MPR121](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-MPR121.md)
- [MPR121QR2](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-MPR121QR2.md)
- [Grove QTouch](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-QTOUCH.md)
- [Waveshare AD](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-analog-ad.md)
- [VKEY](https://github.com/rwaldron/johnny-five/blob/master/docs/keypad-analog-vkey.md)

<!--remove-end-->
