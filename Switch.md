![](https://i.gyazo.com/b3625fde0651907ff823c624c5431a43.png)

The `Switch` class constructs objects that represent a single Switch attached to the physical board.

## Parameters

- **pin** A Number or String address for the pin.

- **options** An object of property parameters.

  | Property | Type           | Value/Description                                        | Default | Required |
  |---------------|----------------|------------|----------------------------------------------------|----------|
  | pin           | Number, String | Any Pin. The Number or String address of the Switch pin     | | yes      |
  

## Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pin: The pin value.
  isClosed: true|false. READONLY
}
```


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
