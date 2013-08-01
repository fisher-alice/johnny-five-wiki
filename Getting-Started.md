## Prerequisites

- An Arduino or compatible board (Uno, Mega, Leonardo, Fio, Mini)
    - [Sparkfun Inventor's Kit](https://www.sparkfun.com/products/11576) (Recommended for getting started)

### OSX

- Install Node.js 0.10.x
- Install Xcode
- Install node-gyp `npm install -g node-gyp`

### Windows 

Via @ThomasDeutsch on https://github.com/rwldrn/johnny-five/issues/48#issuecomment-7696662

- Install Node.js 0.10.x **32 bit** (unless anyone can confirm success with 64 bit)
- Visual Studio Express 2010 32 bit (make sure you have the C++ dependencies checked)
- [Python 2.7.3](http://www.python.org/getit/releases/2.7.3/)
- Open up cmd (Start > Run.. > cmd) and enter `set PATH=%PATH%;C:\Python27`
- Install node-gyp `npm install -g node-gyp`

## Hello World

Generally Arduino boards (Uno, Mega, Leonardo, Fio, Mini) come pre-flashed with the compiled StandardFirmata firmware. In most cases, getting started is as simple as...

```bash
mkdir nodebot && cd nodebot;

npm install johnny-five;
```

Now open your text editor and create a new file called "strobe.js", in that file type or paste the following:

```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // Create an Led on pin 13
  var led = new five.Led(13);

  // Strobe the pin on/off, defaults to 100ms phases
  led.strobe();
});
```

Make sure the board is plugged into your host machine (desktop, laptop, raspberry pi, etc). Now, in your terminal, type or paste the following:

```js
node strobe.js
```

[Success should look like this](http://jsfiddle.net/rwaldron/dtudh/show/light/)



## Trouble Shooting

1. If the above didn't work as expected, make sure that StandardFirmata is installed on the board:
    - Download [Arduino IDE](http://arduino.cc/en/main/software)
    - Plug in your Arduino or Arduino compatible microcontroller via USB
    - Open the Arduino IDE, select: File > Examples > Firmata > StandardFirmata
    - Click the "Upload" button.
    - If the upload was successful, the board is now prepared and you can close the Arduino IDE.

