

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
   controller: "MPR121",
   keys: ["!", "@", "#", "$", "%", "^", "&", "-", "+", "_", "=", ":"]
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
## Events

- **hold** The key has been held for `holdtime` milliseconds

- **down**, **press** The key has been pressed.

- **up**, **release** The key has been released.