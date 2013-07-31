# Board

The `Board` class constructs objects that represent the physical board itself.

### Parameters

**options**
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
      <td>port</td>
      <td>Path or name of device port/COM</td>
      <td>no</td>
    </tr>
  </tbody>
</table>

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


# Boards

The `Boards` class constructs a collection object containing multiple board objects. If no arguments are passed, `Board` objects will be created for every board detected in the order that the system enumerates them. 

### Parameters

**ports** A list of port objects or port address strings

**port**
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
      <td>id</td>
      <td>A string identifier for use in your program</td>
      <td>no</td>
    </tr>
    <tr>
      <td>port</td>
      <td>A string path or name of device port/COM</td>
      <td>no</td>
    </tr>

  </tbody>
</table>

### Initializing a Board

The easiest way to initialize multiple board objects is to call the `Boards` constructor function with `new`. Don't worry about knowing your device's path or COM port, Johnny-Five will figure out which USBs are in use by compatible boards automatically.


```js
// Create 2 board instances with IDs "A" & "B"
new five.Boards([ "A", "B" ])
```

Or

```js
var ports = [
  { id: "A", port: "/dev/cu.usbmodem621" },
  { id: "B", port: "/dev/cu.usbmodem411" }
];

new five.Boards(ports);
```

### Board Ready

Once the board objects have been initialized, they must connect to the physical boards with a set of handshake steps, once this has completed, the boards are ready to communicate with the program. This process is asynchronous, and signified to the program via a "ready" event.

```js
// Create 2 board instances with IDs "A" & "B"
new five.Boards([ "A", "B" ]).on("ready", function() {

  // Both "A" and "B" are initialized
  // (connected and available for communication)

});
```

Override this by providing explicit port paths:


```js
var ports = [
  { id: "A", port: "/dev/cu.usbmodem621" },
  { id: "B", port: "/dev/cu.usbmodem411" }
];

new five.Boards(ports).on("ready", function() {

  // Both "A" and "B" are initialized
  // (connected and available for communication)

});
```

### Usage

A basic, but complete example usage of the `Boards` constructor:
```js
// Create 2 board instances with IDs "A" & "B"
new five.Boards([ "A", "B" ]).on("ready", function() {

  // Both "A" and "B" are initialized
  // (connected and available for communication)

  // |this| is an array-like object containing references
  // to each initialized board.
  this.each(function(board) {

    // Initialize an Led instance on pin 13 of
    // each initialized board and strobe it.
    new five.Led({ pin: 13, board: board }).strobe();
  });
});
```

Override this by providing explicit port paths:


```js
var ports = [
  { id: "A", port: "/dev/cu.usbmodem621" },
  { id: "B", port: "/dev/cu.usbmodem411" }
];

new five.Boards(ports).on("ready", function() {

  // Both "A" and "B" are initialized
  // (connected and available for communication)

  // |this| is an array-like object containing references
  // to each initialized board.
  this.each(function(board) {

    // Initialize an Led instance on pin 13 of
    // each initialized board and strobe it.
    new five.Led({ pin: 13, board: board }).strobe();
  });
});
```



**NOTE** When using multiple boards, all device classes must be initialized with an explicit reference to the board object that they will be associated to. This is illustrated in the previous code example.





## API

**each(callback(board, index))** Call a function once for each board object.