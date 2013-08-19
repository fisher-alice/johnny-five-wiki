The `ShiftRegister` class constructs an object that represents a shift register.

### Parameters

* **options** An object of property parameters
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
      <td>
        ```
        {
          data: 2,
          clock: 3,
          latch: 4
        }
        ```
      </td>
     <td>
       Sets the values of the data , clock and latch pins
     </td>
      <td>
        yes
      </td>
    </tr>
  </tbody>
  </table>

   * **pins**
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
            <td>data</td>
            <td>Number</td>
            <td>
              Any pin on board
            </td>
            <td>
             Sets the pin corresponding to the shiftregisters's data pin
            </td>
            <td>
              yes
            </td>
          </tr>
          <tr>
            <td>clock</td>
            <td>Number</td>
            <td>
              Any pin on board
            </td>
            <td>
             Sets the pin corresponding to the shiftregisters's clock pin
            </td>
            <td>
              yes
            </td>
          </tr>
          <tr>
            <td>latch</td>
            <td>Number</td>
            <td>
              Any pin on board
            </td>
            <td>
             Sets the pin corresponding to the shiftregisters's latch pin
            </td>
            <td>
              yes
            </td>
          </tr>
        </tbody>
       </table>

### Shape

```js
{
  pins : the object containing the pin values for data, clock and latch 
}
```

### Usage

```js
var five = require("johnny-five"),
    board, shiftRegister;

board = new five.Board();

board.on("ready", function() {
  shiftRegister = new five.ShiftRegister({
    pins: {
      data: 2,
      clock: 3,
      latch: 4
    }
  });

});

```

## API

* **send(value)** send a value to the shiftregister
  ```js
  shiftRegister = new five.ShiftRegister({
    pins: {
      data: 2,
      clock: 3,
      latch: 4
    }
  });

  var value = 0;

  shiftRegister.send( 0x11 );    

  ```

## Events

Shiftregisters do not emit any events.

## Examples

* [Shiftregister](https://github.com/rwaldron/johnny-five/blob/master/docs/shiftregister.md)