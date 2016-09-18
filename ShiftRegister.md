The `ShiftRegister` class constructs an object that represents a shift register.

## Parameters

* **pins** An array of pins in `data, clock, latch, [reset]` order
* **options** An object of constructor parameters
  <span class="abbreviate-table">

  | Name | Type   | Value/Description                                        | Default| Required |
  |------|--------|------------------------|----------------------------------------------------|----------|
  | pins | Object | `{data, clock, latch, [reset]}` | | Yes (either)     |
  | pins | Array | `[data, clock, latch, [reset]]` | | Yes (either)     |
  | isAnode | Boolean | `true` or `false`. Initialize shift register to output for common anode device. | `false` | No     |
  </span>

## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `pins` | the object containing the pin values for data, clock and latch | No |
| `value` | the object containing the pin values for data, clock and latch | Yes |
| `isAnode` | the object containing the pin values for data, clock and latch | Yes |

## Component Initialization


#### Default 

```js
new five.ShiftRegister({
  pins: {
    data: 2,
    clock: 3,
    latch: 4
  }
});
```

```js
new five.ShiftRegister({
  pins: [2, 3, 4]
});
```

```js
new five.ShiftRegister([2, 3, 4]);
```

![Shift Register](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/shift-register.png)
![Shift Register Seven Segment](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/shift-register-seven-segment.png)

#### Common Anode

```js
new five.ShiftRegister({
  isAnode: true,
  pins: {
    data: 2,
    clock: 3,
    latch: 4
  }
});
```

```js
new five.ShiftRegister({
  isAnode: true,
  pins: [2, 3, 4]
});
```

![Shift Register Seven Segment Common Anode](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/shift-register-seven-segment-anode.png)

## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var register = new five.ShiftRegister({
    isAnode: true,
    pins: {
      data: 2,
      clock: 3,
      latch: 4,
      reset: 9,
    }
  });
  var number = 0;
  var decimal = 0;

  register.reset();

  // Display numbers 0-9, one at a time in a loop.
  // Shows just the number for a half second, then
  // the number + a decimal point for a half second.
  setInterval(function() {
    register.display(number + (decimal && "."));

    if (decimal) {
      number++;
    }

    if (number > 9) {
      number = 0;
    }

    decimal ^= 1;
  }, 500);
});

```

## API

- **clear()** Clear the register.

- **display(number)** Display a number on a seven segment display. 
  ```js
  // Display a 1 on a single seven segment controlled 
  // by the shift register.
  register.display(1);
  ```
- **display(string)** Display a number on a seven segment display, optionally including the decimal point. 
  ```js
  // Display a 1 on a single seven segment controlled 
  // by the shift register. The decimal point will also be lit
  register.display("1.");

  // Display a 1 on a single seven segment controlled 
  // by the shift register. The decimal point will NOT be lit
  register.display("1");
  ```

- **reset()** Reset the register. 
- **send(B)** Send an 8-bit byte value to the shift register. 
- **send([B1, B2])** Send an array of 8-bit byte values to the shift register.




## Events

`ShiftRegister` objects are output only and therefore do not emit any events.

