The `Led.Matrix` class constructs objects that represent one or more (connected) LED Matrix devices attached to the physical board.


### Parameters


- **options** An object of property parameters.
<table>
  <thead>
    <tr>
      <th>Property Name</th>
      <th>Type</th>
      <th>Value(s)</th>
      <th>Properties</th>
      <th>Required</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>pins</td>
      <td>Object</td>
      <td>A valid pins object or pins array</td>
      <td>`data`, `clock`, `cs`</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>devices</td>
      <td>Number</td>
      <td>1-n</td>
      <td>
        For single device cases, this can be omitted. Defaults to `1`.
      </td>
      <td>no</td>
    </tr>

  </tbody>
</table>
```js
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});

```

### Shape

```js
{ 
  isMatrix: ...Boolean true|false (from LedControl). READONLY
  devices: ...Number of devices controlled. READONLY
}
```




### Usage
```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var matrix = new five.Led.Matrix({
    pins: {
      data: 2,
      clock: 3,
      cs: 4
    }
  });

  matrix.on();

  this.repl.inject({
    display: matrix.device(0)
  });
});
```


## API

- **on(device)** or **on()** Turn the matrix device on. 

``` js
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});

// Specify a device index. Defaults to 0
matrix.on(0);

// Same as
matrix.on();
```

- **off(device)** or **off()** Turn the matrix device off.

``` js
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});

// Specify a device index. Defaults to 0
matrix.off(0);

// Same as
matrix.off();
```

- **clear(device)** or **clear()** Turn off all LEDs, without turning off the matrix device.

``` js
var matrix = new five.Led.Matrix({
  pins: {
    data: 2,
    clock: 3,
    cs: 4
  }
});

// Specify a device index. Defaults to 0
matrix.clear(0);

// Same as
matrix.clear();
```


TODO: 

- led
- row
- column
- draw



## Events

`Led.Matrix` objects are output only and therefore do not emit any events.
