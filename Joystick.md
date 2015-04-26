![](http://i.gyazo.com/6bfe16c39837476090a147ae69f6d48b.png)

The `Joystick` class constructs objects that represent a single Joystick sensor attached to the physical board.

This list will continue to be updated as more Joystick devices are confirmed.

## Parameters

- **General Options**

  | Property | Type          | Value/Description                         | Default | Required |
  |---------------|---------------|----------|-------------------------------------|---------|
  | pins          | Array of Pins | `["A*", ...]`. Analog pins connected to X and Y |    | yes      |

## Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pins: The pins defined for X and Y.
  x: -1, 1. READONLY
  y: -1, 1. READONLY
}
```

## Component Initialization

#### Analog

```js
new five.Joystick({
  // [ x, y ]
  pins: ["A0", "A1"]
});
```

![Joystick](https://github.com/rwaldron/johnny-five/raw/master/docs/images/joystick.jpg)

![Adafruit Joystick](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/joystick-adafruit.png)

![SparkFun Joystick](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/joystick-sparkfun.png)
## API

There are no special API functions for this class.

## Events

- **axismove** Is an alias for "change".

- **change** The "change" event is emitted whenever the value of the gyro changes more then the threshold value allows.

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the "read" event)
