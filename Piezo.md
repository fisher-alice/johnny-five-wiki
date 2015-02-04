The `Piezo` class constructs objects that represent a single piezo component attached to the physical board.

## Parameters

- **pin** A Number or String address for the Piezo (+) pin (digital):
  ```js
  var piezo = new five.Piezo(3);
  ```

- **options** An object of property parameters.
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
        <td>pin</td>
        <td>Number</td>
        <td>Any Digital Pin</td>
        <td>The Number address of the pin the + input your piezo is attached to</td>
        <td>yes</td>
      </tr>
    </tbody>
  </table>

## Shape

```
{ 
  board: A reference to the board object the Led is attached to
  id: A user definable id value. Defaults to null
  pin: The pin address that the Led is attached to
  mode: Mode the piezo's pin is set to: output (1). READONLY
  isPlaying: Boolean: is the piezo currently playing?
}
```

## Component Initialization

```js
var piezo = new five.Piezo({
  pin: 3
});
```

![piezo diagram](https://raw.githubusercontent.com/rwaldron/johnny-five/master/docs/breadboard/piezo.png)


## Usage

```js
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  // Create a standard `piezo` instance on pin 3
  var piezo = new five.Piezo(3);

  // Plays a song
  piezo.play({
    // song is composed by an array of pairs of notes and beats
    // The first argument is the note (null means "no note")
    // The second argument is the length of time (beat) of the note (or non-note)
    song: [
      ["C4", 1 / 4],
      ["D4", 1 / 4],
      ["F4", 1 / 4],
      ["D4", 1 / 4],
      ["A4", 1 / 4],
      [null, 1 / 4],
      ["A4", 1],
      ["G4", 1],
      [null, 1 / 2],
      ["C4", 1 / 4],
      ["D4", 1 / 4],
      ["F4", 1 / 4],
      ["D4", 1 / 4],
      ["G4", 1 / 4],
      [null, 1 / 4],
      ["G4", 1],
      ["F4", 1],
      [null, 1 / 2]
    ],
    tempo: 100
  });
});
```

## API

- **frequency(frequency, duration)** Play tone at `frequency` (in Hz) for `duration` milliseconds.

  ``` js
  piezo.frequency(587, 1000); // Play note d5 for 1 second
  ```

- **play(tune, callback)** Play a `tune` with an optional callback to invoke when the tune is done playing. The `tune` object contains a `song` (an array of the notes in the `tune`) and optional settings (e.g. `tempo`):

  ``` js
  piezo.play({
    tempo: 150, // Beats per minute, default 150
    song: [ // An array of notes that comprise the tune
      [ "c4", 1 ], // Each element is an array in which 
                   // [0] is the note to play and 
                   //[1] is the duration in "beats" (tempo, above)
      [ "e4", 2 ],
      [ "g4", 3 ],
      [ null, 4 ] // null indicates "no tone" for the beats indicated
    ]
  });
  ```

  The notes you can use are defined in `lib/piezo.js` as `Piezo.Notes`:

  ```js
  Piezo.Notes = {
    "c4": 262,
    "c#4": 277,
    "d4": 294,
    "d#4": 311,
    "e4": 330,
    "f4": 349,
    "f#4": 370,
    "g4": 392,
    "g#4": 415,
    "a4": 440,
    "a#4": 466,
    "b4": 494,
    "c5": 523,
    "c#5": 554,
    "d5": 587,
    "d#5": 622,
    "e5": 659,
    "f5": 698,
    "f#5": 740,
    "g5": 784,
    "g#5": 831,
    "a5": 880,
    "a#5": 932,
    "b5": 988,
    "c6": 1047
  };
  ```

  You can also use frequencies directly if you'd like:

  ``` js
  piezo.play({
    song: [
      [ 698, 1 ], // Play frequency 698 for 1 beat
      [ 831, 2 ] // ...
    ]
  });
  ```

- **tone(frequency, duration)** Play a `tone` for `duration` ms. The `tone` value in this case is a computed duty cycle (in microseconds). This is a lower-level method than `frequency` (which does the translation from frequency to tone for you). Most of the time you likely want to use `frequency`.

- **noTone** Stop tone playing from Piezo. Will immediately stop playing a tone (`digitalWrite` the output pin low) and clear any existing queued tone timers.

- **off** Alias of `noTone`

## Events

Piezo objects are output only and therefore do not emit any events.

<!--remove-start-->

## Examples
- [Piezo](https://github.com/rwldrn/johnny-five/blob/master/docs/piezo.md)

<!--remove-end-->