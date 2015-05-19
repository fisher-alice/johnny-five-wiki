![](http://i.gyazo.com/6bfe16c39837476090a147ae69f6d48b.png)

The `Joystick` class constructs objects that represent a single Joystick sensor attached to the physical board.

This list will continue to be updated as more Joystick devices are confirmed.

## Parameters

- **General Options**

  | Property | Type          | Value/Description                         | Default | Required |
  |---------------|---------------|----------|-------------------------------------|---------|
  | pins          | Array of Pins | `["A*", ...]`. Analog pins connected to X and Y |    | yes      |
  | invert        | Boolean | `true, false`. Invert both axes | `false`   | no      |
  | invertX        | Boolean | `true, false`. Invert the X axis | `false`   | no      |
  | invertY        | Boolean | `true, false`. Invert the Y axis | `false`   | no      |

## Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  x: -1, 1. READONLY
  y: -1, 1. READONLY
}
```

## Component Initialization

#### Analog

```js
new five.Joystick({
  // [ x, y ]
  pins: ["A0", "A1"]
});
```

![Joystick](https://github.com/rwaldron/johnny-five/raw/master/docs/images/joystick.jpg)

![Adafruit Joystick](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/joystick-adafruit.png)

![SparkFun Joystick](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/joystick-sparkfun.png)


#### Sparkfun Shield 

```js
new five.Joystick({
  pins: ["A0", "A1"], 
  invertY: true
});
```

![SparkFun JoyStick Shield](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/joystick-shield.png)

#### Esplora

```js
new five.Joystick({
  controller: "ESPLORA"
});
```
![Esplora](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/esplora.png)

#### Axis Inversion

##### Default 

```js
var joy = new Joystick({ pins: ["A0", "A1"], invertY: true });
```

Produces the following default axes:

```
    -1
-1   *   1
     1      
```

##### Invert Y 

```js
var joy = new Joystick({ pins: ["A0", "A1"], invertY: true });
```

Produces an inverted Y axis, with a default X axis: 

```
     1
-1   *   1
    -1      
```

##### Invert X

```js
var joy = new Joystick({ pins: ["A0", "A1"], invertX: true });
```

Produces an inverted X axis, with a default Y axis: 

```
   -1
1   *  -1
    1      
```

##### Invert Both

```js
var joy = new Joystick({ pins: ["A0", "A1"], invert: true });
```

Produces an both inverted X and Y axes: 

```
    1
1   *  -1
   -1      
```



## API

There are no special API functions for this class.

## Events

- **change** The "change" event is emitted whenever the value of the gyro changes more then the threshold value allows.

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the "read" event)
