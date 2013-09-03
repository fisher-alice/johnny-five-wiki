The `Led.RGB` class constructs objects that represent an RGB Led.

### Parameters

- **options**
<table>
  <thead>
    <tr>
      <th>Property Name</th><th>Type</th><th>Value(s)</th><th>Description</th><th>Required</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>pins</td>
      <td>Object</td>
      <td>
        ```
        {
          red: 3,
          green: 5,
          blue: 6
        }
        ```
      </td>
      <td>
        Sets the pins for each corresponding color
      </td>
      <td>yes</td>
    </tr>
  </tbody>
</table>
      
  * **pins**
  <table>
    <thead>
      <tr>
        <th>Property Name</th>
        <th>Type</th>
        <th>Value(s)</th>
        <th>Description</th>
        <th>Required</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>red</td>
        <td>Number</td>
        <td>
          Any PWM pin on board
        </td>
        <td>Sets the Led's red pin</td>
        <td>yes</td>
      </tr>
      <tr>
        <td>green</td>
        <td>Number</td>
        <td>
          Any PWM pin on board
        </td>
        <td>Sets the Led's green pin</td>
        <td>yes</td>
      </tr>
      <tr>
        <td>blue</td>
        <td>Number</td>
        <td>
          Any PWM pin on board
        </td>
        <td>Sets the Led's blue pin</td>
        <td>yes</td>
      </tr>
    </tbody>
  </table>

- **pins** An Array containing the respective values for the red, green and blue pins.
```js
var digital = new five.Led([9, 10, 11]);
```


### Shape

```
{
  red: Led object
  green: Led object
  blue: Led object
}
```


### Usage

```js
var five = require("johnny-five");

five.Board().on("ready", function() {
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

  a.pulse();
  b.pulse();
});

```

## API

- **on()** Turn the led on.
  ```js
  var led = new five.Led.RGB([9, 10, 11]);
  
  led.on();
  ```
- **off()** Turn the Led off.
 ```js
  var led = new five.Led.RGB([9, 10, 11]);
  
  led.off();
  ```

- **color(value)** Sets the Led color.
  ```js
  var led = new five.Led.RGB([9, 10, 11]);
  
  led.color("#ff00ff");
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

- **brightness(0-255)** Set the brightness of led. This operation will only work with Leds attached to PWM pins. 
  ```js
  var led = new five.Led.RGB([9, 10, 11]);
  
  // This will set the brightness to about half 
  led.brightness(128);
  ```

- **fadeIn(ms)** Fade in from current brightness over `ms`. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". This operation will only work with Leds attached to PWM pins.
  ```js
  var led = new five.Led.RGB([9, 10, 11]);
  
  // Fade in over 500ms.
  led.fadeIn(500);
  ```

- **fadeOut(ms)** Fade out from current brightness over `ms`. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". This operation will only work with Leds attached to PWM pins.
  ```js
  var led = new five.Led.RGB([9, 10, 11]);
  
  // Fade out over 500ms.
  led.fadeOut(500);
  ```


- **pulse(ms)** Pulse the Led in phases from on to off over `ms` time. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". This operation will only work with Leds attached to PWM pins.
  ```js
  var led = new five.Led.RGB([9, 10, 11]);
  
  // Pulse from on to off in 500ms phases
  led.pulse(500);
  ```

- **stop(ms)** For **interval** operations, call `stop` to stop the interval. `stop` does not necessarily turn "off" the Led, in order to fully shut down an Led, a program must call `stop().off()`. This operation will only work with Leds attached to PWM pins.
  ```js
  var led = new five.Led.RGB([9, 10, 11]);
  
  // Pulse from on to off in 500ms phases
  led.pulse(500);
  
  ...Sometime later...
  
  led.stop();
  
  ```


## Events
Led objects are output only and therefore do not emit any events.

## Examples
* [Led Rgb](https://github.com/rwaldron/johnny-five/blob/master/docs/led-rgb.md)
