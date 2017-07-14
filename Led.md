![](http://i.gyazo.com/b804a48064ea4db0bdb8fd7c5af352de.png)

The `Led` class constructs objects that represent a single Led attached to the physical board.

See also: 

- [Led.Digits](https://github.com/rwaldron/johnny-five/wiki/led.digits)
- [Led.Matrix](https://github.com/rwaldron/johnny-five/wiki/led.matrix)
- [Led.RGB](https://github.com/rwaldron/johnny-five/wiki/led.rgb)
- [Leds](https://github.com/rwaldron/johnny-five/wiki/leds)


## Parameters

- **pin** A Number or String address for the Led pin (digital/PWM).
For Leds that only have on/off states, use a digital pin:

  ```js
  var digital = new five.Led(13);
  ```

  For Leds that have on/off states, as well as interval or color related state (Pulse, Brightness, RGB, etc), use a PWM pin (usually demarcated by either a "~" or "#" next to the pin number on the actual board).

  ```js
  // Look for "~", ie. ~11
  var pwm = new five.Led(11);
  ```

- **General Options** An object of property parameters.
  <span class="abbreviate-table">

  | Property | Type    | Value/Description                                                                                     | Default | Required |
  |---------------|---------|-----------------------------------------------------------------------------------------------------------------------|----------|----------|
  | pin           | Number  | Digital Pin. The Number address of the pin the led is attached to                                            |  | yes      |
  | controller    | String  | "DEFAULT", "PCA9685". Controller interface type. | `"DEFAULT"`                                      | no       |
  </span>

- **PCA9685 Options (controller: "PCA9685")** An object of property parameters.
  <span class="abbreviate-table">

  | Property | Type    | Value/Description                                                                                   | Default | Required |
  |---------------|---------|-----------------------------------------------------------------------------------------------------------------------|----------|----------|
  | address       | Number  | Address for I2C devices. | `0x04` | no       |
  </span>

## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `pin` | The pin address that the Led is attached to | No |

## Component Initialization


#### LED 
```js
// Just a pin
var led = new five.Led(13);

// Options object with pin property
new five.Led({
  pin: 13
});
```

![LED](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-13.png)

#### LED PCA9685
```js
new five.Led({
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

- **strobe(ms, callback)** Strobe/Blink the Led on/off in phases over `ms` with an optional callback. This is an **interval** operation and can be stopped by calling `led.stop()`, however that will not necessarily turn it "off". The callback will be invoked every time the Led turns on or off. Defaults to 100ms.

  ```js
  var led = new five.Led(13);

  // Strobe on-off in 500ms phases
  led.strobe(500);
  ```

- **blink(ms, callback)** alias to strobe.

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

- **fade(brightness, ms, callback)** Fade from current brightness to `brightness` over `ms` with an optional callback. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". This operation will only work with Leds attached to PWM pins.

  ```js
  var led = new five.Led(11);

  // Fade to half brightness over 2 seconds
  led.fade(128, 2000);
  ```

- **fade(animation options)** Control the fading of an LED with [`Animation` options](https://github.com/rwaldron/johnny-five/wiki/animation#segment-properties).

  ```js
  var led = new five.Led(11);

  led.fade({
    easing: "outSine",
    duration: 1000,
    cuePoints: [0, 0.2, 0.4, 0.6, 0.8, 1],
    keyFrames: [0, 250, 25, 150, 100, 125],
    onstop: function() {
      console.log("Animation stopped");
    }
  });
  ```


- **fadeIn(ms, callback)** Fade in from current brightness over `ms` with an optional callback. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". This operation will only work with Leds attached to PWM pins.

  ```js
  var led = new five.Led(11);

  // Fade in over 500ms.
  led.fadeIn(500);
  ```

- **fadeOut(ms, callback)** Fade out from current brightness over `ms` with an optional callback. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". This operation will only work with Leds attached to PWM pins.

  ```js
  var led = new five.Led(11);

  // Fade out over 500ms.
  led.fadeOut(500);
  ```


- **pulse(ms, callback)** Pulse the Led in phases from on to off over `ms` time, with an optional callback. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". The callback will be invoked every time the Led is fully on or off. This operation will only work with Leds attached to PWM pins.

  ```js
  var led = new five.Led(11);

  // Pulse from on to off in 500ms phases
  led.pulse(500);
  ```

- **pulse(animation options)** Control the pulse of an LED with [`Animation` options](https://github.com/rwaldron/johnny-five/wiki/animation#segment-properties).  

  ```js
  var led = new five.Led(11);

  led.pulse({
    easing: "linear",
    duration: 3000,
    cuePoints: [0, 0.2, 0.4, 0.6, 0.8, 1],
    keyFrames: [0, 10, 0, 50, 0, 255],
    onstop: function() {
      console.log("Animation stopped");
    }
  });
  ```

- **stop()** For **interval** operations (`blink`, `pulse`, etc.), call `stop` to stop the interval. `stop` does not necessarily turn "off" the Led, in order to fully shut down an Led, a program must call `stop().off()`.

  ```js
  var led = new five.Led(11);

  // Blink from on to off in 500ms phases
  led.blink(500);

  ...Sometime later...

  led.stop();

  ```




## Events

Led objects are output only and therefore do not emit any events.

<!--remove-start-->

## Examples
- [Led Matrix](https://github.com/rwaldron/johnny-five/blob/master/docs/led-matrix.md)
- [Led Fade](https://github.com/rwaldron/johnny-five/blob/master/docs/led-fade.md)
- [Led Pulse](https://github.com/rwaldron/johnny-five/blob/master/docs/led-pulse.md)
- [Led Rgb](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rgb.md)
- [Led Rainbow](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rainbow.md)
- [Led Rgb PCA9685](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rgb-PCA9685.md)

<!--remove-end-->
