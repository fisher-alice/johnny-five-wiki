The `GPS` class constructs objects that represent a single GPS (Global Positioning System) receiver attached to the physical board. The receiver can be used to find current latitude, longitude, altitude, course and ground speed. GPS receivers get their signals from an array of 32 satellites orbiting the Earth at an altitude of 12,600 miles. In other words, you can use Johnny-Five to talk to things in outer space (Is that cool or what?)

Supported chipsets, modules and breakouts:

- [MediaTek MT3339](http://www.mediatek.com/en/products/connectivity/gps/mt3339)
  - [66-Channel LS20031 GPS Receiver Module](https://www.pololu.com/product/2138)
  - [G-Top LadyBird 1 (PA6H)](http://www.gtop-tech.com/en/product/LadyBird-1-PA6H/MT3339_GPS_Module_04.html)
  - [Adafruit Ultimate GPS Breakout](https://www.adafruit.com/products/746)
- [U-blox NEO-6M](https://www.u-blox.com/en/product/neo-6-series)
 - [Crius Neo-6 v3.1](http://www.ebay.com/itm/like/281357870575?ul_noapp=true&chn=ps&lpid=82)

Any GPS Module that by default transmits NMEA sentences over serial at 9600bps should "just work". If your device automatically transmits NMEA sentences at a different baud rate, you can set a custom baud rate in the configuration options.

The universe of breakouts, modules, receivers and chip sets can be confusing. Just because you don't see your device listed here doesn't mean it won't work. If you have a device that is not explicitly listed here, open an issue with links to as much documentation you can find on your device and we will take a look. If you have a device not explicitly listed here and it works, please update our list above and add and example to the eg folder.

This list will continue to be updated as more component support is implemented and tested.

## Parameters

- **General Options**


    | Property | Type   | Value/Description                              | Default   | Required |
    |----------|--------|------------------------------------------------|-----------|----------|
    | port | string | The microcontroller's serial port to use. See more below | io.SERIAL_PORT_IDs.DEFAULT | no |
    | pins | array | { tx: ?, rx: ?[, onOff: ?]} | none | no |
    | breakout | string | The breakout or shield being used | none | no |
    | receiver | string | The receiver module being used | none | no |
    | chip | string | The chipset being used on the receiver module | none | no |
    | fixed | integer | The number of digits after the decimal | 6 | no |
    | baud | integer | The baud rate for communication over serial | 9600 | no |
    | frequency | integer | The frequency of updates in Hz | 1 | no |


    Breakouts

    | Breakout | Use With... |
    |----------|---------------------------------------------|
    | ADAFRUIT_ULTIMATE_GPS | Adafruit Ultimate GPS Breakout |






## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| latitude | Current latitude | Yes |
| longitude | Current longitude | Yes |
| altitude | Current altitude | Yes |
| sat | Satellite operation details {pdop, hdop, vdop} | Yes |
| course | Current course | Yes |
| speed | Current ground speed | Yes |
| time | Time of last fix | Yes |

## Component Initialization

#### Default

```js
// U-Blox NEO-6M attached to Arduino UNO on Software Serial 0, pins 10 and 11
new five.GPS([11, 10]);  // [RX, TX]
```

#### Adafruit Ultimate GPS Breakout

```js
// Adafruit Ultimate GPS attached to Arduino UNO on Software Serial 0, pins 10 and 11
new five.GPS({
  pins: [11, 10], // [RX, TX]
  breakout: "ADAFRUIT_ULTIMATE_GPS"
});
```

![gps.png](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/gps-adafruit.png)

## Usage

- **GPS Module connected to Arduino UNO with default buadrate which is 9600**

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var gps = new five.GPS({
    pins: {rx: 11, tx: 10}
  });

  // If latitude, longitude change log it
  gps.on("change", function() {
    console.log("position");
    console.log("  latitude   : ", this.latitude);
    console.log("  longitude  : ", this.longitude);
    console.log("  altitude   : ", this.altitude);
    console.log("--------------------------------------");
  });
  // If speed, course change log it
  gps.on("navigation", function() {
    console.log("navigation");
    console.log("  speed   : ", this.speed);
    console.log("  course  : ", this.course);
    console.log("--------------------------------------");
  });
});
```

## More Customize

- **GPS Module connected to Arduino MEGA 2560's hardware serial with custom buadrate which is 4800**

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var gps = new five.GPS({
    port: this.io.SERIAL_PORT_IDs.HW_SERIAL3,
    baud: 4800
  });

  // If latitude, longitude change log it
  gps.on("change", function() {
    console.log("position");
    console.log("  latitude   : ", this.latitude);
    console.log("  longitude  : ", this.longitude);
    console.log("  altitude   : ", this.altitude);
    console.log("--------------------------------------");
  });
  // If speed, course change log it
  gps.on("navigation", function() {
    console.log("navigation");
    console.log("  speed   : ", this.speed);
    console.log("  course  : ", this.course);
    console.log("--------------------------------------");
  });
});
```

## API

- **sendCommand(commandString)** Send a command to the GPS chipset. The checksum and CR/LF will be automatically appended.

  ``` js
  var gps = new five.GPS([10,11]);

  // Warm reboot
  gps.sendCommand("$PMTK103");
  ```

- **restart(coldRestart)** Reboot the receiver. If coldRestart is true, all cached positioning data is flushed. If you do this, it could take up to 60 seconds to get a new fix.

  ``` js
  var gps = new five.GPS([10,11]);

  // Warm reboot
  gps.restart();
  ```

## Events

- **message** A valid message (NMEA Sentence) was received from the GPS.

- **operations** Satellite operating details have received.

- **acknowledge** An "acknowledge" message was received from the GPS chipset.

- **unknown** An valid but unrecognized message was received from the GPS chipset.

- **change** The position has changed.

- **navigation** The speed or direction has changed.

## Notes on Serial

GPS is the first class in Johnny-Five to leverage firmata's new support for serial. Other IO plugins are following suit and adding their own support for serial.

[Arduino Uno](https://www.arduino.cc/en/Main/ArduinoBoardUno) has a single UART. Assuming your Arduino is connected to a computer via USB, that UART is already in use. By default the GPS class will try and use software serial to connect but please note that software serial does not work reliably above 57600bps. Other boards such as [Arduino MEGA 2560](https://www.arduino.cc/en/Main/arduinoBoardMega2560) has 4 UARTs so hardware serial is the better option on those boards.

Arduino boards that use the SAM and SAMD architectures (i.e. the Due and Zero) do not support Software Serial at all.

<!--remove-start-->

## Examples

- [Default](https://github.com/rwaldron/johnny-five/blob/master/docs/gps.md)

<!--remove-end-->