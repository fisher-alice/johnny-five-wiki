The `ShiftRegister` class constructs an object that represents a shift register.

## Parameters

* **options** An object of constructor parameters

  | Name | Type   | Properties             | Description                                        | Required |
  |------|--------|------------------------|----------------------------------------------------|----------|
  | pins | Object | `{ data, clock, latch }` | Sets the values of the data , clock and latch pins | yes      |


## Shape

```
{
  id: A user definable id value. Defaults to a generated uid
  pins : the object containing the pin values for data, clock and latch 
}
```

## Component Initialization

```js
var register = new five.ShiftRegister({
  pins: {
    data: 2,
    clock: 3,
    latch: 4
  }
});
```

![Shift Register](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/shift-register.png)

## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

// This works with the 74HC595 that comes with the SparkFun Inventor's kit.
// Your mileage may vary with other chips. For more information on working
// with shift registers, see http://arduino.cc/en/Tutorial/ShiftOut

board.on("ready", function() {
  var register = new five.ShiftRegister({
    pins: {
      data: 2,
      clock: 3,
      latch: 4
    }
  });

  var value = 0;

  function next() {
    value = value > 0x11 ? value >> 1 : 0x88;
    register.send(value);
    setTimeout(next, 200);
  }

  next();
});
```

## API

- **send(value)** send a value to the shift register.

## Events

`ShiftRegister` objects are output only and therefore do not emit any events.


<!--remove-start-->

## Examples

* [Shiftregister](https://github.com/rwaldron/johnny-five/blob/master/docs/shiftregister.md)

<!--remove-end-->