![](http://i.gyazo.com/64310ac819ea06fe771101c95dbf8e96.png)

The `Button` class constructs objects that represents a single Button attached to the physical board. 


## Parameters

- **pin** A Number or String address for the Button pin (digital).
  ```js
  var button = new five.Button(8);
  ```

  ```js
  // Attached to an analog pin
  var button = new five.Button("A0");
  ```

- **Options** An object of property parameters.
  <span class="abbreviate-table">

  | Property | Type           | Value/Description                                                                                                                                        | Default | Required |
  |---------------|----------------|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
  | pin           | Number, String | Digital Pin. The Number or String address of the pin the button is attached to, ie. 5 or “I1”                                                                   | | yes      |
  | invert        | Boolean        | `true`, `false`. Invert the up and down values. This is useful for inverting button signals when the pin itself doesn’t have built-in pullup resistor capabilities. | `false` | no       |
  | isPullup      | Boolean        | `true`, `false`. Initialize as a pullup button                                                                                                                      | `false` | no       |
  | holdtime      | Number         | Time in milliseconds that the button must be held until emitting a "hold" event. | 500ms                                                    | no       |

   </span>

## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to a generated uid | No |
| `pin` | The pin address that the Button is attached to | No |
| `downValue` | 0 or 1, depending on invert or pullup | No |
| `upValue` | 0 or 1, depending on invert or pullup | No |
| `holdtime` | milliseconds | No |


## Component Initialization

#### Basic

```js
new five.Button(2);
```
![button breadboard](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/button.png)

#### Inverted

```js
new five.Button({
  pin: 2, 
  invert: true
});
```
![button breadboard](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/button.png)

### Usage
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  // Create a new `button` hardware instance.
  var button = new five.Button(2);

  button.on("hold", function() {
    console.log( "Button held" );
  });

  button.on("press", function() {
    console.log( "Button pressed" );
  });

  button.on("release", function() {
    console.log( "Button released" );
  });
});
```


## Events

- **hold** The button has been held for `holdtime` milliseconds

- **down**, **press** The button has been pressed.

- **up**, **release** The button has been released.

<!--remove-start-->

## Examples

- [Button](https://github.com/rwldrn/johnny-five/blob/master/docs/button.md)
- [Button Bumper](https://github.com/rwldrn/johnny-five/blob/master/docs/button-bumper.md)
- [Button Options](https://github.com/rwldrn/johnny-five/blob/master/docs/button-options.md)
- [Button Pullup](https://github.com/rwldrn/johnny-five/blob/master/docs/button-pullup.md)
- [Tinkerkit Button](https://github.com/rwldrn/johnny-five/blob/master/docs/tinkerkit-button.md)

<!--remove-end-->