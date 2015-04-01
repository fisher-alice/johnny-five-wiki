The `Motor` class constructs objects that represent a single Motor. The motor may attach to the physical board or a motor controller. The controller may be a third party shield or custom built motor controller. This class works well with both **Directional** and **Non-Directional** motors. It also works well with 2-pin or 3-pin controllers.

### Parameters
 * **pin** A Number or String address for the Non-Directional Motor pin (PWM).
 * **pins** An array of 2 or 3 Numbers or String addresses for the Bi-Directional Motor pins. 
 * **options** An object of property parameters.

    | Property   | Type                            | Value(s)/Description                                                 | Required                    |
    |------------|---------------------------------|----------------------------------------------------------------------|-----------------------------|
    | pins       | Object                          | A valid pins object or pins array                                    | yes                         |
    | current    | Object                          | A valid Sensor options object\*                                      | no                          |
    | invertPWM  | Boolean                         | true or false                                                        | no                          |
    | address    | Number (usually in hexadecimal) | An I2C device address                                                | no                          |
    | controller | String                          | Motor controller interface type                                      | no                          |
    | register   | Object {data, clock, latch}     | Pin configuration for a ShiftRegister                                | no                          |
    | bits       | Object {a, b}                   | Switch bits to be flipped to control an HBridge from a ShiftRegister | only if register is defined |



## Shape

```
{ 
  isOn: A boolean flag, true when motor is moving or braking, false when not READONLY
}
```

## Component Initialization

#### Two Pin H-Bridge [pwm, dir]

```js
var motor = new five.Motor([3, 12]);

var motor = new five.Motor({
  pins: {
    pwm: 3,
    dir: 12
  }
});
```

#### Three Pin H-Bridge [pwm, dir, cdir]

```js
var motor = new five.Motor([9, 8, 11]);

var motor = new five.Motor({
  pins: {
    pwm:9,
    dir:8,
    cdir: 11  
  }
});
```

#### Three Pin H-Bridge [pwm, dir, brake]

```js
var motor = new five.Motor([9, 8, 11]);

var motor = new five.Motor({
  pins: {
    pwm:9,
    dir:8,
    brake: 11  
  }
});
```

### Pre-packaged Shield Configs

There are several shields that Johnny-Five provides preset configuration objects for. Instantiating motors using these shield configurations is designed to be extremely simple:


#### Arduino Motor Shield R3

```js
var configs = five.Motor.SHIELD_CONFIGS.ARDUINO_MOTOR_SHIELD_R3_1;

var motorA = new five.Motor(configs.A);
var motorB = new five.Motor(configs.B);
```

#### DF Robot

```js
var configs = five.Motor.SHIELD_CONFIGS.DF_ROBOT;

var motorA = new five.Motor(configs.A);
var motorB = new five.Motor(configs.B);
```


#### Rugged Circuits Rugged Motor Driver

```js
var configs = five.Motor.SHIELD_CONFIGS.RUGGED_CIRCUITS;

var motorA = new five.Motor(configs.A);
var motorB = new five.Motor(configs.B);
```


#### Sparkfun Ardumoto

```js
var configs = five.Motor.SHIELD_CONFIGS.SPARKFUN_ARDUMOTO;

var motorA = new five.Motor(configs.A);
var motorB = new five.Motor(configs.B);
```


#### Seeed Studios Motor Shield V1 or V2

```js
var configs = five.Motor.SHIELD_CONFIGS.SEEED_STUDIO;

var motorA = new five.Motor(configs.A);
var motorB = new five.Motor(configs.B);
```


#### Freetronics Dual Channel H-Bridge Motor Driver Shield


```js
var configs = five.Motor.SHIELD_CONFIGS.FREETRONICS_HBRIDGE;

var motorA = new five.Motor(configs.A);
var motorB = new five.Motor(configs.B);
```

#### Adafruit Motor/Stepper/Servo Shield V1

```js
var configs = five.Motor.SHIELD_CONFIGS.ADAFRUIT_V1;

var motor1 = new five.Motor(configs.M1);
var motor2 = new five.Motor(configs.M2);
var motor3 = new five.Motor(configs.M3);
var motor4 = new five.Motor(configs.M4);
```


#### Adafruit Motor/Stepper/Servo Shield V2

```js
var configs = five.Motor.SHIELD_CONFIGS.ADAFRUIT_V2;

var motor1 = new five.Motor(configs.M1);
var motor2 = new five.Motor(configs.M2);
var motor3 = new five.Motor(configs.M3);
var motor4 = new five.Motor(configs.M4);
```

#### Pololu DRV8835 Shield

```js
var configs = five.Motor.SHIELD_CONFIGS.POLOLU_DRV8835_SHIELD;

var motor1 = new five.Motor(configs.M1);
var motor2 = new five.Motor(configs.M2);
```




## Usage

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

Directional Motor with ShiftRegister to control HBridge (Like the AdaFruit Motor Shield V1)
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var motor = new five.Motor({
      pins: { pwm: 11 },
      register: { data: 8, clock: 4, latch: 12 },
      bits: { a: 2, b: 3 }
    }
  });

  // Start the motor at maximum speed
  motor.forward(255);

});
```

Directional Motor via Adafruit Motor Shield V2
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var motor = new five.Motor({
      pins: {
        pwm: 8,
        dir: 9,
        cdir: 10
      },
      address: 0x60,
      controller: "PCA9685"
    }
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

<!--remove-start-->

## Examples
- [Motor](https://github.com/rwldrn/johnny-five/blob/master/docs/motor.md)
- [Directional Motor](https://github.com/rwldrn/johnny-five/blob/master/docs/motor-directional.md)
- [Motor 3-pin](https://github.com/rwldrn/johnny-five/blob/master/docs/motor-3-pin.md)
- [Motor with Brake](https://github.com/rwldrn/johnny-five/blob/master/docs/motor-brake.md)
- [Motor with Current Sensor](https://github.com/rwldrn/johnny-five/blob/master/docs/motor-current.md)
- [Motor PCA9685 via I2C](https://github.com/rwldrn/johnny-five/blob/master/docs/motor-PCA9685.md)

<!--remove-end-->

## Additional Notes

The PWM pins on an Arduino Uno only output about 40mA. That is barely enough to power a humble hobby motor. You are going to need a motor controller between your Arduino and your motor(s) to deliver power from an external source. Most motor controllers are based on the H-Bridge circuit which uses a set of four switches to direct the voltage being sent through each pole of the motor. Different shields handle different input voltages and output currents. Use the Motor Control Shield Survey below to find a shield that works for you.

### Forward and Reverse are Interchangeable

Keep in mind that "forward" and "reverse" are arbitrary labels. If your motor is turning in the wrong direction you can just switch the poles on the motor. Consider a robot with two motors connected directly to the drive wheels. For your bot to go forward, one should turn clockwise and the other should turn counter-clockwise. Switch the poles on one of those motors so that you can use forward() on both and have them work together.

### Situations Where ```invertPWM: true``` is Required

Most motor controller board/shield manufacturers abstract the need for this away. If you have wired up your own motor controller or you have purchased a very basic motor controller board you may need this property. To check, set both pwm and dir to their highest values by calling ```motor.forward(255)```. If nothing happens try calling ```motor.stop()```. If this makes the motor move you need to add ```invertPWM: true``` as in the example below. Instead of the pins being used for PWM and DIR they are being used to directly control the voltage applied to each pole of the motor. If both are set to high, the motor will not move.

````
var motor = new five.Motor({pins:[8,9], invertPWM:true});
````

### Differences Between 2 and 3 pin Directional Motor Controllers

Controllers that use 2 pins instead of 3 are essentially the same. Both arrangements use one PWN pin to control speed. The switches on the H-Bridge work in pairs. With 3-pin controllers you control the state of each pair. With 2-pin controllers the pairs are toggled for you based on the state of that one digital pin.

### Motor Control Shield Survey
This is by no means exhaustive

#### 2 Pin
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Motor A pins</th>
      <th>Motor B pins</th>
      <th>Shield Config</th>
      <th>Operating Voltage(1)</th>
      <th>Max A per Channel</th>
      <th>Stackable(2)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="http://arduino.cc/en/Main/ArduinoMotorShieldR3">Arduino Motor Shield R3</a></td>
      <td>pwm:3<br/>dir:12<br/>[brake:9,]<br/>[current:A0]</td>
      <td>pwm:11<br/>dir:13<br/>[brake:8,]<br/>[current:A1]</td>
      <td>ARDUINO_MOTOR_SHIELD_R3_1 {A, B} (vanilla)<br/>ARDUINO_MOTOR_SHIELD_R3_2 {A, B} (w/brake)<br/>ARDUINO_MOTOR_SHIELD_R3_3 {A, B} (w/brake & current)</td>
      <td>7-12V</td>
      <td>2A</td>
      <td>No</td>
    </tr>    
    <tr>
      <td><a href="http://www.dfrobot.com/index.php?route=product/product&product_id=59">DF Robot 1A</a></td>
      <td>pwm:6<br/>dir:7</td>
      <td>pwm:5<br/>dir:4</td>
      <td>DF_ROBOT {A, B}</td>
      <td>7 - 12V</td>
      <td>1A</td>
      <td>No</td>
    </tr>    
    <tr>
      <td><a href="http://www.dfrobot.com/index.php?route=product/product&product_id=69">DF Robot 2A</a></td>
      <td>pwm:6<br/>dir:7</td>
      <td>pwm:5<br/>dir:4</td>
      <td>DF_ROBOT {A, B}</td>
      <td>4.8 - 35V</td>
      <td>2A</td>
      <td>No</td>
    </tr>    
    <tr>
      <td><a href="http://www.nkcelectronics.com/freeduino-arduino-motor-control-shield-kit.html">NKC Electronics Motor Control Shield Kit</a></td>
      <td>pwm:9<br/>dir:12</td>
      <td>pwm:10<br/>dir:13</td>
      <td>NKC_ELECTRONICS_KIT {A, B}</td>
      <td>6 - 15V shared</td>
      <td>1A</td>
      <td>No</td>
    </tr>    
    <tr>
      <td><a href="http://www.ruggedcircuits.com/motor-control/rugged-motor-driver">Rugged Circuits Rugged Motor Driver</a></td>
      <td>pwm:3<br/>dir:12</td>
      <td>pwm:11<br/>dir:13</td>
      <td>RUGGED_CIRCUITS {A, B}</td>
      <td>8-30V</td>
      <td>2.8A</td>
      <td>Yes</td>
    </tr>    
    <tr>
      <td><a href="http://www.ruggedcircuits.com/motor-control/basic-motor-driver">Rugged Circuits Basic Motor Driver</a></td>
      <td>pwm:3<br/>dir:12</td>
      <td>pwm:11<br/>dir:13</td>
      <td>RUGGED_CIRCUITS {A, B}</td>
      <td>8-30V</td>
      <td>2A</td>
      <td>Yes</td>
    </tr>    
    <tr>
      <td><a href="https://www.sparkfun.com/products/9815">Sparkfun Ardumoto</a></td>
      <td>pwm:3<br/>dir:12</td>
      <td>pwm:11<br/>dir:13</td>
      <td>SPARKFUN_ARDUMOTO {A, B}</td>
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
      <th>Name</th>
      <th>Motor A pins</th>
      <th>Motor B pins</th>
      <th>Shield Config</th>
      <th>Operating Voltage(1)</th>
      <th>Max A</th>
      <th>Stackable(2)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="http://www.seeedstudio.com/depot/Motor-Shield-p-913.html">Seeed Studios Motor Shield V1</a> and <a href="http://www.seeedstudio.com/depot/motor-shield-v20-p-1377.html?cPath=132_134">V2</a></td>
      <td>pwm:9<br/>dir:8<br/>cdir: 11</td>
      <td>pwm:10<br/>dir:12<br/>cdir: 13</td>
      <td>SEEED_STUDIO {A, B}</td>
      <td>6-15V</td>
      <td>2A</td>
      <td>No</td>
    </tr>
    <tr>
      <td><a href="http://www.freetronics.com/products/hbridge-dual-channel-h-bridge-motor-driver-shield">Freetronics Dual Channel H-Bridge Motor Driver Shield</a></td>
      <td>pwm:6<br/>dir:4<br/>cdir: 7</td>
      <td>pwm:5<br/>dir:3<br/>cdir: 2</td>
      <td>FREETRONICS_HBRIDGE: {A, B}</td>
      <td>8-40V</td>
      <td>2A</td>
      <td>No</td>
    </tr>
  </tbody>
</table>

#### I2C
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Address</th>
      <th>Controller</th>
      <th>Shield Config</th>
      <th>Operating Voltage(1)</th>
      <th>Max A</th>
      <th>Stackable(2)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="http://www.adafruit.com/products/1438">Adafruit Motor/Stepper/Servo Shield V2</a></td>
      <td>0x60 - 0x80</td>
      <td>PCA9685</td>
      <td>ADAFRUIT_V2 {M1, M2, M3, M4}
      <td>4.5-13.5V</td>
      <td>3A</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>
      </td>
      <td colspan="6">
        <table>
          <thead>
            <tr>
              <th>M1</th>
              <th>M2</th>
              <th>M3</th>
              <th>M4</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>pwm:8<br/>dir:9<br/>cdir: 10</td>
              <td>pwm:13<br/>dir:12<br/>cdir: 11</td>
              <td>pwm:2<br/>dir:3<br/>cdir: 4</td>
              <td>pwm:7<br/>dir:6<br/>cdir: 5</td>
            </tr>
          </tbody>
        </table>
        </td>
    </tr>    
  </tbody>
</table>

#### ShiftRegister
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Register</th>
      <th>Shield Config</th>
      <th>Operating Voltage(1)</th>
      <th>Max A</th>
      <th>Stackable(2)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="http://www.sainsmart.com/arduino/arduino-shields/motor-shields/sainsmart-l293d-motor-drive-shield-for-arduino-duemilanove-mega-uno-r3-avr-atmel.html">SainSmart L293D Motor Drive Shield <br/>(clone of AdaFruit Motor Shield v1)</a></td>
      <td>data: 8<br/>clock: 4<br/>latch: 12</td>
      <td>ADAFRUIT_V1: {M1, M2, M3, M4}</td>
      <td>4.5-10V</td>
      <td>1.2A per motor</td>
      <td>No</td>
    </tr>
    <tr>
      <td>
      </td>
      <td colspan="6">
        <table>
          <thead>
            <tr>
              <th>M1</th>
              <th>M2</th>
              <th>M3</th>
              <th>M4</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>pins: { pwm: 11 }<br/>bits: {a: 2, b: 3}</td>
              <td>pins: { pwm: 3 }<br/>bits: {a: 1, b: 4}</td>
              <td>pins: { pwm: 6 }<br/>bits: {a: 5, b: 7}</td>
              <td>pins: { pwm: 5 }<br/>bits: {a: 0, b: 6}</td>
            </tr>
          </tbody>
        </table>
        </td>
    </tr>    
  </tbody>
</table>

1. Beware of shared voltage, the shield may be able to handle higher voltages than your Arduino. 
1. Indicates that the pins can be reconfigured so that you can stack multiple shields of this type or other shields that use the same pins.
