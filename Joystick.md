The `Joystick` class constructs objects that represent a single Joystick sensor attached to the physical board.

This list will continue to be updated as more Joystick devices are confirmed.

## Parameters

- **General Options**
  <table>
    <thead>
      <tr>
        <th>Property Name</th>
        <th>Type</th>
        <th>Value(s)</th>
        <th>Description</th>
        <th>Default</th>
        <th>Required</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>pins</td>
        <td>Array of Pins</td>
        <td>["A*"]</td>
        <td>The String analog pins that X and Y</td>
        <td>none</td>
        <td>yes</td>
      </tr>
    </tbody>
  </table>

## Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pins: The pins defined for X and Y.
  x: -1, 1. READONLY
  y: -1, 1. READONLY
}
```

## Component Initialization


```js
var joystick = new five.Joystick({
  // [ x, y ]
  pins: ["A0", "A1"]
});
```

![Joystick](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/joystick.png)


## Events

- **axismove** Is an alias for "change".

- **change** The "change" event is emitted whenever the value of the gyro changes more then the threshold value allows.

- **data** The "data" event is fired as frequently as the user defined `freq` will allow in milliseconds. ("data" replaced the "read" event)
