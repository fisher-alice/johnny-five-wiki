The `Board` class constructs objects that represent the physical board itself. All device objects depend on an initialized and ready board object.

Johnny-Five has been tested on, but is not limited to, the following boards:

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

For non-Arduino based projects, the following platform [IO Plugins](https://github.com/rwaldron/johnny-five/wiki/IO-Plugins) are available:

- Beagle Bone
  - [BeagleBone-IO](https://github.com/julianduque/beaglebone-io)
- Intel Galileo, Edison
  - [Galileo-IO](https://github.com/rwaldron/galileo-io/)
- Pinoccio
  - [Pinoccio-IO](https://github.com/soldair/pinoccio-io/)
- Raspberry Pi
  - [Raspi-IO](https://gitlab.theoreticalideations.com/nebrius/raspi-io/)
- Spark Core
  - [Spark-IO](https://github.com/rwaldron/spark-io/)
- IO Board (Generic IO Plugin class to make your own!)
  - [IOBoard](https://github.com/achingbrain/node-ioboard)
- Electric Imp
  - [Imp-IO](https://github.com/rwaldron/imp-io)

See also: [Multi-Board Support](https://github.com/rwldrn/johnny-five/wiki/Boards)

### Parameters

- **options** Optional object of themselves optional parameters.
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
      <td>id</td>
      <td>Number, String</td>
      <td>Any</td>
      <td>User definable identification</td>
      <td>no</td>
    </tr>
    <tr>
      <td>port</td>
      <td>String or object</td>
      <td>"/dev/ttyAM0", "COM1", new SerialPort()</td>
      <td>Path or name of device port/COM or SerialPort object</td>
      <td>no</td>
    </tr>
    <tr>
      <td>repl</td>
      <td>Boolean</td>
      <td>true, false</td>
      <td>Set to `false` to disable REPL</td>
      <td>no</td>
    </tr>

  </tbody>
</table>

### Shape
```js
{ 
  ready: ...A boolean value that indicates the readiness of the physical board
  firmata: ...A reference to the protocol layer
  id: ...A user definable id value. Defaults to a generated uid
  pins: ...An array of all pins produced by the capabilities query
  type: ...A string identifier of the board type, one of: "UNO", "MEGA", "LEONARDO" or "UNKOWN"
  repl: ...A reference to the repl session object
  port: A string illustrating the device path or COM address
}
```

### Initializing a Board

The easiest way to initialize a board object is to call the `Board` constructor function with `new`. Don't worry about knowing your device's path or COM port, Johnny-Five will figure out which USB the board is plugged into and connect to that automatically.

```js
var five = require("johnny-five"),
    board = new five.Board();
```

You may optionally specify the port by providing it as a property of the options object parameter:

```js
var five = require("johnny-five"),
    board = new five.Board({ port: "/dev/tty.usbmodemNNNN" });
```

or 

```js
var five = require("johnny-five"),
    board = new five.Board({ port: "COM1" });
```

or you can specify a SerialPort object by providing it as a property of the options object parameter:

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


### Board Ready

Once the board object has been initialized, it must connect to the physical board with a set of handshake steps, once this has completed, the board is ready to communicate with the program. This process is asynchronous, and signified to the program via a "ready" event.

```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // The board can now communicate with this program.
});
```

### Usage

A basic, but complete example usage of the `Board` constructor:


```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {

  // Create an Led on pin 13
  var led = new five.Led(13);

  // Strobe the pin on/off, defaults to 100ms phases
  led.strobe();

});
```

## API

- **repl** This is a property of the board object that represents the automatically generated REPL that's created when a Johnny-Five program is executed. This object has an `inject` method that may be called as many times as desired. 

- **repl.inject(object)** Inject objects or values, from the program, into the REPL session.
```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // Initialize an Led object that can be controlled from the REPL session
  this.repl.inject({
    led: new five.Led(13)
  });  
});
/*
  From the terminal...

  > node program.js
  1375291815062 Board Connecting... 
  1375291815081 Serial Found possible serial port /dev/whatever-this-port-is
  1375291815082 Board -> Serialport connected /dev/whatever-this-port-is
  1375291816115 Board <- Serialport ready /dev/whatever-this-port-is
  1375291816116 Repl Initialized 
  >> 
  (Since the led object is available here...)
  >> led.on();
  >> led.off();
  
*/
```

- **pinMode(pin, mode)** Set the `mode` of a specific `pin`, one of INPUT, OUTPUT, ANALOG, PWM, SERVO.
Mode constants are exposed via the `Pin` class
<table>
  <thead>
    <tr>
      <th>Mode</th>
      <th>Value</th>
      <th>Constant</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>INPUT</td>
      <td>0</td>
      <td>Pin.INPUT</td>
    </tr>
    <tr>
      <td>OUTPUT</td>
      <td>1</td>
      <td>Pin.OUTPUT</td>
    </tr>
    <tr>
      <td>ANALOG</td>
      <td>2</td>
      <td>Pin.ANALOG</td>
    </tr>
    <tr>
      <td>PWM</td>
      <td>3</td>
      <td>Pin.PWM</td>
    </tr>
    <tr>
      <td>SERVO</td>
      <td>4</td>
      <td>Pin.SERVO</td>
    </tr>
  </tbody>
</table>
```js
// Set a pin to INPUT mode
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  
  // pin mode constants are available on the Pin class
  this.pinMode(13, five.Pin.INPUT);
});
```

- **analogWrite(pin, value)** Write an analog value (0-255) to a digital `pin`.
```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // Assuming an Led is attached to pin 9, this will turn it on at full brightness
  // PWM is the mode used to write ANALOG signals to a digital pin
  this.pinMode(9, five.Pin.PWM);
  this.analogWrite(9, 255);
});
```

- **analogRead(pin, handler(voltage))** Register a handler to be called whenever the board reports the voltage value (0-1023) of the specified analog `pin`.
```js
var five = require("johnny-five"),
    board = new five.Board();

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
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // Assuming an Led is attached to pin 13, this will turn it on
  this.pinMode(13, five.Pin.OUTPUT);
  this.digitalWrite(13, 1);
});
```

- **digitalRead(pin, handler(value))** Register a handler to be called whenever the board reports the value (0 or 1) of the specified digital `pin`.
```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // Assuming a button is attached to pin 9
  this.pinMode(9, five.Pin.INPUT);
  this.digitalRead(9, function(value) {
    console.log(value);
  });
});
```

- **shiftOut(dataPin, clockPin, isBigEndian, value)** Write a byte to `dataPin`, followed by toggling the `clockPin`. [Understanding Big and Little Endian Byte Order](http://betterexplained.com/articles/understanding-big-and-little-endian-byte-order/)

- **wait(ms, handler())** Register a handler to be called once in another execution turn and after the amount of time specified in milliseconds has passed.
```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // Assuming an Led is attached to pin 13
  this.pinMode(13, five.Pin.OUTPUT);

  // Turn it on...
  this.digitalWrite(13, 1);

  this.wait(1000, function() {
    // Turn it off...
    this.digitalWrite(13, 0);
  });
});
```

- **loop(ms, handler())** Register a handler to be called repeatedly in another execution turn, every time the specified milliseconds has lapsed. 
```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  var byte;

  // Assuming an Led is attached to pin 13
  this.pinMode(13, five.Pin.OUTPUT);

  // Homemade strobe
  this.loop(500, function() {
    this.digitalWrite(13, (byte ^= 0x01));
  });
});
```

## Examples

- [Board](https://github.com/rwldrn/johnny-five/blob/master/docs/board.md)
- [Board With Port](https://github.com/rwldrn/johnny-five/blob/master/docs/board-with-port.md)
- [Board Multi](https://github.com/rwldrn/johnny-five/blob/master/docs/board-multi.md)
- [Repl](https://github.com/rwldrn/johnny-five/blob/master/docs/repl.md)