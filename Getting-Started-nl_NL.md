## Benodigdheden

- Je hebt op zijn minst een Arduino of een vergelijkbaar bordje nodig (Uno, Mega, Leonardo, Fio, Pro, Pro Mini)
    - [Arduino UNO](http://arduino.cc/en/Main/arduinoBoardUno)
    - [Arduino Leonardo](http://arduino.cc/en/Main/arduinoBoardLeonardo)
    - [Arduino MEGA](http://arduino.cc/en/Main/arduinoBoardMega)
    - [Arduino FIO](http://arduino.cc/en/Main/ArduinoBoardFio)
    - [Arduino Pro](http://arduino.cc/en/Main/ArduinoBoardPro)
    - [Arduino Pro Mini](http://arduino.cc/en/Main/ArduinoBoardProMini)
    - [TinyDuino](http://tiny-circuits.com/products/tinyduino/)
- [Sparkfun Inventor's Kit](https://www.sparkfun.com/products/11576) (Word aangeraden als je net begint)

### OSX

- Installeer Node.js 0.10.x
- Installeer Xcode
- Installeer node-gyp `npm install -g node-gyp`

### Windows 

Door @ThomasDeutsch in https://github.com/rwldrn/johnny-five/issues/48#issuecomment-7696662

- Installeer Node.js 0.10.x **32 bit** (behalve als iemand 64 bit aan de praat heeft gekregen)
- Installeer Visual Studio Express 2010 32 bit (zorg ervoor dat de C++ afhankelijkheden zijn aan gevinkt)
- Installeer [Python 2.7.3](http://www.python.org/getit/releases/2.7.3/)
- Open cmd (Start > Run.. > cmd) en type `set PATH=%PATH%;C:\Python27`
- Installeer node-gyp `npm install -g node-gyp`

## Hallo Wereld

Over het algemeen zijn Arduino bordjes (Uno, Mega, Leonardo, Fio, Mini) uitgerust met een compileerde StandardFirmata firmware. In de meeste gevallen is beginnen zo simpel als...

```bash
mkdir nodebot && cd nodebot;

npm install johnny-five;
```

Open een tekst editor en maak een bestand "strobe.js". Type of plak daar het volgende in:

```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // maak een Led op pin 13
  var led = new five.Led(13);

  // Laat de pint aan en uit blinken met standaard tussenpozen van 100ms
  led.strobe();
});
```

Zorg ervoor dat het bordje is aangesloten op de host machine (desktop, laptop, raspberry pi, etcetera). Type of plan nu het volgende in een terminal:

```js
node strobe.js
```

[Als alles goed gaat ziet het er zo uit.](http://jsfiddle.net/rwaldron/dtudh/show/light/)



## Problemen oplossen

1. Als het bovenstaande niet het gewenste resultaat oplevert, zorg er dan voor dat StandardFirmate op het bordje is geinstalleerd:
    - Download [Arduino IDE](http://arduino.cc/en/main/software)
    - Sluit je Arduino of Arduino compatibele microcontroller aan via USB.
    - Open de Arduino IDE, selecteer: File > Examples > Firmata > StandardFirmata
    - Klik op de "Upload" knop.
    - Als het uploaden geslaagd is, kan de Arduino IDE gesloten worden. Het bordje is nu klaar gemaakt voor gebruik.

2. Het kan voorkomen dat Windows systemen afhankelijkheden niet correct kan compileren. Wanneer dit het geval is probeer dan:
```bash
npm install johnny-five --msvs_version=2012
```