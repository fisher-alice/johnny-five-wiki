The `LCD` class constructs an object that represents an LCD Display. Controllers are generally named for the chip used or component model number. Most LCD's use the HD44780 screen (or similar), so be sure to check the model number or chip name for the right controller (valid controllers are listed below).

## Parameters

- **General Options**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description    | Default | Required |
  |---------------|--------|-------------------------------------|----------------|----------|
  | rows          | Number | The number of rows on the device    | 2  | No       |
  | cols          | Number | The number of columns on the device | 16 | No       |
  </span>

- **I2C Options**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                        | Default | Required |
  |---------------|--------|--------------------------------------|------------------------------------|----------|
  | controller    | String | "PCF8574", "PCF8574A", "JHD1313M1" (Grove). The name of the controller to use. | | Yes      |
  </span>

- **Parallel Options (Default Controller)**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                                           |Default | Required |
  |---------------|--------|--------------------------------------------------------|---|----------|
  | pins          | Object | `{ rs, en, d4, d5, d6, d7 }`. Sets the values of the rs, en, d4, d5, d6 and d7 pins. | | Yes (Either) |
  | pins          | Array  | `[ rs, en, d4, d5, d6, d7 ]`. Sets the values of the rs, en, d4, d5, d6 and d7 pins. | | Yes (Either) |
  </span>


## Shape

| Property Name | Description | Read Only |
|---------------| ----------- | ----------|
| `id` | A user definable id value. Defaults to null | No |
| `rows` | The number of rows that the LCD display supports. Defaults to 2. | No |
| `cols` | The number of columns that the LCD display contains. Defaults to 16. | No |

## Component Initialization

#### Parallel

```js
// Parallel LCD
new five.LCD({ 
  pins: [7, 8, 9, 10, 11, 12]
});

// Parallel LCD w/ Backlight
new five.LCD({ 
  pins: [7, 8, 9, 10, 11, 12],
  backlight: 13,
});

// Parallel LCD w/ Backlight & Explicit Rows and Columns (20 cols, 2 rows)
new five.LCD({ 
  pins: [7, 8, 9, 10, 11, 12],
  backlight: 13,
  rows: 2,
  cols: 16
});
```

![LCD](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/lcd.png)

#### I2C, PCF8574 (Generic)

```js
// I2C LCD, PCF8574
new five.LCD({ 
  controller: "PCF8574"
});

// I2C LCD, PCF8574A
new five.LCD({ 
  controller: "PCF8574A"
});

// I2C LCD, PCF8574
new five.LCD({ 
  controller: "PCF8574"
});
```

![LCD I2C](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/lcd-i2c-PCF8574.png)

#### I2C, JHD1313M1 (Grove)

```js
new five.LCD({ 
  controller: "JHD1313M1"
});
```

![LCD Grove](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/grove-lcd-rgb.png)

## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var lcd = new five.LCD({ pins: [7, 8, 9, 10, 11, 12] });

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

- **on()** Turn the display on.

- **off()** Turn the display off.

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


## Predefined Characters

```

0:
  ■ ■ ■   
■ ■   ■ ■ 
■ ■   ■ ■ 
■ ■   ■ ■ 
■ ■   ■ ■ 
■ ■   ■ ■ 
  ■ ■ ■   

1:
      ■   
    ■ ■   
  ■ ■ ■   
    ■ ■   
    ■ ■   
    ■ ■   
    ■ ■   

2:
  ■ ■ ■   
■ ■   ■ ■ 
      ■ ■ 
    ■ ■   
  ■ ■     
■ ■       
■ ■ ■ ■ ■ 

3:
  ■ ■ ■   
■ ■   ■ ■ 
      ■ ■ 
  ■ ■ ■   
      ■ ■ 
■ ■   ■ ■ 
  ■ ■ ■   

4:
      ■ ■ 
    ■ ■ ■ 
  ■ ■ ■ ■ 
■ ■   ■ ■ 
■ ■ ■ ■ ■ 
      ■ ■ 
      ■ ■ 

5:
■ ■ ■ ■ ■ 
■ ■       
■ ■ ■ ■   
      ■ ■ 
      ■ ■ 
■ ■   ■ ■ 
  ■ ■ ■   

6:
  ■ ■ ■   
■ ■   ■ ■ 
■ ■       
■ ■ ■ ■   
■ ■   ■ ■ 
■ ■   ■ ■ 
  ■ ■ ■   

7:
■ ■ ■ ■ ■ 
      ■ ■ 
    ■ ■   
  ■ ■     
  ■ ■     
  ■ ■     
  ■ ■     

8:
  ■ ■ ■   
■ ■   ■ ■ 
■ ■   ■ ■ 
  ■ ■ ■   
■ ■   ■ ■ 
■ ■   ■ ■ 
  ■ ■ ■   

9:
  ■ ■ ■   
■ ■   ■ ■ 
■ ■   ■ ■ 
  ■ ■ ■ ■ 
      ■ ■ 
■ ■   ■ ■ 
  ■ ■ ■   

10:
■   ■ ■ ■ 
■   ■   ■ 
■   ■   ■ 
■   ■   ■ 
■   ■ ■ ■ 
          
■ ■ ■ ■ ■ 

11:
  ■   ■   
  ■   ■   
  ■   ■   
  ■   ■   
  ■   ■   
          
■ ■ ■ ■ ■ 

12:
■   ■ ■ ■ 
■       ■ 
■   ■ ■ ■ 
■   ■     
■   ■ ■ ■ 
          
■ ■ ■ ■ ■ 

13:
■   ■ ■ ■ 
■       ■ 
■     ■ ■ 
■       ■ 
■   ■ ■ ■ 
          
■ ■ ■ ■ ■ 

14:
■   ■   ■ 
■   ■   ■ 
■   ■ ■ ■ 
■       ■ 
■       ■ 
          
■ ■ ■ ■ ■ 

15:
■   ■ ■ ■ 
■   ■     
■   ■ ■ ■ 
■       ■ 
■   ■ ■ ■ 
          
■ ■ ■ ■ ■ 

16:
■   ■ ■ ■ 
■   ■     
■   ■ ■ ■ 
■   ■   ■ 
■   ■ ■ ■ 
          
■ ■ ■ ■ ■ 

17:
■   ■ ■ ■ 
■       ■ 
■     ■   
■     ■   
■     ■   
          
■ ■ ■ ■ ■ 

18:
■   ■ ■ ■ 
■   ■   ■ 
■   ■ ■ ■ 
■   ■   ■ 
■   ■ ■ ■ 
          
■ ■ ■ ■ ■ 

19:
■   ■ ■ ■ 
■   ■   ■ 
■   ■ ■ ■ 
■       ■ 
■   ■ ■ ■ 
          
■ ■ ■ ■ ■ 

circle:
          
  ■ ■ ■   
■       ■ 
■       ■ 
■       ■ 
  ■ ■ ■   
          

cdot:
          
  ■ ■ ■   
■       ■ 
■   ■   ■ 
■       ■ 
  ■ ■ ■   
          

donut:
          
  ■ ■ ■   
■ ■ ■ ■ ■ 
■ ■   ■ ■ 
■ ■ ■ ■ ■ 
  ■ ■ ■   
          

ball:
          
  ■ ■ ■   
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
  ■ ■ ■   
          

square:
          
■ ■ ■ ■ ■ 
■       ■ 
■       ■ 
■       ■ 
■ ■ ■ ■ ■ 
          

sdot:
          
■ ■ ■ ■ ■ 
■       ■ 
■   ■   ■ 
■       ■ 
■ ■ ■ ■ ■ 
          

fbox:
          
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
          

sbox:
          
          
  ■ ■ ■   
  ■   ■   
  ■ ■ ■   
          
          

sfbox:
          
          
  ■ ■ ■   
  ■ ■ ■   
  ■ ■ ■   
          
          

bigpointerright:
  ■       
  ■ ■     
  ■   ■   
  ■     ■ 
  ■   ■   
  ■ ■     
  ■       

bigpointerleft:
      ■   
    ■ ■   
  ■   ■   
■     ■   
  ■   ■   
    ■ ■   
      ■   

arrowright:
  ■       
  ■ ■     
  ■   ■   
  ■     ■ 
  ■   ■   
  ■ ■     
  ■       

arrowleft:
      ■   
    ■ ■   
  ■   ■   
■     ■   
  ■   ■   
    ■ ■   
      ■   

ascprogress1:
■         
■         
■         
■         
■         
■         
■         
■         

ascprogress2:
■ ■       
■ ■       
■ ■       
■ ■       
■ ■       
■ ■       
■ ■       
■ ■       

ascprogress3:
■ ■ ■     
■ ■ ■     
■ ■ ■     
■ ■ ■     
■ ■ ■     
■ ■ ■     
■ ■ ■     
■ ■ ■     

ascprogress4:
■ ■ ■ ■   
■ ■ ■ ■   
■ ■ ■ ■   
■ ■ ■ ■   
■ ■ ■ ■   
■ ■ ■ ■   
■ ■ ■ ■   
■ ■ ■ ■   

fullprogress:
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 

descprogress1:
        ■ 
        ■ 
        ■ 
        ■ 
        ■ 
        ■ 
        ■ 
        ■ 

descprogress2:
      ■ ■ 
      ■ ■ 
      ■ ■ 
      ■ ■ 
      ■ ■ 
      ■ ■ 
      ■ ■ 
      ■ ■ 

descprogress3:
    ■ ■ ■ 
    ■ ■ ■ 
    ■ ■ ■ 
    ■ ■ ■ 
    ■ ■ ■ 
    ■ ■ ■ 
    ■ ■ ■ 
    ■ ■ ■ 

descprogress4:
  ■ ■ ■ ■ 
  ■ ■ ■ ■ 
  ■ ■ ■ ■ 
  ■ ■ ■ ■ 
  ■ ■ ■ ■ 
  ■ ■ ■ ■ 
  ■ ■ ■ ■ 
  ■ ■ ■ ■ 

ascchart1:
■ ■ ■ ■ ■ 
          
          
          
          
          
          
          

ascchart2:
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
          
          
          
          
          
          

ascchart3:
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
          
          
          
          
          

ascchart4:
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
          
          
          
          

ascchart5:
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
          
          
          

ascchart6:
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
          
          

ascchart7:
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
          

descchart1:
          
          
          
          
          
          
          
■ ■ ■ ■ ■ 

descchart2:
          
          
          
          
          
          
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 

descchart3:
          
          
          
          
          
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 

descchart4:
          
          
          
          
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 

descchart5:
          
          
          
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 

descchart6:
          
          
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 

descchart7:
          
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 

borderleft1:
        ■ 
        ■ 
        ■ 
        ■ 
        ■ 
        ■ 
        ■ 
        ■ 

borderleft2:
      ■ ■ 
      ■   
      ■   
      ■   
      ■   
      ■   
      ■   
      ■ ■ 

borderleft3:
    ■ ■ ■ 
    ■     
    ■     
    ■     
    ■     
    ■     
    ■     
    ■ ■ ■ 

borderleft4:
  ■ ■ ■ ■ 
  ■       
  ■       
  ■       
  ■       
  ■       
  ■       
  ■ ■ ■ ■ 

borderleft5:
■ ■ ■ ■ ■ 
■         
■         
■         
■         
■         
■         
■ ■ ■ ■ ■ 

bordertopbottom5:
■ ■ ■ ■ ■ 
          
          
          
          
          
          
■ ■ ■ ■ ■ 

borderright1:
■         
■         
■         
■         
■         
■         
■         
■         

borderright2:
■ ■       
  ■       
  ■       
  ■       
  ■       
  ■       
  ■       
■ ■       

borderright3:
■ ■ ■     
    ■     
    ■     
    ■     
    ■     
    ■     
    ■     
■ ■ ■     

borderright4:
■ ■ ■ ■   
      ■   
      ■   
      ■   
      ■   
      ■   
      ■   
■ ■ ■ ■   

borderright5:
■ ■ ■ ■ ■ 
        ■ 
        ■ 
        ■ 
        ■ 
        ■ 
        ■ 
■ ■ ■ ■ ■ 

box1:
      ■ ■ 
      ■ ■ 
      ■ ■ 
          
          
          
          

box2:
■ ■       
■ ■       
■ ■       
          
          
          
          

box3:
■ ■   ■ ■ 
■ ■   ■ ■ 
■ ■   ■ ■ 
          
          
          
          

box4:
          
          
          
          
      ■ ■ 
      ■ ■ 
      ■ ■ 

box5:
      ■ ■ 
      ■ ■ 
      ■ ■ 
          
      ■ ■ 
      ■ ■ 
      ■ ■ 

box6:
■ ■       
■ ■       
■ ■       
          
      ■ ■ 
      ■ ■ 
      ■ ■ 

box7:
■ ■   ■ ■ 
■ ■   ■ ■ 
■ ■   ■ ■ 
          
      ■ ■ 
      ■ ■ 
      ■ ■ 

box8:
          
          
          
          
■ ■       
■ ■       
■ ■       

box9:
      ■ ■ 
      ■ ■ 
      ■ ■ 
          
■ ■       
■ ■       
■ ■       

box10:
■ ■       
■ ■       
■ ■       
          
■ ■       
■ ■       
■ ■       

box11:
■ ■   ■ ■ 
■ ■   ■ ■ 
■ ■   ■ ■ 
          
■ ■       
■ ■       
■ ■       

box12:
          
          
          
          
■ ■   ■ ■ 
■ ■   ■ ■ 
■ ■   ■ ■ 

box13:
      ■ ■ 
      ■ ■ 
      ■ ■ 
          
■ ■   ■ ■ 
■ ■   ■ ■ 
■ ■   ■ ■ 

box14:
■ ■       
■ ■       
■ ■       
          
■ ■   ■ ■ 
■ ■   ■ ■ 
■ ■   ■ ■ 

box15:
■ ■   ■ ■ 
■ ■   ■ ■ 
■ ■   ■ ■ 
          
■ ■   ■ ■ 
■ ■   ■ ■ 
■ ■   ■ ■ 

euro:
      ■ ■ 
    ■     
■ ■ ■ ■   
  ■       
■ ■ ■ ■   
  ■       
    ■ ■ ■ 

cent:
          
          
  ■ ■ ■   
■       ■ 
■         
■   ■   ■ 
  ■ ■ ■   
  ■       

speaker:
        ■ 
      ■ ■ 
  ■ ■ ■ ■ 
  ■ ■ ■ ■ 
  ■ ■ ■ ■ 
      ■ ■ 
        ■ 

sound:
  ■       
■         
          
■ ■       
          
■         
  ■       

x:
          
■ ■   ■ ■ 
  ■ ■ ■   
    ■     
  ■ ■ ■   
■ ■   ■ ■ 
          

target:
          
  ■   ■   
■       ■ 
■   ■   ■ 
■       ■ 
  ■   ■   
          

pointerright:
          
  ■       
  ■ ■     
  ■ ■ ■   
  ■ ■     
  ■       
          

pointerup:
          
          
    ■     
  ■ ■ ■   
■ ■ ■ ■ ■ 
          
          

pointerleft:
          
      ■   
    ■ ■   
  ■ ■ ■   
    ■ ■   
      ■   
          

pointerdown:
          
          
■ ■ ■ ■ ■ 
  ■ ■ ■   
    ■     
          
          

arrowne:
          
  ■ ■ ■ ■ 
      ■ ■ 
    ■   ■ 
  ■     ■ 
■         
          

arrownw:
          
■ ■ ■ ■   
■ ■       
■   ■     
■     ■   
        ■ 
          

arrowsw:
          
        ■ 
■     ■   
■   ■     
■ ■       
■ ■ ■ ■   
          

arrowse:
          
■         
  ■     ■ 
    ■   ■ 
      ■ ■ 
  ■ ■ ■ ■ 
          

dice1:
          
          
          
    ■     
          
          
          

dice2:
          
■         
          
          
          
        ■ 
          

dice3:
          
■         
          
    ■     
          
        ■ 
          

dice4:
          
■       ■ 
          
          
          
■       ■ 
          

dice5:
          
■       ■ 
          
    ■     
          
■       ■ 
          

dice6:
          
■       ■ 
          
■       ■ 
          
■       ■ 
          

bell:
    ■     
  ■ ■ ■   
  ■ ■ ■   
  ■ ■ ■   
■ ■ ■ ■ ■ 
          
    ■     

smile:
          
  ■   ■   
          
■       ■ 
  ■ ■ ■   
          
          

note:
      ■   
      ■ ■ 
      ■   
  ■ ■ ■   
■ ■ ■ ■   
  ■ ■     
          

clock:
          
  ■ ■ ■   
■   ■   ■ 
■   ■ ■ ■ 
■       ■ 
  ■ ■ ■   
          

heart:
          
  ■   ■   
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
■ ■ ■ ■ ■ 
  ■ ■ ■   
    ■     
          

duck:
          
  ■ ■     
■ ■ ■   ■ 
  ■ ■ ■ ■ 
  ■ ■ ■ ■ 
    ■ ■   
          

check:
          
        ■ 
      ■ ■ 
■   ■ ■   
■ ■ ■     
  ■       
          

retarrow:
        ■ 
        ■ 
    ■   ■ 
  ■     ■ 
■ ■ ■ ■ ■ 
  ■       
    ■     

runninga:
    ■ ■   
    ■ ■   
    ■   ■ 
  ■ ■ ■   
■   ■     
    ■     
  ■   ■   
■       ■ 

runningb:
    ■ ■   
    ■ ■   
    ■     
  ■ ■ ■   
  ■ ■ ■   
    ■     
  ■   ■   
  ■   ■   
```
