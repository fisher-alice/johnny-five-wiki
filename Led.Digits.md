The `Led.Digits` class constructs an object that may represent one or more (chained) 8 Digit, 7-Segment \* LED Digit display (MAX7219/MAX7221) devices attached to the physical board. Up to 8 devices can be controlled with one instance, giving you 64 displayable digits.

\* Support for varying segment devices is in progress.

![](http://ecx.images-amazon.com/images/I/61LJiwBPXHL._SL1500_.jpg)


Known supported devices: 

- [MAX7219 Red Module 8-Digit 7 Segment Digital LED](http://www.newegg.com/Product/Product.aspx?Item=9SIA2C516T9999)
- [Sunkee MAX7219 with 8-Digit LED Display Module](http://www.amazon.com/Sunkee-MAX7219-8-Digit-Display-Module/dp/B00D3J04JC/)
- [5V MAX7219 8-Digit Red LED Display Module 7 Segment Digital Tube For Arduino MCU](http://www.amazon.com/MAX7219-8-Digit-Display-Segment-Digital/dp/B00IHQ7STK)

(This list only represents devices that I've personally confirmed, please add devices as needed.)


### Parameters


- **options** An object of property parameters.

  | Property | Type   | Value(s)                          | Properties                                                   | Required |
  |---------------|--------|-----------------------------------|--------------------------------------------------------------|----------|
  | pins          | Object | A valid pins object or pins array | data, clock, cs                                              | yes      |
  | devices       | Number | 1-8                               | For single device cases, this can be omitted. Defaults to 1. | no       |



## Shape

```js
{ 
  isMatrix: false. READONLY
  devices: ...Number of devices controlled. READONLY
}
```

## Component Initialization

```js
var digits = new five.Led.Digits({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});
```

![Led.Digits](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/led-digits-clock.png)




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

  digits.print("Hello!");
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
- **clear(device index)** Shut off all LEDs, for a device at specified device index.

Note: `clear()` does not shut off the device.

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

Valid `character` values: 

- A single number
- A single number or character string
- A string containing a number or letter character followed by a decimal point character.


  ```js
  // Draw a "1" to device 1, digit position 0
  digits.draw(1, 0, "1");

  // Draw a "1" to all devices at digit position 0
  digits.draw(0, "1");

  // Draw an "A" to device 1, digit position 0
  digits.draw(1, 0, "A");

  // Draw an "A" to all devices at digit position 0
  digits.draw(0, "A");

  // Draw a "1." to device 1, digit position 0
  digits.draw(1, 0, "1.");

  // Draw a "1." to all devices at digit position 0
  digits.draw(1, "1.");


  // Write a message to a single device, using the default device:

  var message = "Hello";

  message.split("").forEach(function(char, i) {
    digits.draw(i, char);
  });

  // NOTE: This last example will hopefully be replaced by an easier 
  // API. Overtime I hope that use patterns will emerge that will 
  // help guide the design of an easier "print to device" API.
  ```


## Predefined Characters

`Led.Digits.CHARS`

```
0 1 2 3 4 5 6 7 8 9   
! .
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
a b c d e f g h i j k l m n o p q r s t u v w x y z
```


## Events

`Led.Digits` objects are output only and therefore do not emit any events.
