The `Motors` class constructs a collection object containing multiple motor objects. Any method called on a `Motors` object will be called on each entry of the `Motors` object with the same parameters.

See also: 

- [Motor](motor)


## Parameters

- **numsOrObjects** An array of pins, motor parameter objects and/or Motor objects:
  <span class="abbreviate-table">
  
  | Property | Type           | Value/ Description                     | Default | Required |
  |----------|----------------|-----------------------|---------------------------------|----------|
  | numsOrObjects       | Array | An entry for each motor. Any valid [motor parameters](https://github.com/rwaldron/johnny-five/wiki/motor#parameters) will work  |  | yes       |
  </span>

## Component Initialization

### With Pins

```js
// Create three non-directional motors
//
new five.Motors([9, 10, 11]);
```

### With Objects

```js
// Create two customized motors
//
new five.Motors([{
  pins: {
    pwm: 3,
    dir: 12
  }
}, {
  pins: {
    pwm:9,
    dir:8,
    cdir: 11  
  }
}]);
```


## Usage

Control all members simultaneously:

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var motors = new five.Motors([
    { dir: 7, pwm: 6 },
    { dir: 8, pwm: 9 },
  ]);

  motors.forward(255);
});
```



Control a single motor in a Motors instance:

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var motors = new five.Motors([
    { dir: 4, cdir: 5, pwm: 6 },
    { dir: 7, cdir: 8, pwm: 9 },
  ]);


  // Sweep the motor on pin 9 from 0-180 and repeat.
  motors[0].forward(255);
});
```

Using multiple controllers in a single Motors instance:

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var motors = new five.Motors([
    { controller: "PCA9685", pins: { dir: 4, cdir: 5, pwm: 6 } },
    { pins: { dir: 4, cdir: 5, pwm: 6 } }
  ]);

  // Sweep both motors from 0-180 and repeat.
  motors.forward(255);
});
```


## API

All methods in the [Motor API](https://github.com/rwaldron/johnny-five/wiki/motor#api) are available on Motors
