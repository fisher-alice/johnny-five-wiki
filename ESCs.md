The `ESCs` class constructs a collection object containing multiple ESC objects. Any method called on an ESCs object will be called on each member of the ESCs object with the same parameters.

For working with a single ESC check out the [ESC](https://github.com/rwaldron/johnny-five/wiki/esc) class.

## Parameters

- **numsOrObjects** An array of pins, ESC parameter objects and/or ESC objects:
  <span class="abbreviate-table">
  
  | Property | Type           | Value/ Description                     | Default | Required |
  |----------|----------------|-----------------------|---------------------------------|----------|
  | numsOrObjects       | Array | An element for each ESC. Any valid [ESC parameters](https://github.com/rwaldron/johnny-five/wiki/esc#parameters) will work  |  | yes       |
  </span>

## Component Initialization

###With Pins
````
// Create two basic ESCs
//
new five.ESCs([9, 10]);
````

###With Objects
````
// Create two ESCs
//
new five.ESCs([{
  pin: 9
}, {
  pin: 10
}]);
````


## Usage

Control all members simultaneously:
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var escs = new five.ESCs([3, 5]);

  // Set ll ESCs to max.
  escs.max();
});
```

![ESCs](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/esc-array.png)

Control a single ESC in an ESCs instance:
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var escs = new five.ESCs([9, 10]);

  // Set the ESC on pin 9 to 90% of max speed.
  escs[0].speed(90);
});
```

Using multiple controllers in a single ESCs instance:
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var escs = new five.ESCs([
    { controller: "PCA9685", pin: 0 }, // Attached to an Adafruit PWM shield
    { pin: 3 } // Attached directly to the Arduino
  ]);

  // Set both escs to 75% of max speed.
  escs.speed(75);
});
```

![Two Controllers](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/escs-2-controllers.png)

Using ESC objects in ESCs:
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var escA = new five.ESC(9);
  var escB = new five.ESC(10);

var motors = new five.ESCs([escA, escB]);

  // Set ESCs independently
  escA.speed(20);
  escB.speed(90);

  // Set all ESCs to max speed.
  escs.max();

});
```

## API

All methods and properties in the [ESC API](https://github.com/rwaldron/johnny-five/wiki/esc#api) are available on ESCs

## Events

Events are emitted on the individual ESC objects so listeners must be attached there. See [ESC events](https://github.com/rwaldron/johnny-five/wiki/esc#events)

<!--remove-start-->

## Examples

- [ESCs](http://johnny-five.io/examples/esc-array)

<!--remove-end-->