The `Animation` class constructs objects that represent a single Animation. An Animation consists of a target and an array of segments.
A **target** is the device or group of devices that are being animated. 

A **segment** is a short modular animation sequence (i.e. sit, stand, walk, etc). Segments are synchronous and run first-in, first-out.

Servos and Servo.Arrays may be animated. If you have a use case for another device type, open an issue.

You should be familiar with the API of the device(s) you will be animating. You don't have to call the device methods directly but if you haven't used them before this will be frustrating.

### Parameters
**target** A Servo, Servo.Array or list of devices. Optional, but if not passed here it must be passed as a property of each animation segment.
```js
// Animate a single servo instance
var servo = new five.Servo(9);
var animation = new five.Animation(servo);
```
```js
// Independently animate the servos in a Servo.Array
var servos = new five.Servo.Array([9,10,11]);
var animation = new five.Animation(servos);
```
```js
// Animate a Servo and Servo.Array. When passed as the member of an array, the 
// Servo.Array will be passed the same value for all devices in the Servo.Array
var servo = new five.Servo(9);
var servos = new five.Servo.Array(10,11]);
var animation = new five.Animation([servo, servos]);
```
### Usage

Single Servo
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var servo = new five.Servo(9),
    animation = new five.Animation(servo);

  // Create an animation segment object
  var tictoc = {
    duration: 2000,
    cuePoints: [0, 0.25, 0.5, 0.75, 1.0],
    keyFrames: [ 0, 135, 45, 180, 0]
  };

  animation.enqueue(tictoc);

});
```

Servo.Array
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var servos = new five.Servo.Array([9, 10, 11]),
    animation = new five.Animation(servos);

  // Create an animation segment object
  var grip = {
    duration: 2000,
    cuePoints: [0, 0.5, 1.0],
    keyFrames: [ 
      [0, 135, 180],
      [0, 90, 180],
      [0, 45, 180]
    ]
  };

  animation.enqueue(grip);

});
```

## API

- **enqueue(segment object)** Add a segment to the animation's queue. See the list of available segment properties below.
```js
var five = require("johnny-five"), 
    board = new five.Board();

board.on("ready", function() {

  var servo = new five.Servo(9),
    animation = new five.Animation(servo);

  // Create an animation object
  var sweep = {
    duration: 2000,
    cuePoints: [0, 0.25, 0.5, 0.75, 1.0],
    keyFrames: [ 0, 135, 45, 180, 0]
  };

  animation.enqueue(sweep);

});
```

- **play()** Play the animation. Animation's are set to play by default and this only needs to be called if the animation has been paused or a segment's speed property was set to 0.

- **pause()** Pause an animation while retaining the current progress and segment queue. 

- **stop()** Immediately stop the animation and flush the segment queue.

- **next()** Jump to the next segment in the queue. This is called automatically when a segment completes and in most cases should not be called by the user.

- **speed([Number speed])** Set the currentSpeed of the animation. If no value is passed, the currentSpeed is returned.

## Segment Properties
 * **target**: Overrides the target passed when the Animation was created
 * **duration**: Duration of the segment in milliseconds (default 1000)
 * **cuePoints**: Array of values from 0.0 to 1.0 representing the beginning and end of the animation respectively (default [0, 1])
 * **keyFrames**: A 1 or 2 dimensional array of device positions over time. See more on keyFrames below. (required)
 * **easing**: An easing function from ease-component to apply to the playback head on the timeline. See the [ease-component docs](https://www.npmjs.org/package/ease-component) for a list of available easing functions (default: "linear") 
 * **loop**: When true, segment will loop until animation.next() or animation.stop() is called (default: false)
 * **loopback**: The cuePoint that the animation will loop back to. If the animation is playing in reverse, this is the point at which the animation will "loop back" to 1.0 (default: 0.0)
 * **metronomic**: Will play to cuePoint[1] then play in reverse to cuePoint[0]. If the segment is set to loop then the animation will play back and forth until next(), pause() or stop() are called (default: false)
 * **progress**: The starting point for the playback head (default 0.0)
 * **currentSpeed**: Controls the speed of the playback head and scales the calculated duration of this and all subsequent segments until it is changed by another segment or a call to the speed() method (default: 1.0)
 * **fps**: The maximum frames per second for the segment (default: 60)
 * **onstart**: function to execute when segment is started (default: none)
 * **onpause**: function to execute when segment is paused (default: none)
 * **onstop**: function to execute when animation is stopped (default: none)
 * **oncomplete**: function to execute when segment is completed (default: none)
 * **onloop**: function to execute when segment loops (default: none)

## keyFrame Arrays

If a single device is being animated, keyFrames may be a single dimensional array. If more than one device is being animated it must be 2-dimensional (an array of arrays). We call this a "keyFrame set". The index of each device in the target maps to the same index in a keyFrame set so the length of the two should be identical.

Each keyFrame array should have an element that maps to each cue point in the cuePoints array. keyFrames[0][0] for example represents the position of the first device in your animation target at the first cuePoint. These may or may not be the same length. If there are fewer elements in a keyFrame array than in the cuePoints array, then the keyFrame array will be right padded with null values.

Elements in a keyFrame array represent a device's position at the corresponding cuePoint. Positions can be described in a number of ways:

###A Number
This is a step in degrees from the previous cuePoint's position.
```
// servo.last.degrees === 90
... cuePoints: [0, 0.25, 0.5, 0.75, 1], keyFrames : [ -45, 90, -135, 20, 70 ], ...
```
![keyFrames as a number](https://github.com/dtex/johnny-five/blob/animation/assets/Animation-Graph(1).png)

###null
The behavior of null varies depending upon its position in the array. If used in the first position, it will adopt the device's current value at the time the animation segment is played.
```
// servo.last.degrees === 90
... cuePoints: [0, 0.25, 0.5, 0.75, 1], keyFrames : [ null, 90, -135, 20, 70 ], ...
```
![null at beginning](https://github.com/dtex/johnny-five/blob/animation/assets/Animation-Graph(2).png)
- - -
If used at the end of the array it will copy the previous known value.
```
// servo.last.degrees === 90
... cuePoints: [0, 0.25, 0.5, 0.75, 1], keyFrames : [ -45, 90, -135, 20, null ], ...
```
![null at end](https://github.com/dtex/johnny-five/blob/animation/assets/Animation-Graph(3).png)
- - -
If used between two keyFrames this cuePoint will be ignored for this device and the value will be a tween of the previous and next known values.
```
// servo.last.degrees === 90
... cuePoints: [0, 0.25, 0.5, 0.75, 1], keyFrames : [ -45, 90, null, 20, -155 ], ...
```
![null in middle](https://github.com/dtex/johnny-five/blob/animation/assets/Animation-Graph(4).png)

###false
Will copy the previous known value (don't move the device)
```
// servo.last.degrees === 90
... cuePoints: [0, 0.25, 0.5, 0.75, 1], keyFrames : [ -45, 90, false, 20, -155 ], ...
```
![false as a keyFrame](https://github.com/dtex/johnny-five/blob/animation/assets/Animation-Graph(5).png)

###A keyFrame object
The available properties for keyFrame objects are:

**step**: A step in degrees from the previous cuePoint position.
```
// servo.last.degrees === 90
... cuePoints: [0, 0.25, 0.5, 0.75, 1], keyFrames : [ -45, {step: 90}, -135, 20, 70 ], ...
```
![step property](https://github.com/dtex/johnny-five/blob/animation/assets/Animation-Graph(1).png)

**degrees**: The servo position in degrees.
```
// servo.last.degrees === 90
... cuePoints: [0, 0.25, 0.5, 0.75, 1], keyFrames : [ -45, {degrees: 180}, -135, 20, 70 ], ...
```
![degrees property](https://github.com/dtex/johnny-five/blob/animation/assets/Animation-Graph(6).png)

**easing**: An easing function from ease-component to apply to the tweened value of the previous and next keyFrames. See the [ease-component docs](https://www.npmjs.org/package/ease-component) for a list of available easing functions (default: "linear")
```
// servo.last.degrees === 90
... cuePoints: [0, 0.25, 0.5, 0.75, 1], keyFrames : [ -45, {degrees: 180, easing: "inOutCirc", -135, 20, 70 ], ...
```
![easing property](https://github.com/dtex/johnny-five/blob/animation/assets/Animation-Graph(7).png)

**copyDegrees**: An index from this keyFrames array from which we copy the calculated or explicitly set degrees value.
```
// servo.last.degrees === 90
... cuePoints: [0, 0.25, 0.5, 0.75, 1], keyFrames : [ -45, 90, -135, { copyDegrees: 1 }, -45 ], ...
```
![copyDegrees property](https://github.com/dtex/johnny-five/blob/animation/assets/Animation-Graph(8).png)

**copyFrame**: An index from this keyFrames array from which we copy all of the properties.
```
// servo.last.degrees === 90
... cuePoints: [0, 0.25, 0.5, 0.75, 1], keyFrames : [ -45, 90, -135, { copyFrame: 1 }, 70 ], ...
```
![copyFrame property](https://github.com/dtex/johnny-five/blob/animation/assets/Animation-Graph(9).png)

**position**: A two or three tuple defining a coordinate in 2d or 3d space.
```
// two-tuple
...  cuePoints: [0, 0.5, 1], keyFrames : [ { position: [10, 10 ] }, { position: [20, 50 ] }, { position: [10, 10 ] } ] ...

// three-tuple
...  cuePoints: [0, 0.5, 1], keyFrames : [ { position: [10, 10, 0 ] }, { position: [20, 50, 90 ] }, { position: [10, 10, 0 ] } ] ...
```


## Examples
- [Animation](https://github.com/rwldrn/johnny-five/blob/master/docs/animation.md)
- [Phoenix](https://github.com/rwldrn/johnny-five/blob/master/docs/phoenix.md)

## Additional Tips

 * Use the servos' isInverted property to keep movements on the left and right side moving in the same direction when passed the same value.
 * Use the servos' offset property so that 90 degrees on one is the same as 90 degrees on corresponding servos.
 * Divide animations into small reusable components. These will be your segments.
 * Use onstop functions when your looping animation is halted to return a bot's animated limbs to their home positions. In other words, when an animation stops make sure your bot is ready for the next segment, whatever it may be.
 * Use loopback properties to give you a place for prep moves that occur before a looping segment. For example, when walking the first step is half the size of the looping steps.
 * Nearly always use null as the first value in an animation segment. It allows the segment to be started from a variety of positions.
 * For most walking bots a step cannot be accomplished by turning a single servo. Joints must work together to keep your end effector (foot) in a stable position on the floor. Turning a single servo will force effectors to work against each other and make your robot look clumsy.
 * Use an easing function on your segments. It gives your bot more natural movement.
 * Easing functions can be applied to the segment timeline AND keyframes. They are additive. Experiment with this.
 * Avoid having your end effector impact the ground with significant force. Doing so will eventually strip your servo gears.

["Key frames" on Wikipedia](https://en.wikipedia.org/wiki/Key_frame)
