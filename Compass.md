![](http://i.gyazo.com/5808690439afa3eb111d82234ed97f76.png)

The `Compass` class constructs an object that represents a single Compass or Magnetometer.

Supported Compass/Magnetometer:

- HMC6352
- HMC5883L

## Parameters

 * **options** An object of property parameters.

  | Property | Type   | Value/Description                                               | Default | Required |
  |---------------|--------|-------------------|-----------------------------------------------------------|----------|
  | controller    | String | "HMC6352", "HMC5883L". Defines the compass module device  | | yes      |
  | gauss         | Number | cgs units. Set the scale gauss for compass readings. | 1.3 | no       |

## Shape

```js
{
  heading: The current heading in degrees, 0-360°
  bearing: {
    // Example
    point: "North",
    abbr: "N",
    low: 354.38,
    mid: 360,
    high: 360
  }
}
```

## Component Initialization

#### HMC6352

```js
new five.Compass({
  controller: "HMC6352"
});
```
![HMC6352](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/compass-hmc6352.png)

#### HMC5883L

```js
new five.Compass({
  controller: "HMC5883L"
});
```
![HMC5883L](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/compass-hmc5883l.png)


## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var compass = new five.Compass({
    controller: "HMC6352"
  });

  compass.on("headingchange", function() {
    console.log("headingchange");
    console.log("  heading : ", Math.floor(this.heading));
    console.log("  bearing : ", this.bearing.name);
    console.log("--------------------------------------");
  });

  compass.on("data", function() {
    console.log("  heading : ", Math.floor(this.heading));
    console.log("  bearing : ", this.bearing.name);
    console.log("--------------------------------------");
  });
});
```

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var compass = new five.Compass({
    controller: "HMC5883L"
  });

  compass.on("headingchange", function() {
    console.log("headingchange");
    console.log("  heading : ", Math.floor(this.heading));
    console.log("  bearing : ", this.bearing.name);
    console.log("--------------------------------------");
  });

  compass.on("data", function() {
    console.log("  heading : ", Math.floor(this.heading));
    console.log("  bearing : ", this.bearing.name);
    console.log("--------------------------------------");
  });
});
```

## API

There are no special API functions for this class.


## Events

- **change** The "change" event is emitted whenever the heading of the compass has changed from it's last position
- **data** The "data" event is fired as frequently as the user defined freq will allow in milliseconds.

<!--remove-start-->

## Examples

<!--remove-end-->