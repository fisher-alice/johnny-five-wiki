The `Led.RGB` class constructs objects that represent an RGB Led.

## Parameters

- **pins** An Array containing the pins **red**, **green** and **blue**.

  | Index | Type   | Value/Description              | Required |
  |-------|--------|--------------------------------|----------|
  | 0     | Number | PWM capable to control red   | yes      |
  | 1     | Number | PWM capable to control green | yes      |
  | 2     | Number | PWM capable to control blue  | yes      |

- **options**

  | Property | Type    | Value(s)                                    | Description                                                                                                     | Required |
  |----------|---------|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------|----------|
  | pins     | Object, Array  | See **pins** table below | Sets the pins for each corresponding color                                                                      | yes      |
  | isAnode  | Boolean | true, false                                 | Set this to true to indicate the LED is a common anode LED. Defaults to false, indicating a common cathode LED. | no       |
  | controller  | String  | "DEFAULT", "PCA9685" | Controller interface type. Defaults to `"DEFAULT"`. | no |                                           

  * **pins** (Object)

    | Property | Type   | Value/Description              | Required |
    |----------|--------|--------------------------------|----------|
    | red      | Number | PWM capable to control red   | yes      |
    | green    | Number | PWM capable to control green | yes      |
    | blue     | Number | PWM capable to control blue  | yes      |

  * **pins** (Array)

    | Index | Type   | Value/Description              | Required |
    |-------|--------|--------------------------------|----------|
    | 0     | Number | PWM capable to control red   | yes      |
    | 1     | Number | PWM capable to control green | yes      |
    | 2     | Number | PWM capable to control blue  | yes      |




## Shape

```
{
  red: Led object
  green: Led object
  blue: Led object, 
  isAnode: true | false. READONLY
}
```

## Component Initialization


#### LED RGB
```js
// With Options object & pins object
new five.Led.RGB({
  pins: {
    red: 9,
    green: 10,
    blue: 11
  }
});

// With Options object & pins array
new five.Led.RGB({
  pins: [9, 10, 11]
});

// With pins array
new five.Led.RGB([9, 10, 11]);
```

![led rgb](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-rgb.png)


#### LED RGB Anode

```js
// With Options object & pins object
new five.Led.RGB({
  pins: {
    red: 9,
    green: 10,
    blue: 11
  },
  isAnode: true
});

// With Options object & pins array
new five.Led.RGB({
  pins: [9, 10, 11],
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
    red: 0,
    green: 1,
    blue: 2
  }
});

// With Options object & pins array
new five.Led.RGB({
  controller: "PCA9685",
  pins: [0, 1, 2]
});

```

![PCA9685](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-rgb-PCA9685.png)

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

- **stop(ms)** For **interval** operations, call `stop` to stop the interval. `stop` does not necessarily turn "off" the Led, in order to fully shut down an Led, a program must call `stop().off()`. This operation will only work with Leds attached to PWM pins.
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
* [Led Rainbow](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rainbow.md)
* [Led Rgb PCA9685](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rgb-PCA9685.md)