![](https://i.gyazo.com/b3625fde0651907ff823c624c5431a43.png)

The `Switch` class constructs objects that represent a single Switch attached to the physical board.

## Parameters

- **pin** A Number or String address for the pin.

- **options** An object of property parameters.
  <span class="abbreviate-table">

  | Property | Type           | Value/Description                                        | Default | Required |
  |---------------|----------------|------------|----------------------------------------------------|----------|
  | pin           | Number, String | Any Pin. The Number or String address of the Switch pin     | | yes      |
  | type           | String | "NO" or "NC". Indicate if the switch is "normally open" or "normally closed" | "NO" | no |

  </span>  

## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid. | No |
| `pin` | The pin value. | No |
| `isClosed` | Boolean indicating whether the switch is closed. | Yes |
| `isOpen` | Boolean indicating whether the switch is open. | Yes |


## Component Initialization

#### Any

```js
new five.Switch(8);

// Options object with pin property
new five.Switch({
  pin: 8
});
```

![Switch SPDT](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/switch-spdt.png)

![Switch SPDT](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/switch-spst-rocker.png)

## Usage
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var spdt = new five.Switch(8);
  var led = new five.Led(13);

  spdt.on("open", function() {
    led.off();
  });

  spdt.on("close", function() {
    led.on();
  });
});
```

## Events

- **open** The `open` event will emit when the circuit opens. 
- **close** The `close` event will emit when the circuit closes. 


## Collection

`Switch` supports a `Switches` collection class, which allows multiple `Switch` instances to be controlled via a single instance object. Events emitted by instances of the `Switch` class are forwarded through instances of the `Switches` class. The handler receives the instance that emitted the event as the first parameter.

```js
new five.Switches([ 2, 3, 4, 5 ]);
new five.Switches([ { pin: 2 }, { pin: 3 }, { pin: 4 }, { pin: 5 } ]);
new five.Switches([ switch1, switch2, switch3 ]);
```


