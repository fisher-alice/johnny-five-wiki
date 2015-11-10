The `Board` class constructs objects that represent the physical board itself. All device objects depend on an initialized and ready board object.

Johnny-Five (sans IO Plugin) has been tested on, but is not limited to, the following boards:

- [Arduino UNO](http://arduino.cc/en/Main/arduinoBoardUno)
- [Arduino Leonardo](http://arduino.cc/en/Main/arduinoBoardLeonardo)
- [Arduino MEGA](http://arduino.cc/en/Main/arduinoBoardMega)
- [Arduino FIO](http://arduino.cc/en/Main/ArduinoBoardFio)
- [Arduino Pro](http://arduino.cc/en/Main/ArduinoBoardPro)
- [Arduino Pro Mini](http://arduino.cc/en/Main/ArduinoBoardProMini)
- [Arduino Nano](http://arduino.cc/en/Main/arduinoBoardNano)
- [TinyDuino](http://tiny-circuits.com/products/tinyduino/)
- [BotBoarduino](http://www.lynxmotion.com/c-153-botboarduino.aspx)
- [Dagu Micro Magician v2](http://www.dawnrobotics.co.uk/dagu-micro-magician-v2/)
- [Red Back Spider](http://robosavvy.com/store/product_info.php/products_id/1574)
- [TI Launchpad](http://www.ti.com/ww/en/launchpad/launchpad.html?DCMP=mcu-launchpad&HQS=launchpad) (with [Energia Firmata](https://github.com/energia/Energia/tree/master/libraries/Firmata))

For non-Arduino based projects, a number of [IO Plugins](https://github.com/rwaldron/IO-Plugins) are available:

- [BeagleBone-IO](https://github.com/julianduque/beaglebone-io)
- [Blend-Micro-IO](https://github.com/noopkat/blend-micro-io)
- [Galileo-IO](https://github.com/rwaldron/galileo-io/)
- [Bean-IO](https://github.com/monteslu/bean-io/)
- [Nino-IO](https://github.com/rwaldron/nino-io/)
- [pcDuino-IO](https://github.com/rwaldron/pcDuino-io/)
- [Pinoccio-IO](https://github.com/soldair/pinoccio-io/)
- [Raspi-IO](https://github.com/bryan-m-hughes/raspi-io)
- [Spark-IO](https://github.com/rwaldron/spark-io/)
- [Imp-IO](https://github.com/rwaldron/imp-io/)
- [Remote-IO](https://github.com/monteslu/remote-io)
- [Board-IO](https://github.com/achingbrain/board-io)


See also: [Multi-Board Support](https://github.com/rwldrn/johnny-five/wiki/Boards)

## Parameters

- **options** Optional object of themselves optional parameters.

  | Property | Type             | Value/Description                                          | Default | Required |
  |---------------|------------------|-----------------------------------------|------------------------------------------------------|----------|
  | id            | Number, String   | Any. User definable identification                        | Generated | no       |
  | port          | String or object | eg. `/dev/ttyAM0`, `COM1`, `new SerialPort()`. Path or name of device port/COM or SerialPort object | Detected | no       |
  | repl          | Boolean          | `true`, `false`. Set to `false` to disable REPL                 | `true` | no       |
  | debug         | Boolean          | `true`, `false`. Set to `false` to disable debugging output | `true`                     | no       |

## Shape
| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `io` | A reference to the IO protocol layer. | No |
| `id` | A user definable id value. Defaults to a generated uid. | No |
| `repl` | A reference to the active REPL. | No |

## Component Initialization

To initialize control of a board, construct an instance of the `Board` class.


> When connecting to a USB serial device, such as an Arduino, you **do not** need to specify the device path or COM port, Johnny-Five will determine which to connect to and connect automatically.

```js
new five.Board();
```

You may optionally specify the port by providing it as a property of the options object parameter.

```js
// OSX
new five.Board({ port: "/dev/tty.usbmodem****" });

// Linux
new five.Board({ port: "/dev/ttyUSB*" });

// Windows
new five.Board({ port: "COM*" });
```

\* Denotes system specific enumeration value (ie. a number)

You can specify a `SerialPort` object by providing it as a property of the options object parameter:

```js
var SerialPort = require("serialport").SerialPort;
var five = require("johnny-five");
var board = new five.Board({
  port: new SerialPort("COM4", {
    baudrate: 9600,
    buffersize: 1
  })
});
```



## Usage

A basic, but complete example usage of the `Board` constructor:


```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  /*
    Initialize pin 13, which 
    controls the built-in LED
  */
  var led = new five.Led(13);
  
  /*
    Injecting object into the REPL
    allow access while the program
    is running. 
    
    Try these in the REPL: 
    
    led.on();
    led.off();
    led.blink();

    (One at a time to see each action)
  */
  this.repl.inject({
    led: led
  });
});
```

## API


### Logging 

- **info(class, message [, ...detail])** Displays an _info_ message in the console (when `debug: true`, which is the default) and emits an event of the same name, as well as a generic _message_ event. `class` is the class that triggered the message, or that is being reported for; `message` is any relevant message. This argument accept an arbitrary number of _detail_ arguments. If the last _detail_ argument is an object or array, it will be passed along as the value of a `data` property of the event object.
  ```js
  board.info("Board", "I got somethin' to say!", { foo: 1 });

  board.info("Servo", "This pin doesn't have hardware PWM support, but will function correctly with software Servo support.");
  ```

- **warn(class, message [, ...detail])** Displays a _warn_ message in the console (when `debug: true`, which is the default) and emits an event of the same name, as well as a generic _message_ event. `class` is the class that triggered the message, or that is being reported for; `message` is any relevant message. This argument accept an arbitrary number of _detail_ arguments. If the last _detail_ argument is an object or array, it will be passed along as the value of a `data` property of the event object.
  ```js
  board.warn("Board", "Watch out!", { bar: 2 });
  ```

- **fail(class, message [, ...detail])** Displays a _fail_ message in the console (when `debug: true`, which is the default) and emits an event of the same name, as well as a generic _message_ event. `class` is the class that triggered the message, or that is being reported for; `message` is any relevant message. This argument accept an arbitrary number of _detail_ arguments. If the last _detail_ argument is an object or array, it will be passed along as the value of a `data` property of the event object.
  ```js
  board.fail("Led", "This pin is already in use!", { baz: NaN });

  board.fail("Board", "This program attempted to do something that isn't possible");  
  ```

See [Events](#events) for specific log event details. 


- **repl** This is a reference to the active REPL automatically created by the `Board` class. This object has an `inject` method that may be called as many times as desired: 
  - **repl.inject(object)** Inject objects or values, from the program, into the REPL session.

    ```js
    var five = require("johnny-five");
    var board = new five.Board();

    board.on("ready", function() {
      // Initialize an LED directly in the REPL
      this.repl.inject({
        led: new five.Led(13)
      });  
    });
    /*
      From the terminal...

      $ node program.js
      1423012815316 Device(s) /dev/cu.usbmodem1421
      1423012818908 Connected /dev/cu.usbmodem1421
      1423012818908 Repl Initialized  
      >> led.on();
      >> led.off();
      
    */
    ```

- **pinMode(pin, mode)** Set the `mode` of a specific `pin`, one of INPUT, OUTPUT, ANALOG, PWM, SERVO.
Mode constants are exposed via the `Pin` class

  | Mode   | Value | Constant   |
  |--------|-------|------------|
  | INPUT  | 0     | Pin.INPUT  |
  | OUTPUT | 1     | Pin.OUTPUT |
  | ANALOG | 2     | Pin.ANALOG |
  | PWM    | 3     | Pin.PWM    |
  | SERVO  | 4     | Pin.SERVO  |

  ```js
  // Set a pin to INPUT mode
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    
    // pin mode constants are available on the Pin class
    this.pinMode(13, five.Pin.INPUT);
  });
  ```

- **analogWrite(pin, value)** Write an unsigned, 8-bit value (0-255) to a digital `pin`.
  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    // Assuming an Led is attached to pin 9, 
    // this will turn it on at full brightness
    // PWM is the mode used to write ANALOG 
    // signals to a digital pin
    this.pinMode(9, five.Pin.PWM);
    this.analogWrite(9, 255);
  });
  ```

- **analogRead(pin, handler(voltage))** Register a handler to be called whenever the board reports the voltage value (0-1023) of the specified analog `pin`.
  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    // Assuming a sensor is attached to pin "A1"
    this.pinMode(1, five.Pin.ANALOG);
    this.analogRead(1, function(voltage) {
      console.log(voltage);
    });
  });
  ```

- **digitalWrite(pin, value)** Write a digital value (0 or 1) to a digital `pin`.
  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    // Assuming an Led is attached to pin 13, this will turn it on
    this.pinMode(13, five.Pin.OUTPUT);
    this.digitalWrite(13, 1);
  });
  ```

- **digitalRead(pin, handler(value))** Register a `handler` to be called whenever the board reports the value (0 or 1) of the specified digital `pin`.
  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    // Assuming a button is attached to pin 9
    this.pinMode(9, five.Pin.INPUT);
    this.digitalRead(9, function(value) {
      console.log(value);
    });
  });
  ```
  > Note: `digitalRead` will only call its handler when the value of the pin **changes**.


- **i2cConfig([milliseconds])** This must be called prior to any I2C reads or writes. Optionally accepts a period in milliseconds to delay between read operations. 

  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    this.i2cConfig();
    // Safely interact with I2C components
  });
  ```

- **i2cWrite(address, arrayOfBytes)** Write an `arrayOfBytes` to the component at `address`.
  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    this.i2cConfig();
    this.i2cWrite(0x01, [0x02, 0x03]);
  });
  ```

- **i2cWrite(address, register, arrayOfBytes)** Write an `arrayOfBytes` to the component at `address`, for a specific `register`.

  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    this.i2cConfig();
    this.i2cWrite(0x01, 0x00, [0x02, 0x03]);
  });
  ```

- **i2cWriteReg(address, register, byte)** Write a single `byte` to the component at `address`, for a specific `register`.

  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    this.i2cConfig();
    this.i2cWrite(0x01, 0x00, 0x7e);
  });
  ```


- **i2cRead(address, bytesToRead, handler(arrayOfBytes))** Repeatedly read the specified number of bytes (`bytesToRead`) and call `handler` with the results as `arrayOfBytes`. 

  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    this.i2cConfig();
    this.i2cRead(0x01, 0x02, 6, function(bytes) {
      console.log("Bytes read: ", bytes);
    });
  });
  ```


- **i2cRead(address, register, bytesToRead, handler(arrayOfBytes))** Repeatedly read the specified number of bytes (`bytesToRead`), starting at a specific register, and call `handler` with the results as `arrayOfBytes`.

  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    this.i2cConfig();
    this.i2cRead(0x01, 0x00, 6, function(bytes) {
      console.log("Bytes read: ", bytes);
    });
  });
  ```

- **i2cReadOnce(address, register, bytesToRead, handler(arrayOfBytes))** Read the specified number of bytes (`bytesToRead`), starting at a specific register, and call `handler` with the results as `arrayOfBytes`.

  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    this.i2cConfig();
    this.i2cReadOnce(0x01, 0x02, 6, function(bytes) {
      console.log("Bytes read: ", bytes);
      console.log("Done!");
    });
  });
  ```

- **i2cReadOnce(address, bytesToRead, handler)** Read the specified number of bytes (`bytesToRead`) and call `handler` with the results as `arrayOfBytes`. 

  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    this.i2cConfig();
    this.i2cRead(0x01, 6, function(bytes) {
      console.log("Bytes read: ", bytes);
      console.log("Done!");
    });
  });
  ```

- **servoWrite(pin, angle)** Write an angle in degrees from 0-180 to a servo.

  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    this.pinMode(9, five.Pin.SERVO);
    this.servoWrite(9, 90);
  });
  ```




- **shiftOut(dataPin, clockPin, isBigEndian, value)** Write a byte to `dataPin`, followed by toggling the `clockPin`. [Understanding Big and Little Endian Byte Order](http://betterexplained.com/articles/understanding-big-and-little-endian-byte-order/)

- **wait(milliseconds, handler())** Register a handler to be called once in another execution turn and after `milliseconds` has elapsed.
  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    // Assuming an Led is attached to pin 13
    this.pinMode(13, this.MODES.OUTPUT);

    // Turn it on...
    this.digitalWrite(13, 1);

    this.wait(1000, function() {
      // Turn it off...
      this.digitalWrite(13, 0);
    });
  });
  ```

- **loop(milliseconds, handler())** Register a handler to be called repeatedly, in another execution turn, every `milliseconds` period.
  ```js
  var five = require("johnny-five");
  var board = new five.Board();

  board.on("ready", function() {
    var state = 0;

    this.pinMode(13, this.MODES.OUTPUT);

    this.loop(500, function() {
      this.digitalWrite(13, (state ^= 1));
    });
  });
  ```

## Events

- **connect** This event will emit once the program has "connected" to the board. This may be immediate, or after some amount of time, but is always asynchronous. For on-board execution, `connect` should emit as soon as possible, but asynchronously.

- **ready** This event will emit _after_ the **connect** event and only when the `Board` instance object has completed any hardware initialization that must take place before the program can operate. This process is asynchronous, and completion is signified to the program via a "ready" event. For on-board execution, `ready` should emit after `connect`. 

- **info** This event will emit for `board.info(class, message [, ...])`
  ```js
  board.on("info", function(event) {
    /*
      Event {
        type: "info"|"warn"|"fail",
        timestamp: Time of event in milliseconds,
        class: name of relevant component class,
        message: message [+ ...detail]
      }
    */
    console.log("%s sent an 'info' message: %s", event.class, event.message);
  });
  ```
  
- **warn** This event will emit for `board.warn(class, message [, ...])`
  ```js
  board.on("warn", function(event) {
    /*
      Event {
        type: "info"|"warn"|"fail",
        timestamp: Time of event in milliseconds,
        class: name of relevant component class,
        message: message [+ ...detail]
      }
    */
    console.log("%s sent a 'warn' message: %s", event.class, event.message);
  });
  ```

- **fail** This event will emit for `board.fail(class, message [, ...])`
  ```js
  board.on("fail", function(event) {
    /*
      Event {
        type: "info"|"warn"|"fail",
        timestamp: Time of event in milliseconds,
        class: name of relevant component class,
        message: message [+ ...detail]
      }
    */
    console.log("%s sent a 'fail' message: %s", event.class, event.message);
  });
  ```

- **message** This event will emit for any logging message: `info`, `warn` or `fail`.
  ```js
  board.on("message", function(event) {
    /*
      Event {
        type: "info"|"warn"|"fail",
        timestamp: Time of event in milliseconds,
        class: name of relevant component class,
        message: message [+ ...detail]
      }
    */
    console.log("Received a %s message, from %s, reporting: %s", event.type, event.class, event.message);
  });
  ```

<!--remove-start-->

## Examples

- [Board](https://github.com/rwldrn/johnny-five/blob/master/docs/board.md)
- [Board With Port](https://github.com/rwldrn/johnny-five/blob/master/docs/board-with-port.md)
- [Board Multi](https://github.com/rwldrn/johnny-five/blob/master/docs/board-multi.md)
- [Repl](https://github.com/rwldrn/johnny-five/blob/master/docs/repl.md)

<!--remove-end-->