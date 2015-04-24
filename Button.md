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




- **options** An object of property parameters.

  | Property | Type           | Value(s)                           | Description                                                                                                                                        | Required |
  |---------------|----------------|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
  | pin           | Number, String | 5, “I1” (Any digital pin on board) | The Number or String address of the pin the button is attached to, ie. 5 or “I1”                                                                   | yes      |
  | invert        | Boolean        | true or false                      | Invert the up and down values. This is useful for inverting button signals when the pin itself doesn’t have built-in pullup resistor capabilities. | no       |
  | isPullup      | Boolean        | true or false                      | Initialize as a pullup button                                                                                                                      | no       |
  | holdtime      | Number         | milliseconds                       | Number of milliseconds the button must be held until emitting a “hold” event. Defaults to 500ms                                                    | no       |


## Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pin: The pin address that the Button is attached to
  
  downValue: 0 or 1, depending on invert or pullup
  upValue: 0 or 1, depending on invert or pullup
  holdtime: milliseconds
}
```

## Component Initialization

#### Basic

```js
new five.Button(8);
```
![button breadboard](https://github.com/rwaldron/johnny-five/raw/master/docs/breadboard/button.png)

#### Inverted

```js
new five.Button({
  pin: 8, 
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
  var button = new five.Button(8);

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