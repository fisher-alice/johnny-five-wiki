## Prérequis

- Au moins une Arduino ou carte compatible (Uno, Mega, Leonardo, Fio, Pro, Pro Mini)
    - [Arduino UNO](http://arduino.cc/en/Main/arduinoBoardUno)
    - [Arduino Leonardo](http://arduino.cc/en/Main/arduinoBoardLeonardo)
    - [Arduino MEGA](http://arduino.cc/en/Main/arduinoBoardMega)
    - [Arduino FIO](http://arduino.cc/en/Main/ArduinoBoardFio)
    - [Arduino Pro](http://arduino.cc/en/Main/ArduinoBoardPro)
    - [Arduino Pro Mini](http://arduino.cc/en/Main/ArduinoBoardProMini)
    - [TinyDuino](http://tiny-circuits.com/products/tinyduino/)
- [Sparkfun Inventor's Kit](https://www.sparkfun.com/products/11576) (Recommandé pour commencer)

### OSX

- Installez Node.js 0.10.x
- Installez Xcode
- Installez node-gyp `npm install -g node-gyp`

### Windows 

Via @ThomasDeutsch sur https://github.com/rwldrn/johnny-five/issues/48#issuecomment-7696662

- Installez Node.js 0.10.x **32 bit** (à moins que quelqu'un peut me confirmer que cela fonctionne avec la version 64 bits)
- Installez Visual Studio Express 2010 32 bit (assurez-vous que vous avez vérifié les dépendances C++)
- Installez [Python 2.7.3](http://www.python.org/getit/releases/2.7.3/)
- Ouvrez cmd (Démarrer > Exécuter.. > cmd) et saisissez `set PATH=%PATH%;C:\Python27`
- Installez node-gyp `npm install -g node-gyp`

## Bonjour tout le monde

Généralement les cartes Arduino (Uno, Mega, Leonardo, Fio, Mini) sont pré-flashé avec le firmware StandardFirmata compilé. Dans la plupart des cas, la mise en route est aussi simple que ça :

```bash
mkdir nodebot && cd nodebot;

npm install johnny-five;
```

Maintenant, ouvrez votre éditeur de texte et créez un nouveau fichier appelé "strobe.js", tapez ou copiez le texte suivant dans ce fichier:

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

Assurez-vous que la carte est branchée sur votre machine hôte (ordinateur de bureau, ordinateur portable, raspberry pi, etc). Maintenant, dans votre terminal, tapez ou collez le texte suivant :

```js
node strobe.js
```

[Le résultat attendu devrait ressembler à ceci](http://jsfiddle.net/rwaldron/dtudh/show/light/)



## Dépannage

1. Si ce qui précède ne fonctionne pas comme prévu, assurez-vous que StandardFirmata est installée sur la carte :
    - Téléchargez [Arduino IDE](http://arduino.cc/en/main/software)
    - Branchez votre Arduino ou votre microcontrôleur compatible Arduino via USB
    - Ouvrez l'IDE de Arduino, selectionnez: Fichier > Exemples > Firmata > StandardFirmata
    - Cliquez sur le bouton "Télécharger".
    - Si la transmission a réussi, la carte est prête et vous pouvez fermer l'IDE d'Arduino.

2. Parfois, les systèmes Windows ne pourront pas compiler les dépendances natives, si vous rencontrez ce problème, essayez ce qui suit :
```bash
npm install johnny-five --msvs_version=2012
```