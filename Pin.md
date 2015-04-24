The `Pin` class constructs objects that represent any one pin on the physical board.

**For most cases, a proper Component class should be used instead of `Pin`.**


## Parameters

- **pin** A Number or String address for the pin. If digital, use the number, if analog use the "A" prefixed string.

- **options** An object of property parameters.

  | Property | Type | Value(s) | Description | Required | 
  | --- | --- | --- | --- | --- | 
  | id | Number, String | Non-specific | Any string or number value. Defaults to `null` | no | 
  | pin | Number, String | Any Pin | The Number or String address of the pin, defaults to 0, or addr when that is supplied | no | 
  | type | String | "digital", "analog" | For most cases, this can be omitted; the type will be inferred based on the pin address number. | no | 
  | mode | Number | 0 .. 4 | The available mode constants are described further down the page.  If not specified, mode defaults to 1 or 2 depending on type | no |
  | addr | Number, String | Any Pin | The Number or String address of the pin | no | 



## Shape

```
{ 
  id: A user definable id value. Defaults to null
  pin: The pin address of the pin
  type: The type of pin this is, either "digital" or "analog"
  value: The most recently reported value for this pin.
  mode: The mode (number) for the pin.  See "Modes" below
}
```


## Component Initialization

#### Basic

```js
var digital = new five.Pin(13);

var analog = new five.Pin("A0");
```

```js
var digital = new five.Pin({
  pin: 13
});

var analog = new five.Pin({
  pin: "A0"
});

var analogAsDigital = new five.Pin({
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
    analogChannel: 127 
  }
  ```

  **Modes**

  <table>
    <thead>
      <tr>
        <th>Mode</th>
        <th>Value</th>
        <th>Constant</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>INPUT</td>
        <td>0</td>
        <td>Pin.INPUT</td>
      </tr>
      <tr>
        <td>OUTPUT</td>
        <td>1</td>
        <td>Pin.OUTPUT</td>
      </tr>
      <tr>
        <td>ANALOG</td>
        <td>2</td>
        <td>Pin.ANALOG</td>
      </tr>
      <tr>
        <td>PWM</td>
        <td>3</td>
        <td>Pin.PWM</td>
      </tr>
      <tr>
        <td>SERVO</td>
        <td>4</td>
        <td>Pin.SERVO</td>
      </tr>
    </tbody>
  </table>


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
<table>
  <thead>
    <tr>
      <th>Mode</th>
      <th>Value</th>
      <th>Constant</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>INPUT</td>
      <td>0</td>
      <td>Pin.INPUT</td>
    </tr>
    <tr>
      <td>OUTPUT</td>
      <td>1</td>
      <td>Pin.OUTPUT</td>
    </tr>
    <tr>
      <td>ANALOG</td>
      <td>2</td>
      <td>Pin.ANALOG</td>
    </tr>
    <tr>
      <td>PWM</td>
      <td>3</td>
      <td>Pin.PWM</td>
    </tr>
    <tr>
      <td>SERVO</td>
      <td>4</td>
      <td>Pin.SERVO</td>
    </tr>
  </tbody>
</table>

### Methods

- **Pin.write(pin, value)** Write a `value` to a `pin`.
  ```js
  // This will set pin 13 High
  five.Pin.write(13, 1);
  ```

- **Pin.read(pin, callback)** Register a handler to be called whenever the board reports the value (digital or analog) of the specified pin. 
  ```js
  five.Pin.read(13, function(error, value) {
    console.log(value);
  });
  ```

<!--remove-start-->

## Examples
- [Pin](https://github.com/rwldrn/johnny-five/blob/master/docs/pin.md)
- [Pin Circuit Event](https://github.com/rwldrn/johnny-five/blob/master/docs/pin-circuit-event.md)

<!--remove-end-->
