## Pré requisitos

- Pelo menos um Arduino ou placa compatível (Uno, Mega, Leonardo, Fio, Pro, Pro Mini)
    - [Arduino UNO](http://arduino.cc/en/Main/arduinoBoardUno)
    - [Arduino Leonardo](http://arduino.cc/en/Main/arduinoBoardLeonardo)
    - [Arduino MEGA](http://arduino.cc/en/Main/arduinoBoardMega)
    - [Arduino FIO](http://arduino.cc/en/Main/ArduinoBoardFio)
    - [Arduino Pro](http://arduino.cc/en/Main/ArduinoBoardPro)
    - [Arduino Pro Mini](http://arduino.cc/en/Main/ArduinoBoardProMini)
    - [TinyDuino](http://tiny-circuits.com/products/tinyduino/)
	- [Sparkfun Inventor's Kit](https://www.sparkfun.com/products/11576) (Recomendado para iniciantes)

### OSX

- Instale o Node.js 0.10.x
- Instale o Xcode
- Instale o node-gyp `npm install -g node-gyp`

### Windows 

Via @ThomasDeutsch em https://github.com/rwldrn/johnny-five/issues/48#issuecomment-7696662

- Instale Node.js 0.10.x **32 bit** (a menos que alguém confirme que conseguiu rodar com sucesso na versão de 64 bit)
- Instale o Visual Studio Express 2010 32 bit (certifique-se de que você tem todas as dependências do C++ selecionadas)
- Instale o [Python 2.7.3](http://www.python.org/getit/releases/2.7.3/)
- Abra o cmd (Iniciar > Executar... > cmd) e digite: `set PATH=%PATH%;C:\Python27`
- Instale o node-gyp `npm install -g node-gyp`

## Olá Mundo

Geralmente, placas de Arduino (Uno, Mega, Leonardo, Fio, Mini) chegam com o StandardFirmata Firmware já instalados de fábrica. Na maioria dos casos, começar a programar é tão simples quanto:

```bash
mkdir nodebot && cd nodebot;

npm install johnny-five;
```

Agora abra seu editor de códigos favorito e crie um arquivo chamado "strobe.js", neste arquivo, digite ou cole o texto seguinte:

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

Certifique-se de que a placa esteja conectada em sua máquina (desktop, laptop, raspberry-pi etc.). Agora, em seu terminal (linha de comandos), digite ou cole o seguinte:


```js
node strobe.js
```

[Se tudo der certo, você deve ver algo parecido com isso](http://jsfiddle.net/rwaldron/dtudh/show/light/)



## Solucionando problemas

1. Se os passos acima não funcionaram como o esperado, certifique-se de que o StandardFirmata está instalado na placa:
    - Baixe o [Arduino IDE](http://arduino.cc/en/main/software)
    - Conecte seu Arduino ou microcontrolador compatível com Arduino via USB
    - Abra o Arduino IDE, selecione: Arquivo > Exemplos > Firmata > StandardFirmata *(File > Examples > Firmata > StandardFirmata)*
    - Clique no botão "Carregar" ("Upload").
    - Se o carregamento foi feito com sucesso, a placa está preparada e você pode fechar o Arduino IDE.

2. As vezes, sistemas baseados no Windows falham ao compilar dependências nativas, se isso acontecer com você, tente o seguinte:
```bash
npm install johnny-five --msvs_version=2012
```