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