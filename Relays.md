The `Relays` class constructs a collection object containing multiple relay objects. Any method called on a `Relays` collection will be called on each member of the `Relays` collection with the same parameters.

Once instantiated, a `Relays` collection object is static.

See also: 

- [Relay](https://github.com/rwaldron/johnny-five/wiki/relay)


## Parameters

- **numsOrObjects** An array of pins, relay parameter objects and/or Relay objects:
  <span class="abbreviate-table">
  
  | Property | Type           | Value/ Description                     | Default | Required |
  |----------|----------------|-----------------------|---------------------------------|----------|
  | numsOrObjects       | Array | An element for each relay. Any valid [relay parameters](https://github.com/rwaldron/johnny-five/wiki/relay#parameters) will work  |  | yes       |
  </span>

## Component Initialization

### With Pins

```js
// Create three basic relays
//
new five.Relays([9, 10, 11]);
```

### With Objects

```js
// Create two customized relays
//
new five.Relays([{
  pin: 9, 
  type: "NO",
}, {
  pin: 10, 
  type: "NC",
}]);
```


## Usage

Control all members simultaneously:

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var relays = new five.Relays([9, 10]);

  // Close all relay circuits.
  relays.close();
});
```

Control a single relay in a Relays instance:

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var relays = new five.Relays([9, 10]);

  // Close the relay on pin 9.
  relays[0].close();
});
```



Using Relay objects in Relays:


```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var r1 = new five.Relay(9);
  var r2 = new five.Relay(10);

  var joints = new five.Relays([r1, r2]);

  // Close all relays independently
  r1.close();
  r2.close();

  // Open all relays.
  relays.open();
});
```

## API

All methods and properties in the [Relay API](https://github.com/rwaldron/johnny-five/wiki/relay#api) are available on Relay

## Events

Events are emitted on the individual Relay objects so listeners must be attached there. See [Relay events](https://github.com/rwaldron/johnny-five/wiki/relay#events)