The `Led.RGB` class constructs objects that represent an RGB Led.

## Parameters

- **pins** An Array containing the pins **red**, **green** and **blue**. All pins must support PWM.
  <span class="abbreviate-table">

  | Index | Type   | Value/Description              | Default | Required |
  |-------|--------|--------------------------------|---------|----------|
  | 0     | Number | PWM capable to control red     |         | yes      |
  | 1     | Number | PWM capable to control green   |         | yes      |
  | 2     | Number | PWM capable to control blue    |         | yes      |
  </span>

- **options**
  <span class="abbreviate-table">

  | Property | Type    | Value/Description                                                                                                     | Default | Required |
  |----------|---------|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------|----------|
  | pins     | Object  | See **pins** table below | | Yes (either)      |
  | pins     | Array  | See **pins** table below | | Yes (either)      |
  | isAnode  | Boolean | `true`, `false`. Set this to true to indicate the LED is a common anode LED. Defaults to false, indicating a common cathode LED. | | no       |
  | controller  | String  | DEFAULT, PCA9685, BLINKM. Controller interface type. | `"DEFAULT"` | no |                                           
  </span>

  * **pins** (Object)
    <span class="abbreviate-table">

    | Property | Type   | Value/Description              | Default | Required |
    |----------|--------|--------------------------------|---------|----------|
    | red      | Number | PWM capable to control **red**   |  | yes      |
    | green    | Number | PWM capable to control **green** |  | yes      |
    | blue     | Number | PWM capable to control **blue**  |  | yes      |
    </span>

  * **pins** (Array)
    <span class="abbreviate-table">

    | Index | Type   | Value/Description              | Default | Required |
    |-------|--------|--------------------------------|---------|----------|
    | 0     | Number | PWM capable to control **red**   |  | yes      |
    | 1     | Number | PWM capable to control **green** |  | yes      |
    | 2     | Number | PWM capable to control **blue**  |  | yes      |
    </span>



## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `red` | `Led` object | No |
| `green` | `Led` object | No |
| `blue` | `Led` object | No |
| `isAnode` | Boolean | Yes |

## Component Initialization


#### LED RGB
```js
// With Options object & pins object
new five.Led.RGB({
  pins: {
    red: 6,
    green: 5,
    blue: 3
  }
});

// With Options object & pins array
new five.Led.RGB({
  pins: [6, 5, 3]
});

// With pins array
new five.Led.RGB([6, 5, 3]);
```

![led rgb](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-rgb.png)


#### LED RGB Anode

```js
// With Options object & pins object
new five.Led.RGB({
  pins: {
    red: 6,
    green: 5,
    blue: 3
  },
  isAnode: true
});

// With Options object & pins array
new five.Led.RGB({
  pins: [6, 5, 3],
  isAnode: true
});
```

![led rgb anode](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-rgb-anode.png)


#### LED RGB PCA9685
```js
// With Options object & pins object
new five.Led.RGB({
  controller: "PCA9685",
  pins: {
    red: 2,
    green: 1,
    blue: 0
  }
});

// With Options object & pins array
new five.Led.RGB({
  controller: "PCA9685",
  pins: [2, 1, 0]
});

```

![Tessel PCA9685](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-rgb-pca9685-tessel.png)

![Arduino PCA9685](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-rgb-PCA9685.png)



#### LED RGB BlinkM

```js
new five.Led.RGB({
  controller: "BLINKM",
});

```

![BlinkM](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-blinkm-basic-tessel.png)

![BlinkM](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-blinkm-basic-arduino.png)

## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var a = new five.Led.RGB([ 9, 10, 11 ]);

  var b = new five.Led.RGB({
    pins: {
      red: 3,
      green: 5,
      blue: 6
    }
  });

  this.repl.inject({
    a: a,
    b: b
  });

  a.strobe(500);
  b.strobe(1000);
});

```

## API

- **on()** Turn the led on, honoring any previous color value
  ```js
  var led = new five.Led.RGB([9, 10, 11]);
  
  led.on();
  ```

- **off()** Turn the Led off.
  ```js
  var led = new five.Led.RGB([9, 10, 11]);
  
  led.off();
  ```

- **color(value)** Sets the Led color. If no value is supplied, it returns a copy of the state value.
  ```js
  var led = new five.Led.RGB([9, 10, 11]);
  
  led.color("#ff00ff");

  console.log( led.color() );
  // {red: 255, blue: 0, green: 255}
  ```
| Valid Color Arguments | Example(s) | In Situ |
|-----------------------|-----------------------|-----------------------|
| Any CSS color name string | `"red"`, `"green"`, `"blue"` | `rgb.color("red")` |
| Hexadecimal color strings | `"ff0000"`, `"00ff00"`, `"0000ff"` | `rgb.color("0000ff")` |
| Hexadecimal color strings, w/ leading \# | `"#ff0000"`, `"#00ff00"`, `"#0000ff"` | `rgb.color("#0000ff")` |
| Array of 8-bit bytes | `[0xff, 0x00, 0x00]` | `rgb.color([0xff, 0x00, 0x00])` |
| Object of 8-bit bytes | `{ red: 0x00, green: 0xFF, blue: 0x00 }` | `rgb.color({ red: 0x00, green: 0xFF, blue: 0x00 })` |


- **intensity(value)** Sets the overall intensity or "brightness" of the Led. Maintains current color state. If no value is supplied, it returns the current intensity value.
  ```js
  var led = new five.Led.RGB([9, 10, 11]);
  
  // Set the initial color (red)
  led.color("#ff0000");
  
  ...Sometime later...
  
  // Stays red, but at 30% intensity
  led.intensity(30);

  ```

- **toggle()** Toggle the current state, if on then turn off, if off then turn on.
  ```js
  var led = new five.Led.RGB([9, 10, 11]);

  led.toggle();
  ```
  
- **strobe(ms)** Strobe/Blink the Led on/off in phases over `ms`. This is an **interval** operation and can be stopped by calling `led.stop()`, however that will not necessarily turn it "off". Defaults to 500ms.
  ```js
  var led = new five.Led.RGB([9, 10, 11]);

  // Strobe on-off in 500ms phases
  led.strobe(500);
  ```

- **pulse(ms, callback)** Pulse the Led in phases from on to off over `ms` time, with an optional callback. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". Defaults to 1000 ms. The callback will be invoked every time the Led is fully on or off. This operation will only work with Leds attached to PWM pins.

  ```js
  var led = new five.Led.RGB([9, 10, 11]);

  // Pulse from on to off in 500ms phases
  led.pulse(500);
  ```

- **pulse(animation options)** Control the pulse of an LED with [`Animation` options](https://github.com/rwaldron/johnny-five/wiki/animation#segment-properties).  

  ```js
  var led = new five.Led.RGB([9, 10, 11]);

  led.pulse({
    easing: "linear",
    duration: 3000,
    cuePoints: [0, 0.2, 0.4, 0.6, 0.8, 1],
    keyFrames: [
        {
          color: "#0000ff",
          intensity: 0
        },
        {
          color: "0000ff",
          intensity: 10
        },
        {
          color: "#0000ff",
          intensity: 0
        },
        {
          color: "0000ff",
          intensity: 25
        },
        {
          color: "#0000ff",
          intensity: 0
        },
        {
          color: "0000ff",
          intensity: 100
        }
      ],
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



- **stop(ms)** For **interval** operations, call `stop` to stop the interval. `stop` does not necessarily turn "off" the Led, in order to fully shut down an Led, a program must call `stop().off()`.
  ```js
  var led = new five.Led.RGB([9, 10, 11]);
  
  // Blink from on to off in 500ms phases
  led.strobe(500);
  
  ...Sometime later...
  
  led.stop();

  ```

## Events
Led objects are output only and therefore do not emit any events.

## Examples
* [Led Rgb](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rgb.md)
* [Led Rgb Pulse](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rgb-pulse.md)
* [Led Rgb Intensity](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rgb-intensity.md)
* [Led Rgb BLINKM](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rgb-BLINKM.md)
* [Led Rgb Anode](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rgb-anode.md)
* [Led Rgb Anode PCA9685](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rgb-anode-PCA9685.md)
* [Led Rainbow](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rainbow.md)
* [Led Rgb PCA9685](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rgb-PCA9685.md)