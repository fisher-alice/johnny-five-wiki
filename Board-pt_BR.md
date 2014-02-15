A classe `Board` constrói objetos que representam a própria placa fisicamente. Todos os objetos do dispositivo dependem de um objeto de placa inicializado e pronto.

Johnny-Five foi testado em, mas não está limitado às, seguintes placas:

- [Arduino UNO](http://arduino.cc/en/Main/arduinoBoardUno)
- [Arduino Leonardo](http://arduino.cc/en/Main/arduinoBoardLeonardo)
- [Arduino MEGA](http://arduino.cc/en/Main/arduinoBoardMega)
- [Arduino FIO](http://arduino.cc/en/Main/ArduinoBoardFio)
- [Arduino Pro](http://arduino.cc/en/Main/ArduinoBoardPro)
- [Arduino Pro Mini](http://arduino.cc/en/Main/ArduinoBoardProMini)
- [Arduino Nano](http://arduino.cc/en/Main/arduinoBoardNano)
- [TinyDuino](http://tiny-circuits.com/products/tinyduino/)

Veja também: [Suporte Multi Placas](https://github.com/rwldrn/johnny-five/wiki/Boards)

### Parâmetros

- **opções** Objeto opcional e parâmetros opcionais.
<table>
  <thead>
    <tr>
      <th>Nome da Propriedade</th>
      <th>Tipo</th>
      <th>Valor(es)</th>
      <th>Descrição</th>
      <th>Obrigatório</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>Número, String</td>
      <td>Qualquer</td>
      <td>Identificação definida pelo usuário</td>
      <td>não</td>
    </tr>
    <tr>
      <td>port</td>
      <td>String ou objeto</td>
      <td>"/dev/ttyAM0", "COM1", new SerialPort()</td>
      <td>Caminho ou nome da porta/COM ou objeto SerialPort</td>
      <td>não</td>
    </tr>
  </tbody>
</table>

### Formato
```js
{ 
  ready: ...Um valor booleano que indica se a placa fisica está pronta
  firmata: ...Uma referência para a camada de protocolo
  id: ...Um valor de identificação definido pelo usuário. Padrão gera um id único
  pins: ...Um array de todos os pinos, resultado da query de capacidades
  type: ...Uma string identificando o tipo da placa, pode ter os seguintes valores: "UNO", "MEGA", "LEONARDO" or "UNKOWN"
  repl: ...Referência para o objeto repl session
  port: Uma string ilustrando o caminho do dispositivo ou endereço COM
}
```

### Inicializando a Placa

O jeito mais fácil de inicializar o objeto placa é chamando a função construtora `Board` precedida de um `new`. Não se preocupe em saber o caminho do seu dispositivo ou porta COM, o Johnny-Five irá descobrir em qual porta USB a placa está conectada e irá se conectar a ela automaticamente.

```js
var five = require("johnny-five"),
    board = new five.Board();
```

Você pode, opcionalmente, especificar a porta fornecendo ela como uma propriedade do parâmetro do objeto:

```js
var five = require("johnny-five"),
    board = new five.Board({ port: "/dev/tty.usbmodemNNNN" });
```

ou 

```js
var five = require("johnny-five"),
    board = new five.Board({ port: "COM1" });
```

ou você pode especificar o objeto SerialPort fornecendo-o como uma propriedade do parâmetro do objeto:

```js
var SerialPort = require("serialport").SerialPort;
var five = require("johnny-five");
var board = new five.Board({
  port: new SerialPort("COM4", {
    baudrate: 9600,
    buffersize: 1
  })
});
```


### Placa Pronta

Uma vez que o objeto da placa foi inicializado, ele deve ser conectado a placa fisica através de uma série de passos simples, uma vez que eles foram completados, a placa está pronta para comunicar com o programa. Esse processo é assíncrono, e enviado ao programa através do evento `ready`.

```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // The board can now communicate with this program.
});
```

### Uso

Um exemplo básico, mas completo, do uso do construtor `Board`:

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

## API

- **repl** Esta é a propriedade do objeto da placa que representa o REPL automáticamente gerado que é criado quando um programa Johnny-Five é executado. Esse objeto possui um método `inject` que pode ser chamado quantas vezes for necessário.

- **repl.inject(object)** Injeta objetos ou valores, do programa para a sessão REPL.
```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // Initialize an Led object that can be controlled from the REPL session
  this.repl.inject({
    led: new five.Led(13)
  });  
});
/*
  From the terminal...

  > node program.js
  1375291815062 Board Connecting... 
  1375291815081 Serial Found possible serial port /dev/whatever-this-port-is
  1375291815082 Board -> Serialport connected /dev/whatever-this-port-is
  1375291816115 Board <- Serialport ready /dev/whatever-this-port-is
  1375291816116 Repl Initialized 
  >> 
  (Since the led object is available here...)
  >> led.on();
  >> led.off();
  
*/
```

- **pinMode(pin, mode)** Define o modo `mode` de um pino `pin` específico, pode ter um dos valores seguintes: INPUT, OUTPUT, ANALOG, PWM, SERVO.
Constantes de modo estão expostas através da classe `Pin`
<table>
  <thead>
    <tr>
      <th>Modo</th>
      <th>Valor</th>
      <th>Constante</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>INPUT</td>
      <td>0</td>
      <td>Pin.INPUT</td>
    </tr>
    <tr>
      <td>OUTPUT</td>
      <td>1</td>
      <td>Pin.OUTPUT</td>
    </tr>
    <tr>
      <td>ANALOG</td>
      <td>2</td>
      <td>Pin.ANALOG</td>
    </tr>
    <tr>
      <td>PWM</td>
      <td>3</td>
      <td>Pin.PWM</td>
    </tr>
    <tr>
      <td>SERVO</td>
      <td>4</td>
      <td>Pin.SERVO</td>
    </tr>
  </tbody>
</table>
```js
// Set a pin to INPUT mode
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  
  // pin mode constants are available on the Pin class
  this.pinMode(13, five.Pin.INPUT);
});
```

- **analogWrite(pin, value)** Write an analog value (0-255) to a digital `pin`.
```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // Assuming an Led is attached to pin 9, this will turn it on at full brightness
  // PWM is the mode used to write ANALOG signals to a digital pin
  this.pinMode(9, five.Pin.PWM);
  this.analogWrite(9, 255);
});
```

- **analogRead(pin, handler(voltage))** Registra uma função para ser chamada quando a placa reportar o valor de voltagem (0-1023) do pino `pin` especificado.
```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // Assuming a sensor is attached to pin "A1"
  this.pinMode(1, five.Pin.ANALOG);
  this.analogRead(1, function(voltage) {
    console.log(voltage);
  });
});
```

- **digitalWrite(pin, value)** Escreve um valor digital (0 ou 1) para um pino `pin` digital.
```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // Assuming an Led is attached to pin 13, this will turn it on
  this.pinMode(13, five.Pin.OUTPUT);
  this.digitalWrite(13, 1);
});
```

- **digitalRead(pin, handler(value))** Registra uma função para ser chamada quando a placa informar o valor (0 ou 1) do pino `pin` especificado.
```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // Assuming a button is attached to pin 9
  this.pinMode(9, five.Pin.INPUT);
  this.digitalRead(9, function(value) {
    console.log(value);
  });
});
```

- **shiftOut(dataPin, clockPin, isBigEndian, value)** Escreve um byte no `dataPin`, seguido de uma alteração no `clockPin`.[Referência: Understanding Big and Little Endian Byte Order](http://betterexplained.com/articles/understanding-big-and-little-endian-byte-order/)

- **wait(ms, handler())** Registra uma função para ser chamada uma vez no próximo turno de execução e assim que o tempo especificado em milissegundos passar.
```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  // Assuming an Led is attached to pin 13
  this.pinMode(13, five.Pin.OUTPUT);

  // Turn it on...
  this.digitalWrite(13, 1);

  this.wait(1000, function() {
    // Turn it off...
    this.digitalWrite(13, 0);
  });
});
```

- **loop(ms, handler())** Registra uma função para ser chamada repetidamente em outro turno de execução, sempre que o tempo especificado em milissegundos passar. 
```js
var five = require("johnny-five"),
    board = new five.Board();

board.on("ready", function() {
  var byte;

  // Assuming an Led is attached to pin 13
  this.pinMode(13, five.Pin.OUTPUT);

  // Homemade strobe
  this.loop(500, function() {
    this.digitalWrite(13, (byte ^= 0x01));
  });
});
```

## Exemplos

- [Placa](https://github.com/rwldrn/johnny-five/blob/master/docs/board.md)
- [Placa com Porta](https://github.com/rwldrn/johnny-five/blob/master/docs/board-with-port.md)
- [Multi Placas](https://github.com/rwldrn/johnny-five/blob/master/docs/board-multi.md)
- [Repl](https://github.com/rwldrn/johnny-five/blob/master/docs/repl.md)