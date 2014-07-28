The `Led.Matrix` class constructs an object that may represent one or more (chained) 8x8 LED Matrix (MAX7219/MAX7221) devices attached to the physical board. 

<img src="https://cdn.sparkfun.com//assets/parts/8/2/6/4/11861-04.jpg">

<img src="https://d3s5r33r268y59.cloudfront.net/27271/products/thumbs/2014-06-04T15:00:16.610Z-481.jpg.855x570_q85_pad_rcrop.jpg">

Known supported devices: 

[See all on Tindie!](https://www.tindie.com/search/?q=MAX7219)

- [MAX7219 Dot Matrix MCU LED Display Control Module Kit For Arduino With Dupont Cable](https://www.tindie.com/products/mmm999/max7219-dot-matrix-mcu-led-display-control-module-kit-for-arduino-with-dupont-cable/)
- [LED Matrix Kit](https://www.sparkfun.com/products/11861)
- [DIY MAX7219 Red LED Dot Matrix Display Module for Arduino](http://www.amazon.com/gp/product/B009U7LAS0/)
- [Dot Matrix Display Kit w/MAX7219 IC, PCB](http://www.electrodragon.com/product/dot-matrix-display-kit-wmax7219-ic-pcb/)
- [Dot Matrix Chain Display Kit Max7219 V2](http://www.electrodragon.com/product/dot-matrix-chain-display-kit-max7219-v2/)


Adafruit offers a selection of 8x8 matrices in various colors: 

- [White](https://www.adafruit.com/products/1821)
- [Amber](https://www.adafruit.com/products/1818)
- [Yellow](https://www.adafruit.com/products/1819)
- [Green](https://www.adafruit.com/products/1820)
- [Blue](https://www.adafruit.com/products/1817)

(This list only represents devices that I've personally confirmed, please add devices as needed.)
### Parameters


- **options** An object of property parameters.
<table>
  <thead>
    <tr>
      <th>Property Name</th>
      <th>Type</th>
      <th>Value(s)</th>
      <th>Properties</th>
      <th>Required</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>pins</td>
      <td>Object</td>
      <td>A valid pins object or pins array</td>
      <td>data, clock, cs</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>devices</td>
      <td>Number</td>
      <td>1-n</td>
      <td>
        For single device cases, this can be omitted. Defaults to 1.
      </td>
      <td>no</td>
    </tr>

  </tbody>
</table>
```js
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});

```

### Shape

```js
{ 
  isMatrix: true. READONLY
  devices: ...Number of devices controlled. READONLY
}
```




### Usage
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var matrix = new five.Led.Matrix({
    pins: {
      data: 2,
      clock: 3,
      cs: 4
    }, 
    devices: 1
  });

  matrix.on();

  this.repl.inject({
    display: matrix.device(0)
  });
});
```


## API

> NOTE: When the device is turned off (with the `off` method), data is retained and will be displayed on the device when turned on (with the `on` method). This is useful when powering the devices from a battery, by providing a power saving mechanism.

- **on()** Turn on all matrix devices.
- **on(device index)** Turn on matrix device at specified device index.

```js
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});

// Turn on a specific device by device index.
matrix.on(0);

// Turn on all devices
matrix.on();
```

- **off()** Turn off all matrix devices.
- **off(device index)** Turn off matrix device at specified device index.

```js
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});

// Turn off a specific device by device index.
matrix.off(0);

// Turn off all devices
matrix.off();

// Send data to the the matrix
// ...Turn on the device to see the data displayed
```


- **clear()** Shut off all LEDs, for all devices.
- **clear(device index)** Shut off all LEDs, for a device at specified device index.

Note: `clear()` does not shut off the device.

```js
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});

// Clear the entire display of a specified device.
matrix.clear(0);

// Clear the entire display for all devices
matrix.clear();
```


- **brightness(0-100)** Set the brightness from 0-100%, of all devices.
- **brightness(device index, 0-100)** Set the brightness from 0-100%, for a device at specified `device index`.


```js
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});

// Set the brightness of a specified device.
matrix.brightness(0, 100);

// Set the brightness for all devices
matrix.brightness(100);
```


- **led(row, col, 1|0)** Set on/off state for led at `row` and `col` (0-8, 0-8), of all devices.
- **led(device index, row, col, state)** Set on/off state for led at `row` and `col` (00-8), for a device at the specified `device index`.

```js
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});

// Turn on the top-left led of the specified device
matrix.led(0, 0, 0, 1);

// Turn on the top-left led of all devices
matrix.led(0, 0, 1);
```


- **row(row, 0-255)** Set `row` (0-8) value to 8-bit byte (0-255), of all devices.
- **row(device index, row, 0-255)** Set `row` (0-8) value to 8-bit byte (0-255), for a device at the specified `device index`.

```js
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});

// Turn on the entire first row of the specified device
matrix.row(0, 0, 255);

// Turn on the entire first row of all devices
matrix.row(0, 255);
```


- **column(column, 0-255)** Set `column` (0-8) value to 8-bit byte (0-255), of all devices.
- **column(device index, column, 0-255)** Set `column` (0-8) value to 8-bit byte (0-255), for a device at the specified `device index`.

```js
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});

// Turn on the entire first column of the specified device
matrix.column(0, 0, 255);

// Turn on the entire first column of all devices
matrix.column(0, 255);
```

- **draw(character)** Draw a "character" to all devices.
- **draw(device index, character)** Draw a "character" to a device at the specified `device index`.

Valid "character" values: 

- A single character string, eg. `"A", "b", "1", "$"`
- An 8 element array of 8-bit values, eg. `[0x00, 0x04, 0x15, 0x0E, 0x15, 0x04, 0x00, 0x00]` (That array represents `*` character).
- An 8 element array of 8-bit binary string values, eg.
```js
[
  "01100110",
  "10011001",
  "10000001",
  "10000001",
  "01000010",
  "00100100",
  "00011000",
  "00000000"
];
```


```js
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});

var heart = [
  "01100110",
  "10011001",
  "10000001",
  "10000001",
  "01000010",
  "00100100",
  "00011000",
  "00000000"
];

// Draw a heart to the specified device
matrix.draw(0, heart);

// Draw a heart to all devices
matrix.draw(heart);

// NOTE: this is actually a predefined character
var exclamation = [
  0x04, 
  0x04, 
  0x04, 
  0x04, 
  0x00, 
  0x00, 
  0x04, 
  0x00
];

// Draw an exclamation to the specified device
matrix.draw(0, exclamation);

// Draw an exclamation to all devices
matrix.draw(exclamation);

var asterisk = "*";

// Draw an asterisk to the specified device
matrix.draw(0, asterisk);

// Draw an asterisk to all devices
matrix.draw(asterisk);
```



## Predefined Characters

Led.Matrix.CHARS

```
0 1 2 3 4 5 6 7 8 9   
! " # $ % & ' ( ) * + , - . / : ; < = > ? @ 
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z [ \ ] ^ _ ` 
a b c d e f g h i j k l m n o p q r s t u v w x y z { | } ~
```


## Events

`Led.Matrix` objects are output only and therefore do not emit any events.
