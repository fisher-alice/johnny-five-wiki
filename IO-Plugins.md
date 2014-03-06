(This document is an incomplete draft)

An IO plugin is any class whose instances implement a [Firmata](https://github.com/jgautier/firmata) compatible interface. 

### Minimum Plugin Class Requirements

The plugin must...

- Be a constructor function that defines a prototype object
- Be a subclass of [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter)
- Initialize instances that must...
    - asynchronously emit a "connected" event when the connection to a physical device has been made.
    - asynchronously emit a "ready" event when the handshake to the physical device is complete.
    - include a property named `isReady` whose initial value is `false`. `isReady` must be set to `true` in the same or previous execution turn as the the "ready" event is emitted.
        - The process of establishing a connection and becoming "ready" is irrelevant to this document's purposes.
    - include a readonly property named `MODES` whose value is a frozen object containing the following property/values: `{ INPUT: 0, OUTPUT: 1, ANALOG: 2, PWM: 3, SERVO: 4 }` 
    - include a readonly property named `pins` whose value is an array of pin configuration objects. The indices of the `pins` array must correspond to the pin address integer value, eg. on an Arduino UNO digital pin 0 is at index 0 and analog pin 0 is index 14. See [mock-pins.js](https://github.com/rwaldron/johnny-five/blob/master/test/mock-pins.js) for a complete example.
        - each pin configuration object must contain the following properties and values: 
            - `supportedModes`: an array of modes supported by this pin, eg. 
                - `[0, 1, 2]` represents INPUT, OUTPUT, ANALOG. (Analog pin outs)
                - `[0, 1, 4]` represents INPUT, OUTPUT, SERVO.  (Digital pin outs) 
                - `[0, 1, 3, 4]` represents INPUT, OUTPUT, PWM, SERVO.  (Digital PWM pin outs)
            - `mode`: the current mode this pin is set to.
            - `value`: the current value of this pin 
                - INPUT mode: property updated via the read loop
                - OUTPUT mode: property updated via *Write methods
            - `report`: 1 if reporting, 0 if not reporting
            - `analogChannel`: corresponding analogPin index (127 if none), eg. `analogChannel: 0` is `A0` whose index is `14` in the `pins` array.
    - include a readonly property named `analogPins` whose value is an array of pin indices that correspond to the analog pin indices in the `pins` array. 

### Minimum API Requirements

**pinMode(pin, mode)**
- Set the mode of a specified pin to one of: 
```
INPUT: 0
OUTPUT: 1
ANALOG: 2
PWM: 3
SERVO: 4
```

**analogWrite(pin, value)**
- Ensure pin mode is OUTPUT
- Ensure PWM capability
- Accept an 8 bit value (0-255) to write

**digitalWrite(pin, value)**
- Ensure pin mode is OUTPUT
- Write HIGH/LOW (single bit: 1 or 0)

**analogRead(pin, handler)**
- Create a `data` event stream, emitting an event approximately once per millisecond per pin.
- A corresponding "analog-read-${pin}" event is also emitted

**digitalRead(pin, handler)**
- Create a `data` event stream, emitting an event approximately once per millisecond per pin.
- A corresponding "digital-read-${pin}" event is also emitted

**servoWrite**
TODO.