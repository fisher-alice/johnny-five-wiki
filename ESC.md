![](http://i.gyazo.com/cd7a0b9df8390de58cecdb589fc8cb1c.png)

The `ESC` class constructs objects that represent a single ESC (Electronic Speed Control) component attached to the physical board. `ESC` objects are similar to `Servo` objects as they both use PWM pins to communicate with the physical ESC. Currently, this class assumes the ESC is [pre-calibrated](#wiki-calibrating-an-esc).

If you are interested in working with a multiple ESCs, see the [ESCs](escs) class.

## Parameters

- **pin** A Number or String address for the ESC pin (PWM).

  ```js
  var esc = new five.ESC(9);
  ```
- **Options** An object of property parameters.
  <span class="abbreviate-table">

  | Property | Type           | Value/ Description                                                                        | Default| Required |
  |----------|----------------|----------------------------------------|------------------------------------------------------------------------------------|----------|
  | pin      | Number, String | Any PWM Pin. The address of the PWM pin the ESC is attached to || yes      |
  | controller    | String  | DEFAULT, PCA9685. Controller interface type. | "DEFAULT"                                           | no       |
  | pwmRange      | Array          | `[ min, max ]` The pulse width range in microseconds. | `[1000, 2000]`                      | no       |
  | device    | String  | FORWARD, FORWARD_REVERSE. Device capability type. | "FORWARD"                                           | no       |
  | neutral    | Number  | Neutral point, usually the middle of the PWM range. | Varies by `device` | no       |
  </span>

- **PCA9685 Options (`controller: "PCA9685"`)** 
  <span class="abbreviate-table">

  | Property | Type                            | Value/Description                              | Default | Required |
  |---------------|---------------------------------|----------------------------------------|----|----------|
  | address       | Number | I2C device address. | `0x40` | no       |
  </span>

## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `pin` | The pin address that the ESC is attached to | No |
| `value` | The value of the last/current speed. | Yes |

## Component Initialization


#### Basic 

```js
new five.ESC(9);
```

![ESC](https://raw.github.com/rwaldron/johnny-five/master/docs/breadboard/esc-keypress.png)


#### ESC PCA9685

```js
new five.ESC({
  controller: "PCA9685",
  pin: 1
});
```

![ESC](https://raw.github.com/rwaldron/johnny-five/master/docs/breadboard/esc-PCA9685-b.png)


#### Bidirectional 

```js
new five.ESC({
  pin: 9, 
  device: "FORWARD_REVERSE", 
});
```

![ESC](https://raw.github.com/rwaldron/johnny-five/master/docs/breadboard/esc-bidirectional.png)


## Usage

Standard ESC
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var esc = new five.ESC(11);
  
  board.loop(100, () => 
  esc.throttle();

});
```

## API

- **speed(value)** Deprecated. Use `throttle(μs)`


- **throttle(value)** Throttle the speed of the ESC by setting a pulse in μs.
  ```js
  var esc = new five.ESC(9);

  esc.throttle(1500); // 1500μs is half speed of a single direction or neutral of a bidirectional
  ```

- **brake()** Brake the ESC
  ```js
  var esc = new five.ESC(9);

  esc.brake();
  ```


<!--remove-start-->

## Examples
- [Esc Keypress](https://github.com/rwldrn/johnny-five/blob/master/docs/esc-keypress.md)
- [Esc Dualshock](https://github.com/rwldrn/johnny-five/blob/master/docs/esc-dualshock.md)

<!--remove-end-->

## Calibrating an ESC

**This isn't a requirement, but if you experience issues with your ESCs, it might prove helpful**

Load the following sketch onto your Arduino, via the Arduino IDE follow the instructions.

This requires that the ESC's power source is **off or disconnected** at the start of the calibration process. You will be prompted to turn on/reconnect the power during the process.

```c
#include <Servo.h>

#define MAX_SIGNAL 2000
#define MIN_SIGNAL 1000
#define MOTOR_PIN 12

Servo motor;

void setup() {
  Serial.begin(9600);
  Serial.println("Program begin...");
  Serial.println("This program will calibrate the ESC.");

  motor.attach(MOTOR_PIN);

  Serial.println("Calibrating Maximum Signal");
  Serial.println("Turn on power source, then wait 2 seconds and press any key + <enter>");
  motor.writeMicroseconds(MAX_SIGNAL);

  // Wait for input
  while (!Serial.available());
  Serial.read();

  // Send min output
  Serial.println("Calibrating Minimum Signal");
  motor.writeMicroseconds(MIN_SIGNAL);

}

void loop() {  

}
```