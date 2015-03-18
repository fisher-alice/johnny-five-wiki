The `Led` class constructs objects that represent a single Led attached to the physical board.

See also: 

- [Led.Digits](https://github.com/rwaldron/johnny-five/wiki/Led.Digits)
- [Led.Matrix](https://github.com/rwaldron/johnny-five/wiki/Led.Matrix)
- [Led.RGB](https://github.com/rwaldron/johnny-five/wiki/Led.RGB)


## Parameters

- **pin** A Number or String address for the Led pin (digital/PWM).
For Leds that only have on/off states, use a digital pin:

  ```js
  var digital = new five.Led(13);
  ```

  For Leds that have on/off states, as well as inverval or color related state (Pulse, Brightness, RGB, etc), use a PWM pin (usually demarcated by either a "~" or "#" next to the pin number on the actual board).

  ```js
  // Look for "~", ie. ~11
  var pwm = new five.Led(11);
  ```

- **options** An object of property parameters.

  | Property | Type    | Value(s)             | Description                                                                                     | Required |
  |---------------|---------|----------------------|-------------------------------------------------------------------------------------------------|----------|
  | pin           | Number  | Any Digital Pin      | The Number address of the pin the led is attached to                                            | yes      |
  | type          | String  | "OUTPUT", "PWM"      | For most cases, this can be omitted; the type will be inferred based on the pin address number. | no       |
  | controller    | String  | "DEFAULT", "PCA9685" | Controller interface type. Defaults to `"DEFAULT"`.                                           | no       |
  | address       | Number  | 0x40                 | Address for I2C devices. Defaults to `0x40`.                                                  | no       |

## Shape

```js
{ 
  id: ...A user definable id value. Defaults to null
  pin: ...The pin address that the Led is attached to
}
```

## Component Initialization


#### LED 
```js
// Just a pin
var led = new five.Led(13);

// Options object with pin property
var led = new five.Led({
  pin: 13
});
```

![LED](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led.png)

#### LED PCA9685
```js
var led = new five.Led({
  controller: "PCA9685",
  pin: 0, 
});
```

![PCA9685](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-PCA9685.png)

## Usage

```js
// Blink an LED
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var led = new five.Led(13);
  led.blink();
});
```

Tinkerkit: 

```js
// Attached to "Output 0"
var digital = new five.Led("O0");
```

I2C Led (i.e. via Adafruit PWM controller)

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var led = new five.Led({
    controller: "PCA9685",
    pin: 0,
  });

  led.blink()
});
```

## API

- **on()** Turn the led on. 

  ``` js
  var led = new five.Led(13);

  led.on();
  ```

- **off()** Turn the led off. If a led is strobing, it will not stop. Use `led.stop().off()` to turn off a led while strobing.

  ```js
  var led = new five.Led(13);

  led.off();
  ```

- **toggle()** Toggle the current state, if _on_ then turn _off_, if _off_ then turn _on_.

  ```js
  var led = new five.Led(13);

  led.toggle();
  ```

- **strobe(ms)** Strobe/Blink the Led on/off in phases over `ms`. This is an **interval** operation and can be stopped by calling `led.stop()`, however that will not necessarily turn it "off". Defaults to 500ms.

  ```js
  var led = new five.Led(13);

  // Strobe on-off in 500ms phases
  led.strobe(500);
  ```

- **blink(ms)** alias to strobe.

  ```js
  var led = new five.Led(13);

  // Strobe on-off in 500ms phases
  led.blink(500);
  ```


- **brightness(0-255)** Set the brightness of led. This operation will only work with Leds attached to PWM pins. 

  ```js
  var led = new five.Led(11);

  // This will set the brightness to about half 
  led.brightness(128);
  ```

- **fade(brightness, ms)** Fade from current brightness to `brightness` over `ms`. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". This operation will only work with Leds attached to PWM pins.

  ```js
  var led = new five.Led(11);

  // Fade to half brightness over 2 seconds
  led.fade(128, 2000);
  ```

- **fadeIn(ms)** Fade in from current brightness over `ms`. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". This operation will only work with Leds attached to PWM pins.

  ```js
  var led = new five.Led(11);

  // Fade in over 500ms.
  led.fadeIn(500);
  ```

- **fadeOut(ms)** Fade out from current brightness over `ms`. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". This operation will only work with Leds attached to PWM pins.

  ```js
  var led = new five.Led(11);

  // Fade out over 500ms.
  led.fadeOut(500);
  ```


- **pulse(ms)** Pulse the Led in phases from on to off over `ms` time. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". This operation will only work with Leds attached to PWM pins.

  ```js
  var led = new five.Led(11);

  // Pulse from on to off in 500ms phases
  led.pulse(500);
  ```

- **stop(ms)** For **interval** operations, call `stop` to stop the interval. `stop` does not necessarily turn "off" the Led, in order to fully shut down an Led, a program must call `stop().off()`. This operation will only work with Leds attached to PWM pins.

  ```js
  var led = new five.Led(11);

  // Pulse from on to off in 500ms phases
  led.pulse(500);

  ...Sometime later...

  led.stop();

  ```




## Events

Led objects are output only and therefore do not emit any events.

<!--remove-start-->

## Examples
- [Laser](https://github.com/rwaldron/johnny-five/blob/master/docs/laser.md)
- [Led Matrix](https://github.com/rwaldron/johnny-five/blob/master/docs/led-matrix.md)
- [Led Fade](https://github.com/rwaldron/johnny-five/blob/master/docs/led-fade.md)
- [Led On Off](https://github.com/rwaldron/johnny-five/blob/master/docs/led-on-off.md)
- [Led Pulse](https://github.com/rwaldron/johnny-five/blob/master/docs/led-pulse.md)
- [Led Rgb](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rgb.md)
- [Led Rainbow](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rainbow.md)
- [Led Strobe](https://github.com/rwaldron/johnny-five/blob/master/docs/led-strobe.md)
- [Led Rgb PCA9685](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rgb-PCA9685.md)
- [Seven Segment](https://github.com/rwaldron/johnny-five/blob/master/docs/seven-segment.md)

<!--remove-end-->
