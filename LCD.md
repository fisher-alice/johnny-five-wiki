The `LCD` class constructs an object that represents an LCD Display.

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
```js
{ rs, en, d4, d5, d6, d7 }
```
</td>
     <td>
       Sets the values of the rs, en, d4, d5, d6 and d7 pins.
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
  board: A reference to the board object the Led is attached to
  id: A user definable id value. Defaults to null
  bitmode: Defines the bitmode of the LCD display. Options are 4bit and 8bit.
  lines: The number of lines that the LCD display supports. Defaults to 2.
  rows: The number of rows that the LCD display supports. Defaults to 2.
  cols: The number of columns that the LCD display contains. Defaults to 16.
  dots: The number of dots per column. Defaults to 5x8.
  pins : the object containing the pin values for rs, en, d4, d5, d6, d7
}
```

### Usage

```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var lcd = new five.LCD({ pins: [ 2, 3, 4, 5, 11, 12 ] });

  lcd.print("Hello");
});

```

## API

- **print(message, opts)** prints the string 'message' to the LCD display at the cursor's current position
- if `opts.dontProcessSpecials` is set to true, it will not print out any special characters created by the `useChar(charCode|name)` function.

``` js

// No special characters
lcd.print("Bleep bloop");

// With special characters. This will print a heart character in 1 character space.
lcd.useChar("heart");
lcd.print(":heart:");

// With special characters, unprocessed. This will print the literal string ":heart:" in 7 character spaces.
lcd.useChar("heart");
lcd.print(":heart:", { dontProcessSpecials: true });
```

- **useChar(charCode|name)** Creates the character in LCD memory and adds character to current LCD character map. LCD memory is limited to 8 special characters at a time. 

``` js
lcd.useChar("heart");
lcd.print("Hello :heart:");
```

- **clear()** Clears all text on the LCD. 

``` js
lcd.print("Bleep bloop");
lcd.clear();
```

- **cursor(row, column)** Sets the cursor position.

``` js
lcd.cursor(0, 0).print("Bleep"); // The starting position of the LCD display
lcd.cursor(0, 1).print("Bloop"); // The second line, first character of the LCD display
```

- **home()** Sets the cursor position to row 0, column 0.

``` js
lcd.cursor(1, 0).print("Bloop");  // The second line, first character of the LCD display
lcd.home().print("Bleep"); // The first line, first character of the LCD display
```

- **display()** Turn the display on.

- **noDisplay()** Turn the display off.

- **blink()** This causes the cursor to show and blink

``` js
lcd.blink().print("Bleep Bloop");
```

- **noBlink()** This causes the cursor to stop blinking

``` js
lcd.noBlink().print("Bleep Bloop");
```

- **autoscroll()** Turns on automatic scrolling of the LCD. This causes each character output to the display to push previous characters over by one space.

``` js
lcd.autoscroll().print("Bloop").print("Bleep");
```

- **noAutoscroll()** Turns off automatic scrolling of the LCD.

``` js
lcd.noAutoscroll().print("Bloop").print("Bleep");
```

