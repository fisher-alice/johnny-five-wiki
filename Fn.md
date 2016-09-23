The `Fn` object gives you access to a number of handy utility functions and "constants" for your Johnny-Five convenience. The source in `lib/fn.js` has extensive inline comments, as well. The `Fn` object is available in Johnny-Five's top-level context:

```js
var five = require('johnny-five');
var total = five.Fn.sum([3, 4, 2, 0, 5]); // -> 14
```

## API

### General Utility Functions

- **cloneDeep(value)** Reference to [lodash.cloneDeep](https://lodash.com/docs/4.16.1#cloneDeep)
- **constrain(value, lower, upper)** Constrain `value` so that it falls within the range bounded by `lower` and `upper`.

    ```js
    five.Fn.constrain(235, 0, 255); // --> 235
    five.Fn.constrain(948, 0, 255); // --> 255
    ```
- **debounce(func, [wait=0], [options={}])** Reference to [lodash.debounce](https://lodash.com/docs/4.16.1#debounce)
- **inRange(value, lower, upper)** Is the `value` within the range bounded by `lower` and `upper`?

    ```js
    five.Fn.inRange(235, 0, 255); // --> true
    five.Fn.inRange(948, 0, 255); // --> false
    ```
    
- **map(value, fromLow, fromHigh, toLow, toHigh)** Map a numeric `value` from one range to another. Based on Arduino's `map(...)`.

    ```js
    five.Fn.map(500, 0, 1000, 0, 255); // --> 127
    ```
    
- **fmap(...)** Like `Fn.map`, but the return value will be cast to a `Float32`.
- **range(lower, upper, tick)** Generate an Array of Numbers with element values ranging from `lower` to `upper`; the step (increment/decrement) between each is defined by `tick`. If `range` is invoked with a single argument, it will be used to determine the number of elements in the resulting Array.

    ```js
    five.Fn.range(5); // -> [0, 1, 2, 3, 4];
    five.Fn.range(5, 10); // -> [5, 6, 7, 8, 9, 10];
    five.Fn.range(3, 27, 3); // -> [3, 6, 9, 12, 15, 18, 21, 24, 27];
    five.Fn.range(0, -9, -3); // -> [0, -3, -6, -9];
    ```
    
- **range.prefixed(prefix, ...)** Adds prefix to each element in the range Array returned by `Fn.range`. The function passes on arguments (other than `prefix`) to the `range` function.

    ```js
    five.Fn.range.prefixed("A", 0, 10, 2); // -> ["A0", "A2", "A4", "A6", "A8", "A10"]
    ```
    
- **scale(...)** Alias for `Fn.map`
- **fscale(...)** Alias for `Fn.fmap`
- **square(x)** Square your `x`!
- **sum(values)** Calculate a sum of all the elements in an Array.

    ```js
    five.Fn.sum([3, 4, 2, 0, 5]); // -> 14
    ```
    
- **toFixed(number, digits)** Format a number such that it has a given number of digits after the decimal point.

    ```js
    five.Fn.toFixed(5.4564, 2); // -> 5.46
    five.Fn.toFixed(1.5, 2); // -> 1.5
    ```
    
- **uid()** Generate a reasonably-unique ID string.

### Functions for Manipulating Bits and Integers

- **bitSize(n)** Return the number of bits in a given number `n`.

    ```js
    five.Fn.bitSize(1000); // --> 10
    ```
    
- **bitValue(bit)** (also aliased as **_BV** and **bv**) Return a value with the bit at the position (`bit`) indicated set (to 1). (From avr/io.h "BV")

    ```
    An example: logically OR these bits together:
    var ORed = _BV(0) | _BV(2) | _BV(7);
    
    BIT         7  6  5  4  3  2  1  0
    ---------------------------------------------------------
    _BV(0)  =   0  0  0  0  0  0  0  1
    _BV(2)  =   0  0  0  0  0  1  0  0
    _BV(7)  =   1  0  0  0  0  0  0  0
    ORed    =   1  0  0  0  0  1  0  1
    
    ORed === 133;
    ```

#### Ints from Multiple Bytes

The following functions allow you to build 16-, 24- and 32-bit numbers by cobbling together multiple bytes:

- **int16(msb, lsb)** Combine `msb` (most-significant byte) and `lsb` (least-significant byte) into a 16-bit signed integer.

    ```js
    five.Fn.int16(8, 0); // --> 2048
    fiveFn.int16(255, 255); // --> -1 (because signed)
    ```
    
- **uint16(msb, lsb)** Combine `msb` (most-significant byte) and `lsb` (least-significant byte) into a 16-bit unsigned integer.

    ```js
    five.Fn.int16(8, 0); // --> 2048
    fiveFn.int16(255, 255); // --> 65535 (because unsigned)
    ```
    
- **int24(b16, b8, b0)** Combine three bytes to make a signed 24-bit integer.

    ```js
    five.Fn.int24(127, 255, 255); // --> 8388607
    five.Fn.int24(255, 255, 255); // --> -1 (because signed)
    ```
    
- **uint24(b16, b8, b0)** Combine three bytes to make an unsigned 24-bit integer.

    ```js
    five.Fn.int24(127, 255, 255); // --> 8388607
    five.Fn.int24(255, 255, 255); // --> 16777215 (because unsigned)
    ```
    
- **int32(b24, b16, b8, b0)** Combine four bytes to make a signed 32-bit integer.

    ```js
    five.Fn.int32(127, 255, 255, 255); // --> 2147483647
    five.Fn.int32(200, 255, 255, 255); // --> -922746881 (because signed)
    ```
    
- **uint32(b24, b16, b8, b0)** Combine three bytes to make an unsigned 32-bit integer.

```js
five.Fn.uint32(127, 255, 255, 255); // --> 2147483647
five.Fn.uint32(200, 255, 255, 255); // --> 4294967295 (because unsigned)
```

#### Auto-Generated Functions

The following functions are available in the bitSizes of: `4, 8, 10, 12, 16, 20, 24, 32`

- **u\[bitSize\](value)** (e.g. `Fn.u8`) Constrain `value` to the valid range of _unsigned_ ints for the given bitsize.

    ```js
    Fn.u8(255); // --> 255
    Fn.u8(256); // --> 255
    Fn.u8(0);   // --> 0
    Fn.u4(255); // --> 15
    ```

- **s\[bitSize\](value)** (e.g. `Fn.s16`) Constrain `value` to the valid range of _signed_ ints for the given bitsize.

    ```js
    Fn.s8(255); // --> -1
    Fn.s8(127); // --> 127
    Fn.s8(128); // --> -128
    ```

### Useful "Constants"

- **RAD_TO_DEG**
- **DEG_TO_RAD**
- **TAU**
- **POW_2_0** through **POW_2_53** Powers of 2
