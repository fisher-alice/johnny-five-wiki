(This document is an incomplete draft)

An IO plugin is any class whose instances implement a [Firmata](https://github.com/jgautier/firmata) compatible interface. 

The plugin must...

- Be a constructor function that defines a prototype object
- Be a subclass of [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter)
- Initialize instances that must...
    - asynchronously emit a "connected" event when the connection to a physical device has been made.
    - asynchronously emit a "ready" event when the handshake to the physical device is complete.
    - include a property named `isReady` whose initial value is `false`. `isReady` must be set to `true` when the 
    - include a readonly property named `MODES` whose value is a frozen object containing the following property/values: `{ INPUT: 0, OUTPUT: 1, ANALOG: 2, PWM: 3, SERVO: 4 }` 
    - include a readonly property named `pins` whose value is an array of pin configuration objects. The indices of the `pins` array must correspond to the pin address integer value, eg. on an Arduino UNO digital pin 0 is at index 0 and analog pin 0 is index 14. 
        - each pin configuration object must contain the following properties and values: 
            - supportedModes: an array of modes supported by this pin, eg. `[0, 1, 2]` represents INPUT, OUTPUT, ANALOG. 
            - mode: the current mode this pin is set to.
            - value: the current value of this pin 
                - INPUT mode: property updated via the read loop
                - OUTPUT mode: property updated via *Write methods
            - report: 1 if reporting, 0 if not reporting
            - analogChannel: corresponding analogPin index (127 if none)

    - include a readonly property named `analogPins` whose value is an array of pin indices that correspond to the analog pin indices in the `pins` array. 



analogPins

{
  supportedModes: [0, 1, 2],
  mode: 1,
  value: 0,
  report: 1,
  analogChannel: 12
}

**Minimum Requirements**