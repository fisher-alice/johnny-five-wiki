A classe `Button` constrói objetos que representam um simples botão digital ligado a placa física.

### Parâmetros

- **pin** Um endereço númerico ou String para o pino do botão (digital).
```js
var button = new five.Button(5);
```
TinkerKit: 
```js
// Ligado ao TinkerKit's "Input 0"
var button = new five.Button("I0");
```


- **options** Um objeto com propriedades.
<table>
  <thead>
    <tr>
      <th>Propriedade</th>
      <th>Tipo</th>
      <th>Valor(es)</th>
      <th>Descrição</th>
      <th>Obrigatório</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>pin</td>
      <td>Number, String</td>
      <td>5, "I1" (Qualquer pino digital da placa)</td>
      <td>O endereço numérico ou String do pino ao qual o botão está ligado, ie. 5 ou "I1"</td>
      <td>sim</td>
    </tr>
    <tr>
      <td>invert</td>
      <td>Boolean</td>
      <td>true ou false</td>
      <td>Inverte os valores máximo e mínimo</td>
      <td>não</td>
    </tr>
    <tr>
      <td>isPullup</td>
      <td>Boolean</td>
      <td>true ou false</td>
      <td>Inicializa como um botão pullup</td>
      <td>não</td>
    </tr>
    <tr>
      <td>holdtime</td>
      <td>Number</td>
      <td>milisegundos</td>
      <td>Número de milisegundos que o botão deve ser pressionado até emitir um evento "hold". O valor padrão é 500ms</td>
      <td>não</td>
    </tr>    
  </tbody>
</table>
```js
// Um botão básico
// 
//   - ligado ao pino 5
//   - emite um evento down/press
//
var button = new five.Button(5);

button.on("press", function() {
  console.log( "O botão foi pressionado" );
});
```

### Formato

```
{ 
  id: Um identificador definido pelo usuário. O padrão é um identificador gerado aleatoriamente
  pin: O endereço do pino que o botão está ligado
  
  downValue: 0 ou 1, depende de invert ou pullup
  upValue: 0 ou 1, depende de invert ou pullup
  holdtime: milisegundos
}
```



### Uso
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  // Cria uma nova instância `button`.
  var button = new five.Button(5);

  button.on("hold", function() {
    console.log( "Botão segurado" );
  });

  button.on("press", function() {
    console.log( "Botão pressionado" );
  });

  button.on("release", function() {
    console.log( "Botão liberado" );
  });

});
```


## Eventos

- **hold** O botão foi segurado por `holdtime` milisegundos

- **down**, **press** O botão foi pressionado

- **up**, **release** O botão foi liberado


## Exemplos

- [Button](https://github.com/rwldrn/johnny-five/blob/master/docs/button.md)
- [Button Bumper](https://github.com/rwldrn/johnny-five/blob/master/docs/button-bumper.md)
- [Button Options](https://github.com/rwldrn/johnny-five/blob/master/docs/button-options.md)
- [Button Pullup](https://github.com/rwldrn/johnny-five/blob/master/docs/button-pullup.md)
- [Tinkerkit Button](https://github.com/rwldrn/johnny-five/blob/master/docs/tinkerkit-button.md)