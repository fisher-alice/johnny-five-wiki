The `Barometer` class constructs objects that represent a single Barometer sensor attached to the physical board.

Supported Barometers:

- MPL115A2 (I2C)
- BMP180 (I2C)

This list will continue to be updated as more component support is implemented.

## Parameters

- **General Options**

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | controller    | string | BMP180, MPL115A2. The Name of the controller to use |  | Yes       |
  | address    | number | Address for I2C device. |  By Device | No       |
  | freq | number | Milliseconds. The rate in ms of data events. | 25 | No |

- **MPL115A2 Options (`controller: "MPL115A2"`)** 

  | Property | Type   | Value/Description                                           | Default | Required |
  |---------------|--------|--------------|-------------------------------------------------------|---------|
  
- **BMP180 Options (`controller: "BMP180"`)** 

  | Property | Type             | Value/Description     | Default | Required |
  |---------------|-------|------------|--------------------------------------------|---------|
  

## Shape

```js
{ 
  id: A user definable id value. Defaults to a generated uid
  pressure: The current pressure value in kPa. READONLY
}
```

## Component Initialization 


#### MPL115A2
```js
new five.Barometer({
  controller: "MPL115A2"
});
```

![BMP180](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-mpl115a2.png)

#### ADXL345
```js
new five.Barometer({
  controller: "BMP180"
});
```

![ADXL345](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/multi-bmp180.png)


## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var barometer = new five.Barometer({
    controller: "MPL115A2"
  });

  barometer.on("data", function() {
    console.log("barometer");
    console.log("  pressure : ", this.pressure);
    console.log("--------------------------------------");
  });
});
```

## API

There are no `Barometer` specific methods.

## Events

- **change** The "change" event is emitted whenever the value of the barometric pressure sensor changes.

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds.
