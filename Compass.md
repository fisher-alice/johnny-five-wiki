The `Compass` class constructs an object that represents a single Compass sensor

### Paramaters

 * **options** An object of property parameters.
### Shape

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
      <td>"HMC6352" or "HMC5883L"</td>
      <td>Defines the compass module device</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>freq</td>
      <td>Number</td>
      <td>Milliseconds</td>
      <td>The frequency in ms of data events. Defaults to 500ms</td>
      <td>no</td>
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

```js
  // Create a compass sensor
  var compass = new five.Compass({
    controller: "HMC6352",
    freq: 100,
    gauss: 1.3
  });
```


### Shape

```js
{
  freq: The frequency of read events.
}
```

## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var compass = new five.Compass({
    controller: "HMC6352"
  });

  compass.on("headingchange", function() {
    console.log("heading", Math.floor(this.heading));
    console.log("bearing", this.bearing);
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
    console.log("heading", Math.floor(this.heading));
    console.log("bearing", this.bearing);
  });
});
```
## API

* **heading** Gets the compass heading reading
* **bearing** Gets the compass bearing object

### Events
* **headingchange** The "headingchange" event is emitted whenever the heading of the compass has changed from it's last position
* **data** The "data" event is fired as frequently as the user defined freq will allow in milliseconds. ("data" replaced the "read" event)


## Examples

