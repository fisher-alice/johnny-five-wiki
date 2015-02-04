The `Led.Matrix` class constructs an object that may represent one or more (chained) 8x8 or 8x16 LED Matrix (MAX7219, MAX7221 and HT16K33) devices attached to the physical board. Up to 8 devices can be controlled with one instance, giving you 512 controllable LEDs.


![](https://cdn.sparkfun.com//assets/parts/8/2/6/4/11861-04.jpg)

![](https://d3s5r33r268y59.cloudfront.net/27271/products/thumbs/2014-06-04T15:00:16.610Z-481.jpg.855x570_q85_pad_rcrop.jpg)


Known supported devices: 

[See all on Tindie!](https://www.tindie.com/search/?q=MAX7219)

- Shift Register Devices
  - MAX7219
    - [MAX7219 Dot Matrix MCU LED Display Control Module Kit For Arduino With Dupont Cable](https://www.tindie.com/products/mmm999/max7219-dot-matrix-mcu-led-display-control-module-kit-for-arduino-with-dupont-cable/)
    - [LED Matrix Kit](https://www.sparkfun.com/products/11861)
    - [DIY MAX7219 Red LED Dot Matrix Display Module for Arduino](http://www.amazon.com/gp/product/B009U7LAS0/)
    - [Dot Matrix Display Kit w/MAX7219 IC, PCB](http://www.electrodragon.com/product/dot-matrix-display-kit-wmax7219-ic-pcb/)
    - [Dot Matrix Chain Display Kit Max7219 V2](http://www.electrodragon.com/product/dot-matrix-chain-display-kit-max7219-v2/)

- I2C devices
  - HT16K33
    - [Adafruit 0.8 8x8 LED Matrix (Any color)](https://learn.adafruit.com/adafruit-led-backpack/0-8-8x8-matrix)
    - [Adafruit 1.2 8x8 LED Matrix bicolor](https://learn.adafruit.com/adafruit-led-backpack/bi-color-8x8-matrix)
    - [Adafruit 1.2 8x8 LED Matrix (Any color)](https://learn.adafruit.com/adafruit-led-backpack/1-2-8x8-matrix)
    - [Adafruit 1.2 16x8 LED Matrix (Any color)](https://www.adafruit.com/products/2040)
    - [OCROBOT 8x8 LED Matrix - bicolor](http://www.aliexpress.com/item/Free-Shipping-1pc-Red-and-green-color-dot-matrix-module-I2C-1-8-inches-3mm-8/1364967656.html)

If you are working with HT16K33 devices, you will need to run at least version 2.4.0 of Firmata on the board, currently available in [beta](http://sourceforge.net/projects/firmata/files/Firmata/).

Adafruit offers a selection of 8x8 matrices in various colors: 

- [White](https://www.adafruit.com/products/1821)
- [Amber](https://www.adafruit.com/products/1818)
- [Yellow](https://www.adafruit.com/products/1819)
- [Green](https://www.adafruit.com/products/1820)
- [Blue](https://www.adafruit.com/products/1817)

(This list only represents devices that I've personally confirmed, please add devices as needed.)

## Parameters


- **shift register options** An object of property parameters.
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
        <td>1-8</td>
        <td>
          For single device cases, this can be omitted. Defaults to 1.
        </td>
        <td>no</td>
      </tr>

    </tbody>
  </table>

- **I2C options** (Requires StandardFirmata 2.4.0 or greater)
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
        <td>controller</td>
        <td>string</td>
        <td>A valid controller model name</td>
        <td>HT16K33</td>
        <td>yes</td>
      </tr>
      <tr>
        <td>addresses</td>
        <td>array</td>
        <td>An array of I2C addresses</td>
        <td>Defaults to array of addresses in range 0x70 - 0x77, up to length specified by devices</td>
        <td>no</td>
      </tr>
      <tr> 
        <td>isBicolor</td>
        <td>Boolean</td>
        <td>true|false</td>
        <td>
          Defaults to false
        </td>
        <td>no</td>
      </tr>
      <tr> 
        <td>dims</td>
        <td>A valid dims object, array or string</td>
        <td>rows, columns</td>
        <td>
          The dimensions for the devices being used: either 8x8, 16x8 or 8x16.
        </td>
        <td>no</td>
      </tr>

    </tbody>
  </table>



## Shape

```
{ 
  isMatrix: true. READONLY
  devices: ...Number of devices controlled. READONLY
}
```


## Component Initialization


```js
// Shift Register device
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});
```

![Led.Matrix](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-matrix.png)

```js
// Led.Matrix HT16K33
var matrix = new five.Led.Matrix({ 
  controller: "HT16K33"
});
```

![Led.Matrix Mono Color](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-matrix-HT16K33.png)

```js
// Led.Matrix HT16K33
var matrix = new five.Led.Matrix({ 
  controller: "HT16K33",
  isBicolor: true
});
```

![Led.Matrix Mono Color](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-matrix-HT16K33.png)

// 16x8 I2C device
var matrix = new five.Led.Matrix({
  controller: "HT16K33",
  dims: "8x16", // or "16x8"
  rotation: 2
});
```

![Led.Matrix HT16K33 16x8](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-matrix-HT16K33-16x8.png)






## Usage

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

  matrix.draw(heart);
});
```


## API

> NOTE: An `Led.Matrix` instance can represent up to 8 chained devices, where each can be addressed directly by its zero-based index in the chain (0-7).


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


- **led(row, col, state)** Set on/off/color state for led at `row` and `col` (0-16, 0-16), of all devices.
- **led(device index, row, col, state)** Set on/off/color state for led at `row` and `col` (00-16), for a device at the specified `device index`.

  ```js
  // Shift Register device
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

  // Turn off the top-left led of all devices
  matrix.led(0, 0, 0);

  // I2C device (bicolor)
  var matrix = new five.Led.Matrix({ 
    controller: "HT16K33",
    isBicolor: true
  });
  // Turn top-left led of the specified device yellow
  matrix.led(0, 0, 0, LedControl.COLORS.YELLOW);
  ```


- **row(row, 0-255)** Set `row` (0-16) to 8-bit or 16-bit value (0-0xFFFF), of all devices.
- **row(device index, row, 0-255)** Set `row` (0-16) to 8-bit or 16-bit value (0-0xFFFF), for a device at the specified `device index`.

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

  // 8x16 I2C device
  var matrix = new five.Led.Matrix({ 
    controller: "HT16K33",
    dims: [8,16]
  });
  // Turn on entire first row of all devices (16-bit value)
  matrix.row(0,0xFFFF)
  ```


- **column(column, 0-255)** Set `column` (0-16) to 8-bit or 16-bit value (0-0xFFFF), of all devices.
- **column(device index, column, 0-255)** Set `column` (0-16) to 8-bit or 16-bit value (0-0xFFFF), for a device at the specified `device index`.

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
- An array (with 8 or 16 elements) of 8-bit or 16-bit values, eg. `[0x00, 0x04, 0x15, 0x0E, 0x15, 0x04, 0x00, 0x00]` (That array represents `*` character). The dimensions of the character array must match the dimensions of the matrix i.e. 8x8, 8x16 or 16x8
- An 8 or 16 element array of 8 or 16 character long strings, where each character represents the on/off/color state of the LED at that position in the matrix, eg.
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