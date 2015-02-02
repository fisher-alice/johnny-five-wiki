The `LCD` class constructs an object that represents an LCD Display.

## Parameters

- **General Options**
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
      <td>rows</td>
      <td>Number</td>
      <td>
        The number of rows on the device
      </td>
      <td>
        Defaults to 2
      </td>
      <td>
        No
      </td>
    </tr>
    <tr>
      <td>cols</td>
      <td>Number</td>
      <td>
        The number of columns on the device
      </td>
      <td>
        Defaults to 16
      </td>
      <td>
        No
      </td>
    </tr>
  </tbody>
  </table>


- **I2C Options**
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
        <td>controller</td>
        <td>String</td>
        <td>
  PCF8574, PCF8574A, JHD1313M1 (Grove)
  </td>
       <td>
         The name of the controller to use.
       </td>
        <td>
          yes
        </td>
      </tr>
    </tbody>
  </table>




- **Parallel Options (Default)**
 <table>
  <thead>
    <tr>
      <th>Property Name</th>
      <th>Type</th>
      <th>Value(s)</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>pins</td>
      <td>Object</td>
      <td>
```js
{ 
  rs: ..., 
  en: ..., 
  d4: ..., 
  d5: ..., 
  d6: ..., 
  d7: ... 
}
```
</td>
     <td>
       Sets the values of the rs, en, d4, d5, d6 and d7 pins.
     </td>
    </tr>
    <tr>
      <td>pins</td>
      <td>Array</td>
      <td>
```js
[ rs, en, d4, d5, d6, d7 ]
```
</td>
     <td>
       Sets the values of the rs, en, d4, d5, d6 and d7 pins.
     </td>
    </tr>
  </tbody>
  </table>






## Shape

```js
{
  id: A user definable id value. Defaults to null
  rows: The number of rows that the LCD display supports. Defaults to 2.
  cols: The number of columns that the LCD display contains. Defaults to 16.
}
```

## Component Initialization

```js
// Parallel LCD
var lcd = new five.LCD({ 
  pins: [8, 9, 4, 5, 6, 7]
});

// Parallel LCD w/ Backlight
var lcd = new five.LCD({ 
  pins: [8, 9, 4, 5, 6, 7],
  backlight: 13,
});

// Parallel LCD w/ Backlight & Explicit Rows and Columns (20 cols, 2 rows)
var lcd = new five.LCD({ 
  pins: [8, 9, 4, 5, 6, 7],
  backlight: 13,
  rows: 2,
  cols: 16
});
```

![LCD](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/lcd.png)

```js
// I2C LCD, PCF8574
var lcd = new five.LCD({ 
  controller: "PCF8574"
});

// I2C LCD, PCF8574A
var lcd = new five.LCD({ 
  controller: "PCF8574A"
});

// I2C LCD, PCF8574
var lcd = new five.LCD({ 
  controller: "PCF8574"
});

// I2C LCD (RGB Controller), JHD1313M1 (this is the Grove LCD)
var lcd = new five.LCD({ 
  controller: "JHD1313M1"
});
```

![LCD I2C](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/lcd-i2c-PCF8574.png)

## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

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

  // With special characters.
  // This will print a heart character in 1 character space.
  lcd.useChar("heart");
  lcd.print(":heart:");

  // With special characters, unprocessed.
  // This will print the literal string ":heart:" in 7 character spaces.
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
  // The starting position of the LCD display
  lcd.cursor(0, 0).print("Bleep");

  // The second line, first character of the LCD display
  lcd.cursor(0, 1).print("Bloop");

  ```

- **home()** Sets the cursor position to row 0, column 0.

  ``` js
  // The second line, first character of the LCD display
  lcd.cursor(1, 0).print("Bloop");

  // The first line, first character of the LCD display
  lcd.home().print("Bleep");
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

