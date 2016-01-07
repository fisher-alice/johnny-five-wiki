
The `Expander` class constructs objects that represent a single I2C IO Expander attached to the physical board. Expansion chips are useful for adding additional I/O to a project. In certain cases, I/O expansion chips can be used to provide capabilities on platforms that don't support natively support them. A couple examples:

- To add "Analog Read" (ADC) capabilities to your Raspberry Pi project, use either:
  - `PCF8591`
  - `GROVEPI`
- To add "Servo Write" (60Hz PWM) capabilities to your Raspberry Pi or pcDuino project, use either: 
  - `PCA9685`
  - `GROVEPI`


Supported Expanders:

- MCP23017
  - [Adafruit](https://www.adafruit.com/products/732?utm_source=j5)
- MCP23008
  - [Adafruit](https://www.adafruit.com/product/593?utm_source=j5)
- PCF8574
  - [Amazon](http://www.amazon.com/Expansion-PCF8574-Expander-Evaluation-Development/dp/B00DUO17J6/)
- PCF8574A
  - [Amazon](http://www.amazon.com/Expansion-PCF8574-Expander-Evaluation-Development/dp/B00DUO17J6/)
- PCF8575
  - [SparkFun](https://www.sparkfun.com/products/8130?utm_source=j5)
- PCA9685
  - [Adafruit](https://www.adafruit.com/products/815?utm_source=j5)
- PCF8591
  - [Amazon](http://www.amazon.com/PCF8591-Converter-Module-Digital-Conversion/dp/B00BXX4UWC)
- MUXSHIELD2
  - [Mayhew](http://mayhewlabs.com/products/mux-shield-2)
- GROVEPI
  - [Dexter](http://www.dexterindustries.com/grovepi/)

This list will continue to be updated as more component support is implemented.

## Parameters

- **Controller String**
  When using _just_ the controller name as an argument, the default I2C bus address will be used.

  - `MCP23017`
  - `MCP23008`
  - `PCF8574`
  - `PCF8574A`
  - `PCF8575`
  - `PCF8591`
  - `PCA9685`
  - `MUXSHIELD2`
  - `GROVEPI`

  ```js
  // Examples:
  new five.Expander("MCP23017");

  new five.Expander("PCF8574");

  new five.Expander("PCF8575");
  ```

- **General Options**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | controller    | string | MCP23017, MCP23008, PCF8574, PCF8574A, PCF8575, PCF8591, PCA9685, MUXSHIELD2, GROVEPI. The Name of the controller to use |  | Yes       |
  | address       | Number  | Address for I2C device. | [By Device](#default-addresses) | no       |
  </span>

  ```js
  new five.Expander({
    controller: "MCP23017"
  });

  // or

  new five.Expander({
    controller: "MCP23017",
    address: 0x??
  });
  ```

  ##### Expander Capabilities

  | Chip | Pins | Modes |
  |------|------|-------|
  | MCP23017 | 16 | `INPUT`, `OUTPUT` |
  | MCP23008 | 8 | `INPUT`, `OUTPUT` |
  | PCF8574 | 8 | `INPUT`, `OUTPUT` |
  | PCF8574A | 8 | `INPUT`, `OUTPUT` |
  | PCF8575 | 16 | `INPUT`, `OUTPUT` |
  | PCF8591 | 4 | `ANALOG` |
  | PCA9685 | 16 | `OUTPUT`, `PWM`, `SERVO` |
  | MUXSHIELD2 | 48 | `INPUT`, `OUTPUT`, `ANALOG` \* |
  | GROVEPI | 10 \** | `INPUT`, `OUTPUT`, `ANALOG`, `PWM`, `SERVO`  |

  \* Modes limited to one per row
  \** 7 Digital, 3 Analog

  ##### Default Addresses

  | Chip | Range | Default |
  |------|------|-------|
  | MCP23017 | `0x20-0x27` | `0x20` |
  | MCP23008 | `0x20-0x27` | `0x20` |
  | PCF8574 | `0x20-0x27` | `0x20` |
  | PCF8574A | `0x38-0x3F` | `0x38` |
  | PCF8575 | `0x20-0x27` | `0x20` |
  | PCF8591 | `0x48-0x4F` | `0x48` |
  | PCA9685 | `0x40-0x4F` | `0x40` |
  | GROVEPI | `0x04` | `0x04` |


## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `pins` | An array of pin objects, containing capability data. | No |
| `MODES` | An object containing supported modes. | No |

## Component Initialization


#### MCP23017
```js
// Create a MCP23017 Expander object:
//
//  - attach SDA and SCL to the I2C pins on
//     your board (A4 and A5 for the Uno)
//  - specify the MCP23017 controller

new five.Expander("MCP23017");

// or

new five.Expander({
  controller: "MCP23017"
});

// or

new five.Expander({
  controller: "MCP23017",
  address: 0x??
});
```

![MCP23017](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/expander-MCP23017-basic.png)


#### MCP23008
```js
// Create a MCP23008 Expander object:
//
//  - attach SDA and SCL to the I2C pins on
//     your board (A4 and A5 for the Uno)
//  - specify the MCP23008 controller

new five.Expander("MCP23008");

// or

new five.Expander({
  controller: "MCP23008"
});

// or

new five.Expander({
  controller: "MCP23008",
  address: 0x??
});
```

![MCP23008](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/expander-MCP23008-basic.png)

#### PCF8574
```js
// Create a PCF8574 Expander object:
//
//  - attach SDA and SCL to the I2C pins on
//     your board (A4 and A5 for the Uno)
//  - specify the PCF8574 controller

new five.Expander("PCF8574");

// or

new five.Expander({
  controller: "PCF8574"
});

// or

new five.Expander({
  controller: "PCF8574",
  address: 0x??
});
```

![PCF8574](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/expander-PCF8574-basic.png)


#### PCF8575
```js
// Create a PCF8575 Expander object:
//
//  - attach SDA and SCL to the I2C pins on
//     your board (A4 and A5 for the Uno)
//  - specify the PCF8575 controller

new five.Expander("PCF8575");

// or

new five.Expander({
  controller: "PCF8575"
});

// or

new five.Expander({
  controller: "PCF8575",
  address: 0x??
});
```

![PCF8575](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/expander-PCF8575-basic.png)


#### PCA9685
```js
// Create a PCA9685 Expander object:
//
//  - attach SDA and SCL to the I2C pins on
//     your board (A4 and A5 for the Uno)
//  - specify the PCA9685 controller

new five.Expander("PCA9685");

// or

new five.Expander({
  controller: "PCA9685"
});

// or

new five.Expander({
  controller: "PCA9685",
  address: 0x??
});
```

![PCA9685](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-PCA9685.png)



#### PCF8591
```js
// Create a PCF8591 Expander object:
//
//  - attach SDA and SCL to the I2C pins on
//     your board (A4 and A5 for the Uno)
//  - specify the PCF8591 controller

new five.Expander("PCF8591");

// or

new five.Expander({
  controller: "PCF8591"
});

// or

new five.Expander({
  controller: "PCF8591",
  address: 0x??
});
```

![PCF8591](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/expander-PCF8591.png)

## Usage

`Expander` objects have the same surface API as an [IO Plugin](https://github.com/rwaldron/io-plugins), which allows them to be optionally used as an IO Plugin themselves. The usage examples here show how an `Expander` can be used to create a virtual board with `Board.Virtual` that can be passed to any Johnny-Five component class initialization.



#### Example: Digital or PWM Output

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var expander = new five.Expander({
    controller: "PCA9685"
  });

  var virtual = new five.Board.Virtual({
    io: expander
  });

  var led = new five.Led({
    pin: 0,
    board: virtual
  });

  led.on();
  // or
  // led.pulse();
});
```

![PCA9685](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-PCA9685.png)


#### Example: Digital Input & Output

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  // An Expander instance can also be passed
  // directly as an argument to Virtual
  var virtual = new five.Board.Virtual(
    new five.Expander("PCF8575")
  );

  var led = new five.Led({
    board: virtual,
    pin: 0,
  });

  var button = new five.Button({
    board: virtual,
    pin: 17,
  });

  button.on("press", function() {
    led.on();
  });

  button.on("release", function() {
    led.off();
  });
});
```

![PCF8575 Led & Button](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/expander-PCF8575-led-button.png)

## API

While the API of an `Expander` instance will be the same as a `Board` instance, not all capabilities will always be supported. In latter cases, attempts to call methods that would result in attempting an unsupported operation will throw an exception.

- **normalize(pin)** Provides pin value normalization. In most cases, this will simply return the value it was passed. It exists to override the default Firmata-centric normalization.
  ```js
  expander.normalize(1); // 1;
  ```

- **pinMode(pin, mode)** Set the mode of a pin. Must correspond to a mode listed in the `MODES` property. [Expander Capabilities](#expander-capabilities)
  ```js
  expander.pinMode(0, expander.MODES.INPUT);
  ```

- **analogRead(pin, handler)** Read an `ANALOG` pin. [Expander Capabilities](#expander-capabilities)
  ```js
  expander.analogRead(0, function(value) { ... });
  ```

- **digitalRead(pin, handler)** Read an `INPUT` pin. [Expander Capabilities](#expander-capabilities)
  ```js
  expander.digitalRead(0, function(value) { ... });
  ```

- **analogWrite(pin, 0-255)** An alias to `pwmWrite`, for backward compat only. Use `pwmWrite` instead.

- **digitalWrite(pin, HIGH | LOW)** Write a `HIGH` or `LOW` value to an `OUTPUT` pin. [Expander Capabilities](#expander-capabilities)
  ```js
  expander.digitalWrite(0, 1);
  ```

- **pwmWrite(pin, 0-255)** Write an 8-bit PWM value to a `PWM` pin. [Expander Capabilities](#expander-capabilities)
  ```js
  expander.pwmWrite(0, 255);
  ```

- **servoWrite(pin, 0-180)** Write a 0-180° value to a `SERVO` pin. [Expander Capabilities](#expander-capabilities)
  ```js
  expander.servoWrite(0, 180);
  ```


## Events

- **digital-read-#** Whenever `digitalRead(pin, ...)` is called, an event handler with naming form of `digital-read-#` (where # is the pin number) is registered.

- **analog-read-#** Whenever `analogRead(pin, ...)` is called, an event handler with naming form of `analog-read-#` (where # is the pin number) is registered.

- **ready** For compatibility with IO Plugins only. This event is meaningless, so don't use it.
