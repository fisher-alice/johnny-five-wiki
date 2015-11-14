The `Stepper` class constructs objects that represent a single stepper motor attached to the physical board. This class is new and should be considered unstable.

In order to use the `Stepper` class, your board must be flashed with `AdvancedFirmata`, which is available here: https://github.com/soundanalogous/AdvancedFirmata

Stepper motors generally require significantly involved hardware setup (that is, more than just plugging a single lead into a pin).

## Parameters

- **options** An object of property parameters.
  <span class="abbreviate-table">

  | Property    | Type      | Value/Description                                                                                              | Default | Required        |
  |-------------|-----------|-------------------------------|----------------------------------------------------------------------------------------------------------|-----------------|
  | pins        | Object \* | See Table. An object containing the named pin addresses for the 3 supported types                                   | | yes \*          |
  | pins        | Array \*  | See Table. An array containing the pin addresses for the 3 supported types                                          | | yes<sup>1</sup> |
  | stepsPerRev | Number    | Steps per revolution. This will differ by motor, refer to motor specs for value                          | | yes             |
  | type        | constant  | `five.Stepper.TYPE.DRIVER`, `five.Stepper.TYPE.TWO_WIRE`, `five.Stepper.TYPE.FOUR_WIRE`                       | | yes             |
  | rpm         | Number    | Revolutions per minute, used to calculate speed. | 180                                         | no              |
  | direction   | constant    | `five.Stepper.DIRECTION.CW`, `five.Stepper.DIRECTION.CCW` | | no<sup>2</sup>  |
  </span>

### Pins 

| Config | As Object  | As Array |
|--------|------------|----------|
| DRIVER | `{ step, dir }` | `[ step, dir ]` |
| TWO\_WIRE | `{ m1, m2 }` | `[ m1, m2 ]` |
| FOUR\_WIRE | `{ m1, m2, m3, m4 }` | `[ m1, m2, m3, m4 ]` |



1. The **pins** property is required, but can be EITHER an object or an array.  
2. The **direction** property is not required, but if it is undefined then Stepper.step() will do nothing.

## Shape

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


## Component Initialization

#### Driver

```js
// Create a stepper motor
// 
//   - Step on pin 11
//   - Direction on pin 12
//   - Steps per revolution: 200
//   - Uses a Driver board
//

new five.Stepper({
  type: five.Stepper.TYPE.DRIVER,
  stepsPerRev: 200,
  pins: [ 11, 12 ]
});

// Is the same as...

five.Stepper({
  type: five.Stepper.TYPE.DRIVER
  stepsPerRev: 200,
  pins: {
    step: 11
    dir: 12
  }
 });
```

```js
five.Stepper({
  type: five.Stepper.TYPE.DRIVER
  stepsPerRev: number,
  pins: {
    step: number
    dir: number
  }
 });
```

```js
five.Stepper({
  type: five.Stepper.TYPE.DRIVER
  stepsPerRev: number,
  pins: [ step, dir ]
}); 
```

![Stepper.Stepper.TYPE.DRIVER](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/stepper-easy-driver.png)

#### TWO WIRE

```js
five.Stepper({
  type: five.Stepper.TYPE.TWO_WIRE
  stepsPerRev: number,
  pins: {
    motor1: number,
    motor2: number
  }
 });
```

```js
five.Stepper({
  type: five.Stepper.TYPE.TWO_WIRE
  stepsPerRev: number,
  pins: [ motor1, motor2 ]
}); 
```
 

#### FOUR WIRE

```js
five.Stepper({
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
five.Stepper({
  type: five.Stepper.TYPE.FOUR_WIRE
  stepsPerRev: number,
  pins: [ motor1, motor2, motor3, motor4 ]
}); 
```

![Stepper.Stepper.TYPE.DRIVER](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/stepper-driver.png)

## Usage

```js
var board = new five.Board();

board.on("ready", function() {
  var k = 0;
  var stepper = new five.Stepper({
    type: five.Stepper.TYPE.DRIVER,
    stepsPerRev: 200,
    pins: [11, 12]
  });

  stepper.rpm(180).ccw().step(2000, function() {
    console.log("done");
  });
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
    console.log("Done stepping!");
  });
  ```

- **rpm()** Get the rpm.
- **rpm(value)** Set the rpm.
  ```js
  stepper.rpm(180).step(2000, function() {
    console.log("Done stepping!");
  });
  ```

- **speed()** Get the speed.
- **speed(value)** Set the speed in `0.01 * rad/s`.
  ```js
  // 180 rpm
  stepper.speed(0.18850).step(2000, function() {
    console.log("Done stepping!");
  });
  ```

- **direction()** Get the direction.
- **direction(value)** Set the direction.
  ```js
  stepper.direction(1).step(2000, function() {
    console.log("Done stepping!");
  });

  stepper.direction(0).step(2000, function() {
    console.log("Done stepping!");
  });
  ```
  Or...
  ```js
  stepper.direction(five.Stepper.DIRECTION.CW).step(2000, function() {
    console.log("Done stepping!");
  });

  stepper.direction(five.Stepper.DIRECTION.CCW).step(2000, function() {
    console.log("Done stepping!");
  });
  ```

- **accel()** Get the acceleration.
- **accel(value)** Set the acceleration.
  ```js
  stepper.accel(1600).step(2000, function() {
    console.log("Done stepping!");
  });
  ```

- **decel()** Get the deceleration.
- **decel(value)** Set the deceleration.
  ```js
  stepper.decel(1600).step(2000, function() {
    console.log("Done stepping!");
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

### Types

| Type   | Value | Constant                |
|--------|-------|-------------------------|
| DRIVER | 1     | Stepper.TYPE.DRIVER     |
| DRIVER | 2     | Stepper.TYPE.TWO\_WIRE  |
| DRIVER | 4     | Stepper.TYPE.FOUR\_WIRE |

### Run States

| State  | Value | Constant               |
|--------|-------|------------------------|
| STOP   | 0     | Stepper.RUNSTATE.STOP  |
| DRIVE  | 1     | Stepper.RUNSTATE.ACCEL |
| DRIVE  | 2     | Stepper.RUNSTATE.DECEL |
| DRIVE  | 3     | Stepper.RUNSTATE.RUN   |

### Directions

| Direction | Value | Constant              |
|-----------|-------|-----------------------|
| CCW       | 0     | Stepper.DIRECTION.CCW |
| CW        | 1     | Stepper.DIRECTION.CW  |
