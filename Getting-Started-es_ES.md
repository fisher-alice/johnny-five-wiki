## Prerequisitos

- Por lo menos un Arduino, o board compatible (Uno, Mega, Leonardo, Fio, Pro, Pro Mini)
    - [Arduino UNO](http://arduino.cc/en/Main/arduinoBoardUno)
    - [Arduino Leonardo](http://arduino.cc/en/Main/arduinoBoardLeonardo)
    - [Arduino MEGA](http://arduino.cc/en/Main/arduinoBoardMega)
    - [Arduino FIO](http://arduino.cc/en/Main/ArduinoBoardFio)
    - [Arduino Pro](http://arduino.cc/en/Main/ArduinoBoardPro)
    - [Arduino Pro Mini](http://arduino.cc/en/Main/ArduinoBoardProMini)
    - [TinyDuino](http://tiny-circuits.com/products/tinyduino/)
- [Sparkfun Inventor's Kit](https://www.sparkfun.com/products/11576?utm_source=j5) (Recomendado para principiantes)

### OSX

- Instalar Node.js 0.10.x
- Instalar Xcode
- Instalar node-gyp `npm install -g node-gyp`

### Windows

Via @ThomasDeutsch en https://github.com/rwldrn/johnny-five/issues/48#issuecomment-7696662

- Instalar Node.js 0.10.x **32 bit** (a no ser de que alguien pueda confirmar éxito con 64 bit)
- Instalar Visual Studio Express 2010 32 bit (asegúrese de tener las dependencias de C++ checkeadas)
- Instalar [Python 2.7.3](http://www.python.org/getit/releases/2.7.3/)
- Abrir cmd (Inicio > Ejecutar.. > cmd) e ingresar `set PATH=%PATH%;C:\Python27`
- Instalar node-gyp `npm install -g node-gyp`

## Hola Mundo

Generalmente los boards de Arduino (Uno, Mega, Leonardo, Fio, Mini) vienen con el firmware de FirmataStandard pre-instalado. En la mayoría de los casos es tan simple como...

```bash
mkdir nodebot && cd nodebot;

npm install johnny-five;
```

Ahora abra su editor de texto y cree un archivo nuevo llamado "strobe.js", en este archivo escriba o pegue lo siguiente:

```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // Crear un Led en el pin 13
  var led = new five.Led(13);

  // Led intermitente en el pin, usa fases de 100ms como valor predeterminado
  led.strobe();
});
```

Asegúrese de que el board esté conectado a su máquina (computador de escritorio, laptop, raspberry pi, etc). Ahora, en su terminal, escriba o pegue lo siguiente:

```js
node strobe.js
```

[Un programa exitoso se vería así](http://jsfiddle.net/rwaldron/dtudh/show/light/)



## Solución de Problemas

1. Si lo anterior no funcionó como esperaba, asegúrese de que StandardFirmata se encuentre instalado en el board:
    - Descargue el [IDE de Arduino](http://arduino.cc/es/Main/Software)
    - Conecte su Arduino, o microcontrolador compatible con Arduino, vía USB
    - Abra el IDE de Arduino, seleccione: Archivo > Ejemplos > Firmata > StandardFirmata
    - Haga click en el boton "Subir".
    - Si la transmisión fue exitosa, el board se encuentra listo y puede cerrar el IDE de Arduino

2. Algunas veces los sistemas Windows fallarán compilando dependencias nativas, si se encuentra con este problema intente lo siguiente:

```bash
npm install johnny-five --msvs_version=2012
```
