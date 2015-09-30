The `Led.Digits` class constructs an object that may represent one or more (chained) 8 Digit, 7-Segment \* LED Digit display (MAX7219/MAX7221) devices attached to the physical board. Up to 8 devices can be controlled with one instance, giving you 64 displayable digits.

\* Support for varying segment devices is in progress.

Known supported devices: 

- [Sunkee MAX7219 with 8-Digit LED Display Module](http://www.amazon.com/Sunkee-MAX7219-8-Digit-Display-Module/dp/B00D3J04JC/)
- [5V MAX7219 8-Digit Red LED Display Module 7 Segment Digital Tube For Arduino MCU](http://www.amazon.com/MAX7219-8-Digit-Display-Segment-Digital/dp/B00IHQ7STK)
- [4 Digit, 7 Segment HT16K33](https://www.adafruit.com/search?q=7-segment&b=1)


(This list only represents devices that I've personally confirmed, please add devices as needed.)


## Parameters

- **options** An object of property parameters.

  | Property | Type   | Value/Description                 |Default| Required |
  |----------|--------|-----------------------------------|---------------------------------|----------|
  | pins     | Object | `{ data, clock, cs }`. Object of digital pin names.                                             || yes      |
  | pins          | Array | `[ data, clock, cs ]`. Array of digital pin names.                                              || yes      |
  | devices       | Number | `1-8`. For single device cases, this can be omitted. | `1` | no       |
  | controller    | string | HT16K33. The Name of the controller to use |  | no       |



## Shape

```js
{ 
  isMatrix: false. READONLY
  devices: ...Number of devices controlled. READONLY
  digitOrder: -1|1. -1 for RTL, 1 for LTR. 
}
```

## Component Initialization

#### Shift Register Device

```js
new five.Led.Digits({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});
```

![Led.Digits](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-digits-clock.png)


#### HT16K33

```js
new five.Led.Digits({
  controller: "HT16K33"
});
```

![Led.Digits](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-digits-clock-HT16K33.png)


## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var digits = new five.Led.Digits({
    pins: {
      data: 2,
      clock: 3,
      cs: 4
    }
  });

  digits.print("----");
});
```


## API

NOTE: An `Led.Digits` instance can represent up to 8 chained devices (1 device can support up to 8 seven-segment digits), where each can be addressed directly by its device index in the chain (1-8).

> NOTE: When the device is turned off (with the `off` method), data is retained and will be displayed on the device when turned on (with the `on` method). This is useful when powering the devices from a battery, by providing a power saving mechanism.

- **on()** Turn on all digits devices.
- **on(device index)** Turn on digits device at specified device index.

  ```js
  // Turn on a specific device by device index.
  digits.on(0);

  // Turn on all devices
  digits.on();
  ```

- **off()** Turn off all digits devices.
- **off(device index)** Turn off digits device at specified device index.

  ```js
  // Turn off a specific device by device index.
  digits.off(0);

  // Turn off all devices
  digits.off();

  // Send data to the the digits
  // ...Turn on the device to see the data displayed
  ```


- **clear()** Shut off all LEDs, for all devices.
- **clear(device index)** Shut off all LEDs, for a device at specified device index. (Note: `clear()` does not shut off the device.)

  ```js
  // Clear the entire display of a specified device.
  digits.clear(0);

  // Clear the entire display for all devices
  digits.clear();
  ```


- **brightness(0-100)** Set the brightness from 0-100%, of all devices.
- **brightness(device index, 0-100)** Set the brightness from 0-100%, for a device at specified `device index`.


  ```js
  // Set the brightness of a specified device.
  digits.brightness(1, 100);

  // Set the brightness for all devices
  digits.brightness(100);
  ```


- **draw(position, character)** Draw a `character` to a digit `position` (0-7) on all devices.
- **draw(device index, position, character)** Draw a `character` to a digit `position` (0-7) on a device at the specified `device index`.
  - **This is probably not the API you want to use. See `print(string)` below**

  - Valid `character` values: 

    - A single number
    - A single number or character string
    - A string containing a number or letter character followed by a decimal point character.
    ```js
    // Draw a "1" to device 1, digit position 0
    digits.draw(1, 0, "1");

    // Draw a "1" to all devices at digit position 0
    digits.draw(0, "1");
    ```
    ![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/4-digit-7-segment-draw-001.png)

    ```js
    // Draw an "A" to device 1, digit position 0
    digits.draw(1, 0, "A");

    // Draw an "A" to all devices at digit position 0
    digits.draw(0, "A");
    ```
    ![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/4-digit-7-segment-draw-002.png)

    ```js
    // Draw a "1." to device 1, digit position 0
    digits.draw(1, 0, "1.");

    // Draw a "1." to all devices at digit position 0
    digits.draw(1, "1.");
    ```

    ![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/4-digit-7-segment-draw-003.png)

- **print(string)** Display a string across available digits. 
  - If a colon `:` is present on the display unit, then it is treated as a single character. That means that a 4 digit display that includes a colon will be treated as a 5 character display. 
    ```js
    digits.print("12:00"); // display the colon
    ```
    ![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/4-digit-7-segment-print-001.png)

    ```js
    digits.print("12 00"); // do not display the colon
    ```
    ![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/4-digit-7-segment-print-002.png)

    ```js
    digits.print("HOLA");
    ```
    ![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/4-digit-7-segment-with-colon-001.png)

  - Periods following a digit are **not** considered a character, they are interpreted as being part of the preceding character. 
    ```js
    digits.print("0.0.0.0."); 
    ```
    ![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/4-digit-7-segment-print-003.png)

    ```js
    digits.print("0.0.:0.0."); 
    ```
    ![](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/4-digit-7-segment-print-004.png)

## Predefined Characters

`Led.Digits.DIGIT_CHARS`

```
0 1 2 3 4 5 6 7 8 9   
! .
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
a b c d e f g h i j k l m n o p q r s t u v w x y z
```


## Events

`Led.Digits` objects are output only and therefore do not emit any events.
