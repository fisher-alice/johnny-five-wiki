The `Stepper` class constructs objects that represent a single stepper motor attached to the physical board. This class is new and should be considered unstable.

In order to use the `Stepper` class, your board must be flashed with `AdvancedFirmata`, which is available here: https://github.com/soundanalogous/AdvancedFirmata

Stepper motors generally require significantly involved hardware setup (that is, more then just plugging a single lead into a pin).

### Parameters

- **options** An object of property parameters.
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
      <td>pins</td>
      <td>Object *</td>
      <td>
        ```js
        Type: DRIVER
        { step, dir }
        ```

        ```js
        Type: TWO_WIRE
        { motor1, motor2 }
        ```

        ```js
        Type: FOUR_WIRE
        { motor1, motor2, motor3, motor4 }
        ```

      </td>
      <td>An object containing the named pin addresses for the 3 supported types</td>
      <td>yes *</td>
    </tr>
    <tr>
      <td>pins</td>
      <td>Array *</td>
      <td>
        ```js
        Type: DRIVER
        [ step, dir ]
        ```

        ```js
        Type: TWO_WIRE
        [ motor1, motor2 ]
        ```

        ```js
        Type: FOUR_WIRE
        [ motor1, motor2, motor3, motor4 ]
        ```
      </td>

      <td>An array containing the pin addresses for the 3 supported types</td>
      <td>yes *</td>
    </tr>


    <tr>
      <td>stepsPerRev</td>
      <td>Number</td>
      <td>#</td>
      <td>Steps per revolution. This will differ by motor, refer to motor specs for value</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>type</td>
      <td>constant</td>
      <td>DRIVER, TWO_WIRE, FOUR_WIRE</td>
      <td>five.Stepper.TYPE.DRIVER, five.Stepper.TYPE.TWO_WIRE, five.Stepper.TYPE.FOUR_WIRE</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>rpm</td>
      <td>Number</td>
      <td>Per device</td>
      <td>Revolutions per minute, used to calculate speed. Defaluts to 180</td>
      <td>no</td>
    </tr>
    <tr>
      <td>direction</td>
      <td>Number</td>
      <td>-1, 0, 1</td>
      <td>
        Counter-Clockwise: 0, Clockwise: 1, Default: -1

        - five.Stepper.DIRECTION.CW
        - five.Stepper.DIRECTION.CCW

      </td>
      <td>no</td>
    </tr>
  </tbody>
</table>
```js
// Create a stepper motor
// 
//   - Step on pin 11
//   - Direction on pin 12
//   - Steps per revolution: 200
//   - Uses a Driver board
//

var stepper = new five.Stepper({
  type: five.Stepper.TYPE.DRIVER,
  stepsPerRev: 200,
  pins: [ 11, 12 ]
});

// Is the same as...

var stepper = five.Stepper({
  type: five.Stepper.TYPE.DRIVER
  stepsPerRev: 200,
  pins: {
    step: 11
    dir: 12
  }
 });
```


* The **pins** property is required, but can be EITHER an object or an array.

### Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pins: Object containing the pin addresses for the Stepper
  rpm: Revolutions per minute, used to calculate speed. Defaluts to 180
  direction: 
  speed:
  accel:
  decel:
}
```



### Usage

```js
var stepper = five.Stepper({
  type: five.Stepper.TYPE.DRIVER
  stepsPerRev: number,
  pins: {
    step: number
    dir: number
  }
 });
 ```

```js
var stepper = five.Stepper({
  type: five.Stepper.TYPE.DRIVER
  stepsPerRev: number,
  pins: [ step, dir ]
}); 
 ```
 
 ```js
 var stepper = five.Stepper({
  type: five.Stepper.TYPE.TWO_WIRE
  stepsPerRev: number,
  pins: {
    motor1: number,
    motor2: number
  }
 });
 ```

```js
var stepper = five.Stepper({
  type: five.Stepper.TYPE.TWO_WIRE
  stepsPerRev: number,
  pins: [ motor1, motor2 ]
}); 
 ```
 
 ```js
 var stepper = five.Stepper({
  type: five.Stepper.TYPE.FOUR_WIRE
  stepsPerRev: number,
  pins: {
    motor1: number,
    motor2: number,
    motor3: number,
    motor4: number
  }
});
```

```js
var stepper = five.Stepper({
  type: five.Stepper.TYPE.FOUR_WIRE
  stepsPerRev: number,
  pins: [ motor1, motor2, motor3, motor4 ]
}); 
 ```
 
 
 ## API

- **step(stepsOrOpts, callback)** Move a stepper motor.
```js
// stepsOrOpts
{
  steps: number of steps to move
  direction: 1, 0 (CCW, CW)
  rpm: Revolutions per minute. Defaults to 180
  accel: Number of steps to accelerate
  decel: Number of steps to decelerate
}

//
//   - 10 full revolutions
//   - Clockwise
//   - Accelerate over the first 1600 steps
//   - Decelerate over the last 1600 steps
//

stepper.step({ steps: 2000, direction: 1, accel: 1600, decel: 1600 }, function() {
  console.log( "Done stepping!" );
});
```

- **rpm()** Get the rpm.
- **rpm(value)** Set the rpm.
```js
stepper.rpm(180).step(2000, function() {
  console.log( "Done stepping!" );
});
```

- **direction()** Get the direction.
- **direction(value)** Set the direction.
```js
stepper.direction(1).step(2000, function() {
  console.log( "Done stepping!" );
});

stepper.direction(0).step(2000, function() {
  console.log( "Done stepping!" );
});
```
Or...
```js
stepper.direction(five.Stepper.DIRECTION.CW).step(2000, function() {
  console.log( "Done stepping!" );
});

stepper.direction(five.Stepper.DIRECTION.CCW).step(2000, function() {
  console.log( "Done stepping!" );
});

```

- **accel()** Get the acceleration.
- **accel(value)** Set the acceleration.
```js
stepper.accel(1600).step(2000, function() {
  console.log( "Done stepping!" );
});
```

- **decel()** Get the deceleration.
- **decel(value)** Set the deceleration.
```js
stepper.decel(1600).step(2000, function() {
  console.log( "Done stepping!" );
});
```



- **cw()** Set the Stepper to move Clockwise.
```js
stepper.cw().step(2000);
```

- **ccw()** Set the Stepper to move Counter-Clockwise.
```js
stepper.ccw().step(2000);
```


## Static

#### Types
<table>
  <thead>
    <tr>
      <th>Type</th>
      <th>Value</th>
      <th>Constant</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>DRIVER</td>
      <td>1</td>
      <td>Stepper.TYPE.DRIVER</td>
    </tr>
    <tr>
      <td>DRIVER</td>
      <td>2</td>
      <td>Stepper.TYPE.TWO_WIRE</td>
    </tr>
    <tr>
      <td>DRIVER</td>
      <td>4</td>
      <td>Stepper.TYPE.FOUR_WIRE</td>
    </tr>
  </tbody>
</table>

#### Run States
<table>
  <thead>
    <tr>
      <th>State</th>
      <th>Value</th>
      <th>Constant</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>STOP</td>
      <td>0</td>
      <td>Stepper.RUNSTATE.STOP</td>
    </tr>
    <tr>
      <td>DRIVER</td>
      <td>1</td>
      <td>Stepper.RUNSTATE.ACCEL</td>
    </tr>
    <tr>
      <td>DRIVER</td>
      <td>2</td>
      <td>Stepper.RUNSTATE.DECEL</td>
    </tr>
    <tr>
      <td>DRIVER</td>
      <td>3</td>
      <td>Stepper.RUNSTATE.RUN</td>
    </tr>
  </tbody>
</table>

#### Directions

<table>
  <thead>
    <tr>
      <th>Direction</th>
      <th>Value</th>
      <th>Constant</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>CCW</td>
      <td>0</td>
      <td>Stepper.DIRECTION.CCW</td>
    </tr>
    <tr>
      <td>CW</td>
      <td>1</td>
      <td>Stepper.DIRECTION.CW</td>
    </tr>
  </tbody>
</table>
