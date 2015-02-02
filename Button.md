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
        <td>pin</td>
        <td>Number, String</td>
        <td>5, "I1" (Any digital pin on board)</td>
        <td>The Number or String address of the pin the button is attached to, ie. 5 or "I1"</td>
        <td>yes</td>
      </tr>
      <tr>
        <td>invert</td>
        <td>Boolean</td>
        <td>true or false</td>
        <td>Invert the up and down values. This is useful for inverting button signals when the pin itself doesn't have built-in pullup resistor capabilities.</td>
        <td>no</td>
      </tr>
      <tr>
        <td>isPullup</td>
        <td>Boolean</td>
        <td>true or false</td>
        <td>Initialize as a pullup button</td>
        <td>no</td>
      </tr>
      <tr>
        <td>holdtime</td>
        <td>Number</td>
        <td>milliseconds</td>
        <td>Number of milliseconds the button must be held until emitting a "hold" event. Defaults to 500ms</td>
        <td>no</td>
      </tr>    
    </tbody>
  </table>


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

```js
// A basic button
// 
//   - attached to pin 8
//   - emits down/press event
//
var button = new five.Button(8);

button.on("press", function() {
  console.log( "Button has been pressed" );
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


## Examples

- [Button](https://github.com/rwldrn/johnny-five/blob/master/docs/button.md)
- [Button Bumper](https://github.com/rwldrn/johnny-five/blob/master/docs/button-bumper.md)
- [Button Options](https://github.com/rwldrn/johnny-five/blob/master/docs/button-options.md)
- [Button Pullup](https://github.com/rwldrn/johnny-five/blob/master/docs/button-pullup.md)
- [Tinkerkit Button](https://github.com/rwldrn/johnny-five/blob/master/docs/tinkerkit-button.md)