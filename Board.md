# Board

The `Board` class constructs objects that represent the physical board itself.

## Parameters

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

## Initializing a Board

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


## Board Ready

Once the the board object has been initialized, it must connect to the physical board with a set of handshake steps, once this has completed, the board is ready to communicate with the program. This process is asynchronous, and signified to the program via a "ready" event.

```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // The board can now communicate with this program.
});
```

## Usage

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