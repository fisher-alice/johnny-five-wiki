The `Proximity` class constructs an object that represents a single Proximity sensor.


Supported Proximity sensors:

- Pulse/PWM
  - Ultrasonic (All use `controller: "HCSR04"`, Arduino boards require [PingFirmata](#pingfirmata) alternatively an HCSR04 backpack can be used to make the sensor I2C compatible.)
    - [SR04 or HCSR04](http://www.amazon.com/SainSmart-HC-SR04-Ranging-Detector-Distance/dp/B004U8TOE6) \*
    - [SRF05](http://www.robotshop.com/en/devantech-ultrasonic-range-finder-srf05.html) \*
    - [Parallax Ping](https://www.parallax.com/product/28015) \*
    - [SeeedStudio Ultrasonic Range](http://www.seeedstudio.com/depot/Ultra-Sonic-range-measurement-module-p-626.html)
    - [Grove - Ultrasonic Ranger](http://www.seeedstudio.com/depot/Grove-Ultrasonic-Ranger-p-960.html) \*
      - Hardware Constraints
        - Range:
          - Close: 0-2cm (will result in 2cm reading)
          - Long: 2–400cm
        - Resolution: ~0.3cm
    - [RadioShack 2760342](http://comingsoon.radioshack.com/radioshack-ultrasonic-range-sensor/2760342.html)
- Analog
  - Ultrasonic
    - [MB1000, LV-MaxSonar-EZ0](http://maxbotix.com/Ultrasonic_Sensors/MB1000.htm)
      - Hardware Constraints
        - Range:
          - Close: 0-15cm (will result in 15cm reading)
          - Long: 15-645cm (15-50cm may experience acoustic phase cancellation)
        - Resolution: 2.54cm
      - Likely supports the entire `LV-MaxSonar-EZ*` line, additional sensors to be confirmed.
    - [MB1010, LV-MaxSonar-EZ1](http://maxbotix.com/Ultrasonic_Sensors/MB1010.htm)
      - Hardware Constraints
        - Range:
          - Close: 0-15cm (will result in 15cm reading)
          - Long: 15-645cm
        - Resolution: 2.54cm
    - [MB1003, HRLV-MaxSonar-EZ0](http://maxbotix.com/Ultrasonic_Sensors/MB1003.htm)
      - Hardware Constraints
        - Range:
          - Close: 0-30cm (will result in 30cm reading)
          - Long: 30-500cm
        - Resolution: 1cm
    - [MB1230, XL-MaxSonar-EZ3](http://maxbotix.com/Ultrasonic_Sensors/MB1230.htm)
      - Hardware Constraints
        - Range:
          - Close: 0-20cm (will result in 20cm reading)
          - Long: 20-765cm
        - Resolution: 1cm
  - Infrared
    - [GP2Y0A21YK](https://www.sparkfun.com/products/242?utm_source=j5)
      - Hardware Constraints
        - Range:
          - Close: 0-10cm (will result in 10cm reading)
          - Long: 10-80cm
        - Resolution: 1cm
    - [GP2D120XJ00F](https://www.sparkfun.com/products/8959?utm_source=j5)
      - Hardware Constraints
        - Range:
          - Close: 0-4cm (will result in 4cm reading)
          - Long: 4-30cm
        - Resolution: 1cm
    - [GP2Y0A02YK0F](https://www.sparkfun.com/products/8958?utm_source=j5)
      - Hardware Constraints
        - Range:
          - Close: 0-15cm (will result in 15cm reading)
          - Long: 15-150cm
        - Resolution: 1cm
    - [GP2Y0A41SK0F](https://www.sparkfun.com/products/12728?utm_source=j5)
      - Hardware Constraints
        - Range:
          - Close: 0-4cm (will result in 4cm reading)
          - Long: 4-30cm
        - Resolution: 1cm
    - [GP2Y0A710K0F](https://www.adafruit.com/products/1568?utm_source=j5)
      - Hardware Constraints
        - Range:
          - Close: 0-50cm (will result in 50cm reading)
          - Long: 50-500cm
        - Resolution: 1cm
- I2C
  - Lidar
    - [LIDAR-Lite](https://www.sparkfun.com/products/13167?utm_source=j5)
      - Hardware Constraints
        - Range:
          - Long: 0-4000cm
        - Resolution: < 1cm
  - Ultrasonic
    - [SRF10](http://www.robotshop.com/en/devantech-srf10-ultrasonic-range-finder.html)
      - Hardware Constraints
        - Range:
          - Close: 0-6cm (will result in 6cm reading)
          - Long: 6-600cm
        - Resolution: < 1cm
    - [HCSR04 (or similar see above) using and I2C backpack]
      - Hardware Constraints
        - Additional hardware:
          - Requires addition of another board such as an Arduino Nano to provide the I2C support.
        - Range:
          - Close: 0-2cm (will result in 2cm reading)
          - Long: 2-300cm
        - Resolution: < 1cm


## Parameters

- **options** An object of property parameters.
  <span class="abbreviate-table">

  | Property | Type | Value/Description  | Default | Required |
  |----------|------|--------------------|---------|----------|
  | pin      | Number, String | Analog or Digital Pin. Use for non-I2C sensors | | Yes (non-I2C) |
  | controller | String | GP2Y0A21YK, GP2D120XJ00F, GP2Y0A02YK0F, GP2Y0A41SK0F, GP2Y0A710K0F, PING_PULSEIN \*, MB1000, MB1003, MB1230, LIDARLITE. See aliases || Yes |
  | freq     | Number         | Milliseconds. The frequency in ms of data events. | 25ms | No |
  </span>

  ##### Controller Alias Table

  | Controller | Alias |
  |------------|-------|
  | GP2Y0A21YK | 2Y0A21 |
  | GP2D120XJ00F | 2D120X |
  | GP2Y0A02YK0F | 2Y0A02 |
  | GP2Y0A41SK0F | OA41SK |
  | GP2Y0A21YK | 0A21 |
  | GP2Y0A02YK0F | 0A02 |
  | LV-MaxSonar-EZ | MB1000 |
  | HRLV-MaxSonar-EZ0 | MB1003 |
  | XL-MaxSonar-EZ3 | MB1230 |
  | HC-SR04 | PING_PULSE_IN |
  | HCSR04 | PING_PULSE_IN |
  | SRF05 | PING_PULSE_IN |
  | PARALLAXPING | PING_PULSE_IN |
  | SEEEDPING | PING_PULSE_IN |
  | GROVEPING | PING_PULSE_IN |
  | LIDAR-Lite | LIDARLITE |


## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `cm` | Distance to obstruction in centimeters. | Yes |
| `centimeters` | Distance to obstruction in centimeters. | Yes |
| `in` | Distance to obstruction in inches. | Yes |
| `inches` | Distance to obstruction in inches. | Yes |


## Component Initialization


#### Analog GP2Y0A21YK (and friends)

```js
/*
  Use with:

  - GP2Y0A21YK
  - GP2D120XJ00F
  - GP2Y0A02YK0F
  - GP2Y0A41SK0F
*/

new five.Proximity({
  controller: "GP2Y0A21YK",
  pin: "A0"
});
```

![Proximity](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/ir-proximity.png)

#### Analog GP2Y0A710K0F

```js
new five.Proximity({
  controller: "GP2Y0A710K0F",
  pin: "A0"
});
```

![Proximity](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/proximity-GP2Y0A710K0F.png)

#### HCSR04/Parallax Ping \*

```js
new five.Proximity({
  controller: "HCSR04",
  pin: 7
});
```

![Ping](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/ping.png)

> \* For Arduino boards, it is absolutely REQUIRED to flash your board with a special version of StandardFirmata (PingFirmata). Instructions are [here](#pingfirmata). 

Alternatively also consider an I2C Backpack which will keep your firmata the same and allow you to use this sensor on alternative boards such as a Raspberry Pi, BeagleBone or other non-Arduino board.

#### HCSR04 (and friends) I2C Backpack \*

```js
new five.Proximity({
  controller: "HCSR04I2CBACKPACK",
});
```

![Ping](https://raw.github.com/rwaldron/johnny-five/master/docs/breadboard/proximity-hcsr04-i2c.png)

The I2C backpack requires using an Arduino (eg Nano or pro-mini) to provide the I2C capability. This removes the blocking behaviour of the PingFirmata firmware and makes this sensor more portable across alternative boards (anything that has I2C support in its IO plugin). See [NodeBots Interchange HC-SR04 module](https://github.com/ajfisher/nodebots-hcsr04) for more details.

##### MB1000 LV-MaxSonar-EZ0

```js
new five.Proximity({
  controller: "MB1000",
  pin: "A0"
});
```

##### MB1003 HRLV-MaxSonar-EZ0

```js
new five.Proximity({
  controller: "MB1003",
  pin: "A0"
});
```

##### MB1230 XL-MaxSonar-EZ3

```js
new five.Proximity({
  controller: "MB1230",
  pin: "A0"
});
```

![Sonar](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/sonar.png)

#### LIDAR-Lite


```js
new five.Proximity({
  controller: "LIDARLITE"
});
```

![LIDAR](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/proximity-lidarlite.png)


#### SRF10


```js
new five.Proximity({
  controller: "SRF10"
});
```

![SRF10](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/proximity-srf10.png)



## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var proximity = new five.Proximity({
    controller: ...,
    pin: "A0"
  });

  proximity.on("data", function() {
    console.log("inches: ", this.inches);
    console.log("cm: ", this.cm);
  });
});

```

## API

- **within(range, unit, handler)** When value is within the provided range for the specified unit (`cm`, `centimeters`, `in` or `inches`), execute callback.

  ```js

  var proximity = new five.Proximity({
    controller: "GP2Y0A21YK",
    pin: "A0"
  });

  proximity.within([ 100, 200 ], "cm", function() {
    
    // This is called when the proximity sensor's cm value falls within 100-200

  });

  ```

## Events

- **data** The "data" event is fired as frequently as the user defined freq will allow in milliseconds.

- **change** The "change" event is fired when the distance to obstruction reading changes within the observable range of the sensor.


## PingFirmata

**DOES NOT APPLY TO NON-ARDUINO PLATFORMS**

For use with:

- [SR04 or HCSR04](http://www.amazon.com/SainSmart-HC-SR04-Ranging-Detector-Distance/dp/B004U8TOE6)
- [SRF05](http://www.robotshop.com/en/devantech-ultrasonic-range-finder-srf05.html)
- [Parallax Ping](https://www.parallax.com/product/28015)
- [SeeedStudio Ultrasonic Range](http://www.seeedstudio.com/depot/Ultra-Sonic-range-measurement-module-p-626.html)
- [Grove - Ultrasonic Ranger](http://www.seeedstudio.com/depot/Grove-Ultrasonic-Ranger-p-960.html)


The above listed devices require a special version of Firmata (shown below) to be loaded onto the Arduino in order to function properly. **This is only necessary for the components listed in this section!**

The most straightforward method to enabling this module is to use the [Ping I2C Backpack](https://github.com/ajfisher/nodebots-hcsr04). If you still want / need to use a custom firmata then the easiest way to get the firmata onto your board is to use [interchange](https://github.com/ajfisher/nodebots-interchange). Follow the directions below:

```
npm install nodebots-interchange
```

With your arduino plugged in then issue the following instruction, where <port> is the serial port your arduino is plugged into (COM3, /dev/ttyUSB0 etc).

```
interchange install hc-sr04 -a uno -p <port> --firmata
```

The latest firmware will be downloaded and installed upon your board.

The final alternative is to copy and paste [the source](https://gist.githubusercontent.com/rwaldron/0519fcd5c48bfe43b827/raw/f17fb09b92ed04722953823d9416649ff380c35b/PingFirmata.ino) into the Arduino IDE and click the Upload button.