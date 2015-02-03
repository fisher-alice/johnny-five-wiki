The `Relay` class constructs objects that represent a single digital Relay  attached to the physical board.

## Parameters

- **pin** A Number or String address for the pin.

- **options** An object of property parameters.
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
      <td>pin</td>
      <td>Number, String</td>
      <td>Any Pin</td>
      <td>The Number or String address of the Relay pin</td>
      <td>yes</td>
    </tr>

    <tr>
      <td>type</td>
      <td>String</td>
      <td>"NO", "NC"</td>
      <td>Normally Open or Normally Closed. Defaults to "NO"</td>
      <td>no</td>
    </tr>
  </tbody>
</table>


## Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pin: The pin value.
  isOn: true|false. READONLY
  type: "NO" or "NC". READONLY
}
```


## Component Initialization

```js
// Create a Relay object:
// 
var relay = new five.Relay({
  pin: 10
});

// Create a Normally Closed Relay object:
// 
var relay = new five.Relay({
  pin: 10, 
  type: "NC"
});
```

![Relay](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/relay.png)

## Usage
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var relay = new five.Relay(10);

  // Control the relay in real time
  // from the REPL by typing commands, eg.
  //
  // relay.on();
  //
  // relay.off();
  //
  // OR...
  //
  // relay.open();
  //
  // relay.close();
  //
  this.repl.inject({
    relay: relay
  });
});
```

## API

- **open()** Open the circuit.

- **close()** Close the circuit.

- **toggle()** Toggle the circuit open/close.

## Events

Relay objects are output only and therefore do not emit any events.
