The `Servos` class constructs a collection object containing multiple servo objects. Any method called on a Servos object will be called on each member of the Servos object with the same parameters.

## Parameters

- **numsOrObjects** An array of pins, servo parameter objects and/or Servo objects:
  <span class="abbreviate-table">
  
  | Property | Type           | Value/ Description                     | Default | Required |
  |----------|----------------|-----------------------|---------------------------------|----------|
  | numsOrObjects       | Array | An element for each servo. Any valid [servo parameters](#parameters) will work  |  | yes       |
  </span>

## Component Initialization

###With Pins
````
// Create three basic servos
//
new five.Servos([9, 10, 11]);
````

###With Objects
````
// Create two customized servos
//
new five.Servos([{
  pin: 9, 
  center: true
}, {
  pin: 10, 
  range: [20,140],
  startAt: 20
}]);
````


## Usage

Control all members simultaneously:
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var servos = new five.Servos([9, 10]);

  // Sweep all servos in the object from 0-180 and repeat.
  servos.sweep();
});
```

Control a single servo in a Servos instance:
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var servos = new five.Servos([9, 10]);

  // Sweep the servo on pin 9 from 0-180 and repeat.
  servos[0].sweep();
});
```

Using Servo objects in Servos:
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var shoulder = new five.Servo(9);
  var elbow = new five.Servo(10);

var joints = new five.Servos([shoulder, elbow]);

  // move servos independently
  elbow.to(20);
  shoulder.to(180);

  // Center all servos.
  joints.center();

});
```

## API

All methods and properties in the [Servo API](#api) are available on Servo

## Events

Events are emitted on the individual Servo objects so listeners must be attached there.

<!--remove-start-->

## Examples

- [Servos](https://github.com/rwldrn/johnny-five/blob/master/docs/servo-array.md)

<!--remove-end-->