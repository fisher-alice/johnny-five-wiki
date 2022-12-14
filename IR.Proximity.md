The `IR.Proximity` class constructs an object that represents a single Infrared Proximity sensor.

- Proximity
    - [GP2Y0A21YK, Analog](https://www.sparkfun.com/products/242?utm_source=j5)
    - [GP2D120XJ00F, Analog](https://www.sparkfun.com/products/8959?utm_source=j5)
    - [GP2Y0A02YK0F, Analog](https://www.sparkfun.com/products/8958?utm_source=j5)
    - [GP2Y0A41SK0F, Analog](https://www.sparkfun.com/products/12728?utm_source=j5)


**NOTE:** The `IR.Proximity` is deprecated. Please use the [Proximity](https://github.com/rwaldron/johnny-five/wiki/Proximity) class.

## Parameters

- **options** An object of property parameters.

  | Property | Type           | Value/Description                                                                    | Default | Required |
  |---------------|----------------|------------------------------|--------------------------------------------------------------------------------|----------|
  | pin           | Number, String | Analog Pin. The Number or String address of the pin the sensor is attached to, ie. 9, “D7” | | yes      |
  | controller    | String         | "GP2Y0A21YK", "GP2D120XJ00F", "GP2Y0A02YK0F", "GP2Y0A41SK0F". The name of the controller to use                                              || yes      |


## Shape

```
{
  id: A user definable id value. Defaults to a generated uid
  pin: The pin address that the Sensor is attached to
  cm: Distance to obstruction in centimeters. READONLY
  in: Distance to obstruction in inches. READONLY
}

```


## Component Initialization


#### Analog

```js
new five.IR.Proximity({
  controller: "GP2Y0A21YK",
  pin: "A0"
});
```

![IR.Proximity](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/ir-proximity.png)

## Usage
```js
var five = require("johnny-five");
var board = new five.Board();
var controller = process.argv[2] || "GP2Y0A02YK0F";

board.on("ready", function() {
  var proximity = new five.IR.Proximity({
    controller: controller,
    pin: "A0"
  });

  proximity.on("data", function() {
    console.log("inches: ", this.inches);
    console.log("cm: ", this.cm);
  });
});

```

## Events

- **data** The "data" event is fired as frequently as the user defined freq will allow in milliseconds. ("data" replaced the "read" event)

- **change** The "change" event is fired when the distance to obstruction reading changes within the observable range of the IR.Proximity sensor.
