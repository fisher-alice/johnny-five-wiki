
The `Expander` class constructs objects that represent a single I2C IO Expander attached to the physical board.

Supported Expanders:

- [MCP23017](https://www.adafruit.com/products/732)
- [MCP23008](https://www.adafruit.com/product/593)
- [PCF8574](http://www.amazon.com/Expansion-PCF8574-Expander-Evaluation-Development/dp/B00DUO17J6/)
- [PCF8574A](http://www.amazon.com/Expansion-PCF8574-Expander-Evaluation-Development/dp/B00DUO17J6/)
- [PCF8575](https://www.sparkfun.com/products/8130)
- [PCA9685](https://www.adafruit.com/products/815)

This list will continue to be updated as more component support is implemented.

## Parameters

- **Controller String**

  - `MCP23017`
  - `MCP23008`
  - `PCF8574`
  - `PCF8574A`
  - `PCF8575`
  - `PCA9685`

  ```js


- **General Options**

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | controller    | string | MCP23017, MCP23008, PCF8574, PCF8574A, PCF8575. The Name of the controller to use |  | Yes       |
  | address       | Number  | Address for I2C device. | By Device \* | no       |


  ##### Expander Capabilities 

  | Chip | Pins | Modes |
  |------|------|-------|
  | MCP23017 | 16 | `INPUT`, `OUTPUT` |
  | MCP23008 | 8 | `INPUT`, `OUTPUT` |
  | PCF8574 | 8 | `INPUT`, `OUTPUT` |
  | PCF8574A | 8 | `INPUT`, `OUTPUT` |
  | PCF8575 | 16 | `INPUT`, `OUTPUT` |
  | PCA9685 | 16 | `OUTPUT`, `PWM`, `SERVO` |  

  ##### Default Addresses

  | Chip | Range | Default |
  |------|------|-------|
  | MCP23017 | `0x20-0x27` | `0x20` |
  | MCP23008 | `0x20-0x27` | `0x20` |
  | PCF8574 | `0x20-0x27` | `0x20` |
  | PCF8574A | `0x38-0x3F` | `0x38` |
  | PCF8575 | `0x20-0x27` | `0x20` |
  | PCA9685 | `0x40-0x4F` | `0x40` |  
 

## Shape

```js
{ 
  id: A user definable id value. Defaults to a generated uid
  pins: An array of pin objects, containing capability data.
  MODES: An object containing supported modes.
}
```

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
  controller: "PCA9685",
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
  controller: "PCA9685",
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
  controller: "PCA9685",
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
  controller: "PCA9685",
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


## Usage

`Expander` objects have the same surface API as an [IO Plugin](https://github.com/rwaldron/io-plugins), which allows them to be optionally used as an IO Plugin themselves. The usage examples here show how an `Expander` can be used to create a virtual board with `Board.Virtual` that can be passed to any Johnny-Five component class initialization.



#### Expander: Digital or PWM Output

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
});
```

![PCA9685](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-PCA9685.png)


#### Expander: Digital Input & Output

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

- **pinMode(pin, mode)** Set the mode of a pin. Must correspond to a mode listed in the `MODES` property.
  ```js
  expander.pinMode(0, expander.MODES.INPUT);
  ```

- **analogRead(pin, handler)** Read an `ANALOG` pin. [Expander Capabilities](#Expander-Capabilities)
  ```js
  expander.analogRead(0, function(value) { ... });
  ```

- **digitalRead(pin, handler)** Read an `INPUT` pin. [Expander Capabilities](#Expander-Capabilities)
  ```js
  expander.digitalRead(0, function(value) { ... });
  ```

- **analogWrite(pin, 0-255)** An alias to `pwmWrite`, for backward compat only. Use `pwmWrite` instead.

- **digitalWrite(pin, HIGH | LOW)** Write a `HIGH` or `LOW` value to an `OUTPUT` pin. [Expander Capabilities](#Expander-Capabilities)
  ```js
  expander.digitalWrite(0, 1);
  ```

- **pwmWrite(pin, 0-255)** Write an 8-bit PWM value to a `PWM` pin. [Expander Capabilities](#Expander-Capabilities)
  ```js
  expander.pwmWrite(0, 255);
  ```

- **servoWrite(pin, 0-180)** Write a 0-180° value to a `SERVO` pin. [Expander Capabilities](#Expander-Capabilities)
  ```js
  expander.servoWrite(0, 180);
  ```


## Events

- **digital-read-#** Whenever `digitalRead(pin, ...)` is called, an event handler with naming form of `digital-read-#` (where # is the pin number) is registered.

- **analog-read-#** Whenever `analogRead(pin, ...)` is called, an event handler with naming form of `analog-read-#` (where # is the pin number) is registered.
