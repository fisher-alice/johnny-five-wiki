The `Motor` class constructs objects that represent a single Motor. The motor may attach to the physical board or a motor controller. The controller may be a third party shield or custom built motor controller. This class works well with both **Directional** and **Non-Directional** motors. It also works well with 2-pin or 3-pin controllers.

### Parameters
 * **pin** A Number or String address for the Non-Directional Motor pin (PWM).
```js
var motor = new five.Motor(9);
```
 * **pins** An array of 2 or 3 Numbers or String addresses for the Bi-Directional Motor pins. 
```js
// Two elements passed [pwm, dir]
var motor = new five.Motor([3, 12]);
```
or
```js
// Three elements passed [pwm, dir, cdir]
var motor = new five.Motor([3, 12, 11]);
```

 * **options**
An object of property parameters.
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
      <td>Object</td>
      <td>A valid pins object or pins array</td>
      <td></td>
      <td>yes</td>
    </tr>
    <tr>
      <td>current</td>
      <td>Object</td>
      <td>A valid Sensor options object*</td>
      <td>
        
      </td>
      <td>no</td>
    </tr>

  </tbody>
</table>
*See [Sensor](https://github.com/rwldrn/johnny-five/wiki/Sensor) for valid options on the current object
```js
// Create a motor with...
//
//   - pwm (speed) on pin 3
//   - dir (direction) on pin 12
//   - and brake on pin 11
//
// ... with a current sensor...
//
//   - that gets the value from pin "A0"
//   - every 250ms
//   - and scales the raw data to value between 0 and 2000
//
var motor = new five.Motor({
  pins: {
    pwm: 3,
    dir: 12,
    brake: 11
  }, 
  current: {
    pin: "A0",
    freq: 250,
    range: [0, 2000]
  }
});
```

### Shape

```
{ 
  isOn: A boolean flag, true when motor is moving or braking, false when not READONLY
}
```

### Usage

Non-Directional Motor
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var motor = new five.Motor(5);

  // Start the motor at maximum speed, wait 2 seconds and stop.
  motor.start(255);

});
```

Directional Motor
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var motor = new five.Motor([3, 12]);

  // Reverse the motor at maximum speed
  motor.reverse(255);

});
```


Directional Motor with Brake
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var motor = new five.Motor({
    pins: {
      pwm: 3,
      dir: 12,
      brake: 9
    }
  });

  motor.on("forward", function(err, timestamp) {
    // demonstrate braking after 5 seconds
    board.wait(5000, function() {
      motor.brake();
    });
  });

  motor.on("brake", function(err, timestamp) {
    // Release the brake after .1 seconds
    board.wait(100, function() {
      motor.stop();
    });
  });

  // Start the motor at maximum speed
  motor.forward(255);

});
```


Directional Motor with Current Sensing
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var motor = new five.Motor({
      pins: [3, 12]
    },
    current: {
      pin: "A0",
      freq: 250,
      threshold: 10
    }
  });

  // Log current mA every 250ms if that value has changed by 10 or more since the last log
  motor.current.scale([0, 3030]).on("change", function() {
    console.log("Motor A: " + this.value.toFixed(2) + "mA");
  });

  // Start the motor at maximum speed
  motor.forward(255);

});
```


## API

- **forward(speed 0-255)** Set a motor moving forward
- **fwd(speed 0-255)** Alias to forward()
```js
var motor = new five.Motor([11, 12]);

// Forward at half speed
motor.forward(128);
```

- **reverse(speed 0-255)** Set a motor moving in reverse
- **rev(speed 0-255)** Alias to reverse()

```js
var motor = new five.Motor([11, 12]);

// Reverse at full speed
motor.reverse(255);
```

- **start([speed 0-255])** Set a motor moving in the current direction
```js
var motor = new five.Motor([11, 12]);

// Forward at half speed
motor.forward(128);

// Stop
motor.stop();

// Resume forward at half speed
motor.start();

// Continue forward at full speed
motor.start(255);

```

- **stop()** Let the motor coast to a stop
```js
var motor = new five.Motor([11, 12]);

// Forward at full speed
motor.forward(255);

// Roll to stop
motor.stop();
```

- **brake()** Force a motor to stop (as opposed to coasting). Please note that this only works on boards with a dedicated brake pin. Other boards and interfaces will simply coast.
```js
var motor = new five.Motor([11, 12]);

// Forward at full speed
motor.forward(255);

// Stop fast
motor.brake();

board.wait(100, function() {
  motor.stop();
});
```

- **release()** Release the brake and resume current speed and direction
```js
var motor = new five.Motor([11, 12]);

// Forward at full speed
motor.forward(255);

// Stop fast
motor.brake();

// Wait five seconds and release the brake
board.wait(5000, function() {
  motor.release();
});
```

## Examples
- [Motor](https://github.com/rwldrn/johnny-five/blob/master/docs/motor.md)
- [Directional Motor](https://github.com/rwldrn/johnny-five/blob/master/docs/motor-directional.md)
- [Motor 3-pin](https://github.com/rwldrn/johnny-five/blob/master/docs/motor-3-pin.md)
- [Motor with Brake](https://github.com/rwldrn/johnny-five/blob/master/docs/motor-brake.md)
- [Motor with Current Sensor](https://github.com/rwldrn/johnny-five/blob/master/docs/motor-current.md)


## Additional Notes

The PWM pins on an Arduino Uno only output about 40mA. That is barely enough to power a humble hobby motor. You are going to need a motor controller between your Arduino and your motor(s) to deliver power from an external source. Most motor controllers are based on the H-Bridge circuit which uses a set of four switches to direct the voltage being sent through each pole of the motor. Different shields handle different input voltages and output currents. Use the Motor Control Shield Survey below to find a shield that works for you.

### Forward and Reverse are Interchangeable

Keep in mind that "forward" and "reverse" are arbitrary labels. If your motor is turning in the wrong direction you can just switch the poles on the motor. Consider a robot with two motors connected directly to the drive wheels. For your bot to go forward, one should turn clockwise and the other should turn counter-clockwise. Switch the poles on one of those motors so that you can use forward() on both and have them work together.

### Differences Between 2 and 3 pin Directional Motor Controllers

Controllers that use 2 pins instead of 3 are essentially the same. Both arrangements use one PWN pin to control speed. The switches on the H-Bridge work in pairs. With 3-pin controllers you control the state of each pair. With 2-pin controllers the pairs are toggled for you based on the state of that one digital pin.

### Motor Control Shield Survey
This is by no means exhaustive

#### 2 Pin
<table>
  <thead>
    <tr>
      <th>Manufacturer</th>
      <th>Name</th>
      <th>Motor A pins</th>
      <th>Motor B pins</th>
      <th>Operating Voltage(1)</th>
      <th>Max Current per Channel</th>
      <th>Stackable(2)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="http://arduino.cc/en/Main/ArduinoMotorShieldR3">Arduino</a></td>
      <td>Motor Shield R3</td>
      <td>pwm:3,<br/>dir:12,<br/>[brake:9,]<br/>[current:A0]</td>
      <td>pwm:11,<br/>dir:13,<br/>[brake:8,]<br/>[current:A1]</td>
      <td>7-12V</td>
      <td>2A</td>
      <td>No</td>
    </tr>    
    <tr>
      <td><a href="http://www.dfrobot.com/index.php?route=product/product&product_id=59">DF Robot</a></td>
      <td>1A</td>
      <td>pwm:6,<br/>dir:7</td>
      <td>pwm:5,<br/>dir:4</td>
      <td>7 - 12V</td>
      <td>1A</td>
      <td>No</td>
    </tr>    
    <tr>
      <td><a href="http://www.dfrobot.com/index.php?route=product/product&product_id=69">DF Robot</a></td>
      <td>2A</td>
      <td>pwm:6,<br/>dir:7</td>
      <td>pwm:5,<br/>dir:4</td>
      <td>4.8 - 35V</td>
      <td>2A</td>
      <td>No</td>
    </tr>    
    <tr>
      <td><a href="http://www.nkcelectronics.com/freeduino-arduino-motor-control-shield-kit.html">NKC Electronics</a></td>
      <td>Motor Control Shield Kit</td>
      <td>pwm:9,<br/>dir:12</td>
      <td>pwm:10,<br/>dir:13</td>
      <td>6 - 15V shared</td>
      <td>1A</td>
      <td>No</td>
    </tr>    
    <tr>
      <td><a href="http://www.ruggedcircuits.com/motor-control/rugged-motor-driver">Rugged Circuits</a></td>
      <td>Rugged Motor Driver</td>
      <td>pwm:3,<br/>dir:12</td>
      <td>pwm:11,<br/>dir:13</td>
      <td>8-30V</td>
      <td>2.8A</td>
      <td>Yes</td>
    </tr>    
    <tr>
      <td><a href="http://www.ruggedcircuits.com/motor-control/basic-motor-driver">Rugged Circuits</a></td>
      <td>Basic Motor Driver</td>
      <td>pwm:3,<br/>dir:12</td>
      <td>pwm:11,<br/>dir:13</td>
      <td>8-30V</td>
      <td>2A</td>
      <td>Yes</td>
    </tr>    
    <tr>
      <td><a href="https://www.sparkfun.com/products/9815">Sparkfun</a></td>
      <td>Ardumoto</td>
      <td>pwm:3,<br/>dir:12</td>
      <td>pwm:11,<br/>dir:13</td>
      <td>6 - 15V shared</td>
      <td>2A</td>
      <td>No</td>
    </tr>
  </tbody>
</table>

#### 3 Pin
<table>
  <thead>
    <tr>
      <th>Manufacturer</th>
      <th>Name</th>
      <th>Motor A pins</th>
      <th>Motor B pins</th>
      <th>Operating Voltage(1)</th>
      <th>Max Current</th>
      <th>Stackable(2)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Seeed Studios</td>
      <td>Motor Shield V1</td>
      <td>pwm:9,<br/>dir:8,<br/>cdir: 11</td>
      <td>pwm:10,<br/>dir:12,<br/>cdir: 13</td>
      <td>6-15V</td>
      <td>2A</td>
      <td>No</td>
    </tr>
    <tr>
      <td>Freetronics</td>
      <td>Dual Channel H-Bridge Motor Driver Shield</td>
      <td>pwm:6,<br/>dir:4,<br/>cdir: 7</td>
      <td>pwm:5,<br/>dir:3,<br/>cdir: 2</td>
      <td>8-40V</td>
      <td>2A</td>
      <td>No</td>
    </tr>
  </tbody>
</table>

1. Beware of shared voltage, the shield may be able to handle higher voltages than your Arduino. 
1. Configurable indicates that the pins can be reconfigured so that you can stack multiple shields of this type or other shields that use the same pins.