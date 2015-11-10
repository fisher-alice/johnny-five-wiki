The `Pin` class constructs objects that represent any one pin on the physical board.

**For most cases, a proper Component class should be used instead of `Pin`.**


## Parameters

- **pin** A Number or String address for the pin. If digital, use the number, if analog use the "A" prefixed string.

- **options** An object of property parameters.

  | Property | Type | Value/Description | Default | Required | 
  | --- | --- | --- | --- | --- | 
  | id | Number, String | Non-specific. Any string or number value. Defaults to `null` || no | 
  | pin | Number, String | Any Pin. The Number or String address of the pin, defaults to 0, or addr when that is supplied | no || 
  | type | String | "digital", "analog". For most cases, this can be omitted; the type will be inferred based on the pin address number. | Inferred | no | 
  | mode | Number | See [Modes](#modes) | 1 or 2 depending on type | no |



## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to null | No |
| `pin` | The pin address of the pin | No |
| `type` | The type of pin this is, either "digital" or "analog" | No |
| `value` | The most recently reported value for this pin. | No |
| `mode` | The mode (number) for the pin.  See "Modes" below | No |


## Component Initialization

#### Basic

```js
// Digital
new five.Pin(13);

// Analog
new five.Pin("A0");
```

```js
// Digital
new five.Pin({
  pin: 13
});

// Analog
new five.Pin({
  pin: "A0"
});

// Analog As Digital
new five.Pin({
  pin: 14,
  type: "digital"
});
```

## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var strobe = new five.Pin(13);
  var state = 0x00;

  this.loop(500, function() {
    strobe.write(state ^= 0x01);
  });
});
```


## API

- **query(callback(value))** Query the board for the current state of this pin, invoking `callback` with argument `state` when complete.
  ```js
  var pin = new five.Pin(13);

  pin.query(function(state) {
    console.log(state);
  });
  ```
  An example pin state object looks like: 
  ```js
  { 
    supportedModes: [ 0, 1, 3, 4 ],
    mode: 0,
    value: 0,
    report: 1,
    state: 1,
    analogChannel: 127 
  }
  ```

  | Property | Description |
  | -------- | ----------- |
  | `supportedModes` | This is a list of modes that are supported by the pin. These numbers correspond to the [modes](#modes) below. |
  | `mode` | This is the present mode of the pin (eg. A digital output pin would have `mode: 1`).
  | `value` | This is the present value of the pin (eg. An analog pin with half voltage reading would have `value: 512`). |
  | `report` | This pin is presently reporting its value. 
  | `state` | For output modes, the `state` is any value that has been previously written to the pin. For input modes, the state is the status of the pullup resistor. |
  | `analogChannel` | The numeric "index" of an analog (ADC) pin, or `127` if digital (eg. "A0" would have `analogChannel: 0`). |


- **high()** Set the pin `HIGH`.
  ```js
  var pin = new five.Pin(13);
  // This will set pin 13 high (on)
  pin.high();
  pin.high();
  ```

- **low()** Set the pin `LOW`.
  ```js
  var pin = new five.Pin(13);
  // This will set pin 13 low (off)
  pin.low();
  ```

- **write(value)** Write a `value` to this pin.
  ```js
  var pin = new five.Pin(13);

  pin.write(1);
  ```

- **read(callback(error, value))** Register a handler to be called whenever the board reports the `value` (digital or analog) of this pin. 
  ```js
  var pin = new five.Pin(13);

  pin.read(function(error, value) {
    console.log(value);
  });
  ```

## Events

Whenever a pin is set to `INPUT` or `ANALOG`, it will automatically emit the following events: 

- **high** The "high" event is emitted whenever the pin goes high.

- **low** The "low" event is emitted whenever the pin goes low.

- **data** The "data" event is emitted for every all data (firehose).




## Static

### Modes

| Mode   | Value | Constant   |
|--------|-------|------------|
| INPUT  | 0     | Pin.INPUT  |
| OUTPUT | 1     | Pin.OUTPUT |
| ANALOG | 2     | Pin.ANALOG |
| PWM    | 3     | Pin.PWM    |
| SERVO  | 4     | Pin.SERVO  |

### Methods

- **Pin.write(pin instance, value)** Write a `value` to a `pin`.
  ```js
  // This will set pin 13 High
  var pin = new five.Pin(13);

  five.Pin.write(pin, 1);
  ```

- **Pin.read(pin instance, callback)** Register a handler to be called whenever the board reports the value (digital or analog) of the specified pin. 
  ```js
  var pin = new five.Pin(13);

  five.Pin.read(pin, function(error, value) {
    console.log(value);
  });
  ```

<!--remove-start-->

## Examples
- [Pin](https://github.com/rwldrn/johnny-five/blob/master/docs/pin.md)
- [Pin Circuit Event](https://github.com/rwldrn/johnny-five/blob/master/docs/pin-circuit-event.md)

<!--remove-end-->