# Led

The `Led` class constructs objects that represent a single Led attached to the physical board.


### Parameters

- **pin** A Number or String address for the Led pin (digital/PWM).
For Leds that only have on/off states, use a digital pin:
```js
var digital = new five.Led(13);
```
For Leds that have on/off states, as well as inverval or color related state (Pulse, Brightness, RGB, etc), use a PWM pin (demarcated by either a "~" or "#" next to the pin number on the actual board).
```js
// Look for "~", ie. ~11
var pwm = new five.Led(11);
```
Tinkerkit: 
```js
// Attached to "Output 0"
var digital = new five.Led("O0");
```


- **options** An object of property parameters.
<table>
  <thead>
    <tr>
      <th>Property Name</th>
      <th>Description</th>
      <th>Required</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>pin</td>
      <td>The Number address of the pin the led is attached to.</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>type</td>
      <td>OUTPUT or PWM</td>
      <td>no</td>
    </tr>

  </tbody>
</table>


### Shape

Led objects will have the following shape:

```js
{ 
  board: ...A reference to the board object the Led is attached to
  id: ...A user definable id value. Defaults to null
  pin: ...The pin address that the Led is attached to
  value: ...The current value of the Led. READONLY
  interval: ...An interval reference, if an interval exists
}
```




### Usage
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  // Leonardo's pin 13 is PWM capable
  var pin = this.type === "LEONARDO" ? 13 : 11;

  // Create a standard `led` hardware instance
  var led = new five.Led(pin);

  // Inject led object into REPL session
  this.repl.inject({
    led: led
  });
});
```


## API

- **on()** Turn the led on.
```js
var led = new five.Led(13);

led.on();
```

- **off()** Turn the led off.
```js
var led = new five.Led(13);

led.off();
```

- **off()** Toggle the current state, if _on_ then turn _off_, if _off_ then turn _on_.
```js
var led = new five.Led(13);

led.off();
```

- **strobe(ms)** Strobe/Blink the Led on/off in phases over `ms`. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". Defaults to 500ms.
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
var led = new five.Led(13);

// This will set the brightness to about half 
led.brightness(128);
```

- **fadeIn(ms)** Fade in from current brightness over `ms`. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". This operation will only work with Leds attached to PWM pins.
```js
var led = new five.Led(13);

// Fade in over 500ms.
led.fadeIn(500);
```

- **fadeOut(ms)** Fade out from current brightness over `ms`. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". This operation will only work with Leds attached to PWM pins.
```js
var led = new five.Led(13);

// Fade out over 500ms.
led.fadeOut(500);
```


- **pulse(ms)** Pulse the Led in phases from on to off over `ms` time. This is an **interval** operation and can be stopped by calling `pin.stop()`, however that will not necessarily turn it "off". This operation will only work with Leds attached to PWM pins.
```js
var led = new five.Led(13);

// Pulse from on to off in 500ms phases
led.pulse(500);
```

- **stop(ms)** For **interval** operations, call `stop` to stop the interval. `stop` does not necessarily turn "off" the Led, in order to fully shut down an Led, a program must call `stop().off()`. This operation will only work with Leds attached to PWM pins.
```js
var led = new five.Led(13);

// Pulse from on to off in 500ms phases
led.pulse(500);

...Sometime later...

led.stop();

```




## Events

Led objects are output only and therefore do not emit any events.

