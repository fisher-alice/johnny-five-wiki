![](http://i.gyazo.com/cd7a0b9df8390de58cecdb589fc8cb1c.png)

The `ESC` class constructs objects that represent a single ESC attached to the physical board. `ESC` objects are similar to `Servo` objects as they both use PWM pins to communicate with the physical ESC. Currently, this class assumes the ESC is [pre-calibrated](#wiki-calibrating-an-esc).


## Parameters

- **pin** A Number or String address for the ESC pin (PWM).

  ```js
  var esc = new five.ESC(9);
  ```
- **options** An object of property parameters.

  | Property | Type           | Value/ Description                                                                        | Default| Required |
  |----------|----------------|----------------------------------------|------------------------------------------------------------------------------------|----------|
  | pin      | Number, String | Any PWM Pin. The address of the PWM pin the ESC is attached to || yes      |
  | range    | Array          | `[ lower, upper ]`. The range of speed in percent. | `[0, 100]`                                | no       |
  | startAt  | Number         | Initial speed, 0-100% | `0` | no       |
  | controller    | String  | DEFAULT, PCA9685. Controller interface type. | "DEFAULT"                                           | no       |
  | device    | String  | FORWARD, FORWARD_REVERSE. Device capability type. | "FORWARD"                                           | no       |
  | neutral    | Number  | Neutral point, 0-100. | 0 | no       |

FORWARD_REVERSE
- **PCA9685 Options (`controller: "PCA9685"`)** 

  | Property | Type                            | Value/Description                              | Default | Required |
  |---------------|---------------------------------|----------------------------------------|----|----------|
  | address       | Number | I2C device address. | `0x40` | no       |


## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| id | A user definable id value. Defaults to a generated uid | No |
| pin | The pin address that the ESC is attached to | No |
| range | The range of speed as an array of fractional percent values. Defaults to [0, 100] | No |
| value | The value of the last/current speed. | Yes |

## Component Initialization


#### Basic 

```js
new five.ESC(9);
```

![ESC](https://raw.github.com/rwaldron/johnny-five/master/docs/breadboard/esc-keypress.png)

#### Limited Speed Range

```js
// Limited speed range to 0-80%
new five.ESC({
  pin: 9, 
  range: [ 0, 80 ]
});
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
  neutral: 50
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

  // Set to top speed. (this can be physically dangerous, you've been warned.)
  esc.max();

});
```

## API

- **speed(value)** Set the speed of the ESC, any fractional number value from 0...100 as a percentage value. If the specified speed is the same as the current speed, no commands are sent.
  ```js
  var esc = new five.ESC(11);

  // Set the motor's speed to 50%
  esc.speed(50);
  ```

- **min()** Set ESC to minimum speed. Defaults to 0%, respects explicit range.
  ```js
  var esc = new five.ESC(11);

  esc.min();
  ```
  Or 
  ```js
  var esc = new five.ESC({
    pin: 11, 
    range: [ 10, 100 ]
  });

  // Minimum speed at 10%
  esc.min();
  ```

- **max()** Set ESC to maximum speed. Defaults to 100%, respects specified range.
  ```js
  var esc = new five.ESC(11);

  esc.max();
  ```
  Or 
  ```js
  var esc = new five.ESC({
    pin: 11, 
    range: [ 0, 80 ]
  });

  // Max at 80%
  esc.max();
  ```

- **stop()** Stop a moving esc. 
  ```js
  var esc = new five.ESC(12);

  esc.stop();
  ```


<!--remove-start-->

## Examples
- [Esc Keypress](https://github.com/rwldrn/johnny-five/blob/master/docs/esc-keypress.md)
- [Esc Dualshock](https://github.com/rwldrn/johnny-five/blob/master/docs/esc-dualshock.md)

<!--remove-end-->

## Calibrating an ESC

Load the following sketch onto your Arduino, via the Arduino IDE follow the instructions.

This requires that the ESC's power source is **off or disconnected** at the start of the calibration process. You will be prompted to turn on/reconnect the power during the process.

```c
#include <Servo.h>

#define MAX_SIGNAL 2000
#define MIN_SIGNAL 700
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