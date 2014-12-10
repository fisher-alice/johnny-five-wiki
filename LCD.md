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

  var lcd = new five.LCD({ pins: [ 2,3,4,5,11,12 ] });

  lcd.on("ready", function(){
    lcd.clear();
    lcd.home();
    lcd.print("Hello");
  });
  });

```

## API

- **print(message,opts)** prints the string 'message' to the LCD display at the cursor's current position
- if opts.dontProcessSpecials is set to true, it will not print out any special characters created by the useChar(charCode) function.

``` js
  // Example without dontProcessSpecials
   var lcd = new five.LCD({ pins: [ 2,3,4,5,11,12 ] });
   lcd.print('Bleep bloop');
  
  // Example with dontProcessSpecials
  lcd.useChar("heart");
  lcd.print('Hello :heart:',{ dontProcessSpecials : false });
```

- **useChar(charCode)** Creates the character in LCD memory and adds character to current LCD character map 

``` js
 var lcd = new five.LCD({ pins: [ 2,3,4,5,11,12 ] });
 lcd.useChar("heart");
 lcd.print('Hello :heart:');
```

- **clear()** Clears all text on the LCD. 

``` js
  var lcd = new five.LCD({ pins: [ 2,3,4,5,11,12 ] });
  lcd.print('Bleep bloop');
  lcd.clear();
```

- **setCursor(x,y)** Sets the cursor the the position <x,y>

``` js
  var lcd = new five.LCD({ pins: [ 2,3,4,5,11,12 ] });
  lcd.setCursor(0,0).print('Bleep'); //The starting position of the LCD display
  lcd.setCursor(0,1).print('Bloop'); //The second line, first character of the LCD display
```

- **home()** Sets the cursor the the position <0,0>

``` js
  var lcd = new five.LCD({ pins: [ 2,3,4,5,11,12 ] });
  lcd.setCursor(0,1).print('Bloop');  //The second line, first character of the LCD display
  lcd.home().print('Bleep'); //The second line, first character of the LCD display
```

- **display()** Shows the characters on the LCD display

``` js
  var lcd = new five.LCD({ pins: [ 2,3,4,5,11,12 ] });
  lcd.display().print('Bleep Bloop');
```
- **noDisplay()** Shows the characters on the LCD display

``` js
  var lcd = new five.LCD({ pins: [ 2,3,4,5,11,12 ] });
  lcd.noDisplay().print('Bleep Bloop'); //You will not see this text on the LCD display
```

- **blink()** This causes the cursor to show and blink

``` js
  var lcd = new five.LCD({ pins: [ 2,3,4,5,11,12 ] });
  lcd.blink().print('Bleep Bloop');
```

- **noBlink()** This causes the cursor to stop blinking

``` js
  var lcd = new five.LCD({ pins: [ 2,3,4,5,11,12 ] });
  lcd.noBlink().print('Bleep Bloop');
```
- **autoscroll()** Turns on automatic scrolling of the LCD. This causes each character output to the display to push previous characters -  over by one space

``` js
  var lcd = new five.LCD({ pins: [ 2,3,4,5,11,12 ] });
  lcd.autoscroll().print('Bloop').print('Bleep');
```
- **noAutoscroll()** Turns off automatic scrolling of the LCD.

``` js
  var lcd = new five.LCD({ pins: [ 2,3,4,5,11,12 ] });
  lcd.noAutoscroll().print('Bloop').print('Bleep');
```

## Events

- **ready** The "ready" event is emitted whenever the value of the LCD has completed setup and is ready for use.
