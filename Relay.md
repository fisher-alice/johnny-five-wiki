![](http://i.gyazo.com/1aaab47df9a262baa36bd7ba515b4cbc.png)

The `Relay` class constructs objects that represent a single digital Relay attached to the physical board.

See also: 

- [Relays](https://github.com/rwaldron/johnny-five/wiki/relays)

## Parameters

- **pin** A Number or String address for the pin.

- **options** An object of property parameters.
  <span class="abbreviate-table">

  | Property | Type           | Value/Description                                        | Default | Required |
  |---------------|----------------|------------|----------------------------------------------------|----------|
  | pin           | Number, String | Any Pin. The Number or String address of the Relay pin     | | yes      |
  | type          | String         | “NO”, “NC”. Normally Open or Normally Closed. | “NO” | no       |
  </span>

## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `pin` | The pin value. | No |
| `isOn` | `true` or `false`. | Yes |
| `type` | `"NO"` or `"NC"`. | Yes |


## Component Initialization

#### Normally Open (default)

```js
// Pin only
new five.Relay(10);

// Options object with pin property
// Defaults to Normally Open: "NO"
new five.Relay({
  pin: 10
});
```

![Relay Open](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/relay-open.png)


#### Normally Closed 

```js
// Options object with pin and type properties
new five.Relay({
  pin: 10, 
  type: "NC"
});
```

![Relay](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/relay-closed.png)

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
