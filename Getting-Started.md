## Prerequisites

- At least an Arduino or compatible board (Uno, Mega, Leonardo, Fio, Pro, Pro Mini)
    - [Arduino UNO](http://arduino.cc/en/Main/arduinoBoardUno)
    - [Arduino Leonardo](http://arduino.cc/en/Main/arduinoBoardLeonardo)
    - [Arduino MEGA](http://arduino.cc/en/Main/arduinoBoardMega)
    - [Arduino FIO](http://arduino.cc/en/Main/ArduinoBoardFio)
    - [Arduino Pro](http://arduino.cc/en/Main/ArduinoBoardPro)
    - [Arduino Pro Mini](http://arduino.cc/en/Main/ArduinoBoardProMini)
    - [TinyDuino](http://tiny-circuits.com/products/tinyduino/)
- [Sparkfun Inventor's Kit](https://www.sparkfun.com/products/11576?utm_source=j5) (Recommended for getting started)

### OSX

- Install Node.js >= 0.10.x
- Install Xcode
- Install node-gyp `npm install -g node-gyp`

### Windows

Via @ThomasDeutsch on https://github.com/rwldrn/johnny-five/issues/48#issuecomment-7696662

- Install <a href="https://nodejs.org" target="_blank">Node.js</a> >= 0.10.x **32 bit** (unless anyone can confirm success with 64 bit)
- `npm --add-python-to-path install --global --production windows-build-tools`
- Install node-gyp `npm install -g node-gyp`

### Ubuntu and Debian

- Install Node.js >= 0.10.x ```apt-get install nodejs```
- Install the nodejs-legacy package ```apt-get install nodejs-legacy```
- Install build-essential or a suitable alternative ```apt-get install build-essential```

### Arch Linux

- Install Node.js ```pacman -S nodejs```
- Install Arduino Libraries (for firmware flashing) ```pacman -S arduino```

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



## Troublehooting

**Firmware**

The StandardFirmataPlus firmware is the one that is used for Johnny-Five to communicate with the board.
That means you have to install it first, then you can execute the nodejs programs.
**Arduiono IDE**
- Open [Arduino IDE](http://arduino.cc/en/main/software)
- Verify correct port and board
- Navigate to File > Examples > Firmata > StandardFirmata
- Load sketch onto board.

**Packaged**
- Install `arduino` package on your operating system ).
- Make a `firmware` folder and save [this firmware.ino](https://github.com/firmata/arduino/blob/master/examples/StandardFirmataPlus/StandardFirmataPlus.ino) into it. if the link is dead again and not appearing in the Arduino IDE, use [this gist backup](https://gist.github.com/cookiengineer/4f292c952209e0f74d4c18b995dac855).
- Install arduino libraries via `arduino --install-library "Firmata,Servo"` in the Terminal.
- Flash the arduino board via `arduino --board "arduino:avr:uno" --upload ./path/to/firmware/firmware.ino`. Remember to change your board according to what you use. See below on how to figure out that identifier.
- If the upload was successful, the board is now prepared for johnny-five usage.

**Finding out your Board identifier for arduino-tools**

- Go to the [package index file](https://github.com/arduino/Arduino/blob/master/hardware/package_index_bundled.json) of the Arduino tools.
- Download the `url` entry of the package that contains your boards, for example `http://downloads.arduino.cc/cores/avr-1.6.18.tar.bz2`.
- Inside the archive, there's a `boards.txt` file that contains all supported boards. These boards can be used as the last part of the identifier. For example, the boards.txt lists `yun` meaning the `arduino --board "arduino:avr:yun"` has to be used.


**List of Arduino Board identifiers (May 2017)**

This is a compiled list that may not be up-to-date. Use the method described above in case you can't find your board here.

- "arduino:avr:yun" for Arduino Yun
- "arduino:avr:uno" for Arduino/Genuino Uno
- "arduino:avr:diecimila" for Arduino Duemilanove or Diecimila
- "arduino:avr:nano" for Arduino Nano
- "arduino:avr:mega" for Arduino/Genuio Mega or Mega 2560
- "arduino:avr:megaADK" for Arduino Mega ADK
- "arduino:avr:leonardo" for Arduino Leonardo
- "arduino:avr:leonardoeth" for Arduino Leonardo ETH
- "arduino:avr:micro" for Arduino/Genuino Micro
- "arduino:avr:esplora" for Arduino Esplora
- "arduino:avr:mini" for Arduino Mini
- "arduino:avr:ethernet" for Arduino Ethernet
- "arduino:avr:fio" for Arduino Fio
- "arduino:avr:bt" for Arduino BT
- "arduino:avr:LilyPadUSB" for LilyPad Arduino USB
- "arduino:avr:lilypad" for LilyPad Arduino
- "arduino:avr:pro" for Arduino Pro
- "arduino:avr:atmegang" for Arduino NG or older
- "arduino:avr:robotControl" for Arduino Robot Control
- "arduino:avr:robotMotor" for Arduino Robot Motor
- "arduino:avr:gemma" for Arduino Gemma
- "arduino:avr:circuitplay32u4cat" for Arduino Circuit Playground
- "arduino:avr:yunmini" for Arduino Yun Mini
- "arduino:avr:chiwawa" for Arduino Industrial 101
- "arduino:avr:one" for Linino One
- "arduino:avr:unowifi" for Arduino Uno WiFi


**Other**

Sometimes Windows systems will fail to compile native dependencies, if you run across this case try:
```bash
npm install johnny-five --msvs_version=2012
```