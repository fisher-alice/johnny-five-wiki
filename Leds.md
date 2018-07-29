The `Leds` class constructs a collection object containing multiple Led objects. Any method called on an Leds object will be called on each member of the Leds object with the same parameters.

For working with a single Led check out the [Led](https://github.com/rwaldron/johnny-five/wiki/led) class.

## Parameters

- **numsOrObjects** An array of pins, Led parameter objects and/or Led objects:
  <span class="abbreviate-table">
  
  | Property | Type           | Value/ Description                     | Default | Required |
  |----------|----------------|-----------------------|---------------------------------|----------|
  | numsOrObjects       | Array | An element for each Led. Any valid [Led parameters](https://github.com/rwaldron/johnny-five/wiki/led#parameters) will work  |  | yes       |
  </span>

## Component Initialization

###With Pins
````
// Create three basic Leds
//
new five.Leds([9, 10, 11]);
````

###With Objects
````
// Create two Leds
//
new five.Leds([{
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

  var leds = new five.Leds([3, 5, 6]);

  // Pulse all leds in the object.
  leds.pulse();
});
```

![leds](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-array.png)

Control a single Led in an Leds instance:
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var leds = new five.Leds([9, 10]);

  // Pulse the Led on pin 9.
  leds[0].pulse();
});
```

Using multiple controllers in a single Leds instance:
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var leds = new five.Leds([
    { controller: "PCA9685", pin: 0 }, // Attached to an Adafruit PWM shield
    { controller: "PCA9685", pin: 1 }, // Attached to an Adafruit PWM shield
    { pin: 3 } // Attached directly to the Arduino
  ]);

  // Pulse both leds.
  leds.pulse();
});
```

![Two Controllers](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-array-2-controllers.png)

Using Led objects in Leds:
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var ledA = new five.Led(9);
  var ledB = new five.Led(10);

var lights = new five.Leds([ledA, ledB]);

  // Set leds independently
  ledA.brightness(20);
  ledB.brightness(255);

  // Pulse all Leds.
  lights.pulse();

});
```

## API

All methods and properties in the [Led API](https://github.com/rwaldron/johnny-five/wiki/led#api) are available on Leds

## Events

Events are emitted on the individual Led objects so listeners must be attached there. See [Led events](https://github.com/rwaldron/johnny-five/wiki/led#events)

<!--remove-start-->

## Examples

- [Leds](http://johnny-five.io/examples/led-array)

<!--remove-end-->