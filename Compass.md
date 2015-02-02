The `Compass` class constructs an object that represents a single Compass or Magnetometer.

Supported Compass/Magnetometer:

- HMC6352
- HMC5883L

### Parameters

 * **options** An object of property parameters.
  <table>
    <thead>
      <tr>
        <th>Property Name</th>
        <th>Type</th>
        <th>Value(s)</th>
        <th>Description</th>
        <th>Required</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>controller</td>
        <td>String</td>
        <td>HMC6352, HMC5883L</td>
        <td>Defines the compass module device</td>
        <td>yes</td>
      </tr>
      <tr>
        <td>gauss</td>
        <td>Number</td>
        <td>cgs units</td>
        <td>Set the scale gauss for compass readings. Defaults to 1.3</td>
        <td>no</td>
      </tr>
    </tbody>
  </table>

### Shape

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

```js
var compass = new five.Compass({
  controller: "HMC6352"
});
```
![HMC6352](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/compass-hmc6352.png)


```js
var compass = new five.Compass({
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

### Events

* **change** The "change" event is emitted whenever the heading of the compass has changed from it's last position
* **data** The "data" event is fired as frequently as the user defined freq will allow in milliseconds.

## Examples

