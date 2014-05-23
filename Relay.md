The `Relay` class constructs objects that represent a single digital Relay  attached to the physical board.

### Parameters

- **pin** A Number or String address for the pin. If digital, use the number, if analog use the "A" prefixed string.
```js
var relay = new five.Relay(13);
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
      <td>Any Pin</td>
      <td>The Number or String address of the Relay pin</td>
      <td>yes</td>
    </tr>

    <tr>
      <td>type</td>
      <td>String</td>
      <td>"NO", "NC"</td>
      <td>Normally Open or Normally Closed. Defaults to "NO"</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
```js
// Create a Relay object:
// 
var relay = new five.Relay({
  pin: 10
});

// Create a Normally Closed Relay object:
// 
var relay = new five.Relay({
  pin: 10, 
  type: "NC"
});
```

### Shape

```
{ 
  id: A user definable id value. Defaults to a generated uid
  pin: The pin value.
  isOn: true|false. READONLY
  type: "NO" or "NC". READONLY
}
```



### Usage
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var relay = new five.Relay(10);

  relay.on();

  relay.off();
});
```