> This guide was forked from Mashery's [Intel IoTDevKit Example Sketches](https://github.com/mashery/edison-guides/wiki/Cylon.js-and-Intel-IoTDevKit-Example-Sketches)

**Galileo-IO/Edison-IO will install a version of libmraa that is known to be stable and well tested.**

The purpose of this guide is to provide a working Intel XDK IoT Edition project with snippets to help you use 10 of the useful sensors which are packaged by default in the Seeed Studios IoT DevKit.

### Index

* [Sensor: Blink LED Example](Johnny-Five-and-Intel-IoTDevKit-Example-Sketches#blink-sketch)
* [Sensor: Light Sensor Example](Johnny-Five-and-Intel-IoTDevKit-Example-Sketches#light-sensor-sketch)
* [Sensor: Sound Sensor Example](Johnny-Five-and-Intel-IoTDevKit-Example-Sketches#sound-sensor-sketch)
* [Sensor: Buzzer Example](Johnny-Five-and-Intel-IoTDevKit-Example-Sketches#buzzer)
* [Sensor: Rotary Angle Example](Johnny-Five-and-Intel-IoTDevKit-Example-Sketches#rotary-angle-sensor)
* [Sensor: Temperature Sensor Example](Johnny-Five-and-Intel-IoTDevKit-Example-Sketches#temperature-sensor)
* [Sensor: LCD Example](Johnny-Five-and-Intel-IoTDevKit-Example-Sketches#lcd)
* [Sensor: Button(s) Example](Johnny-Five-and-Intel-IoTDevKit-Example-Sketches#buttons)
* [Sensor: Servo Example](Johnny-Five-and-Intel-IoTDevKit-Example-Sketches#servo)
* [Sensor: Relay Example](Johnny-Five-and-Intel-IoTDevKit-Example-Sketches#relay)

### Getting Started
1. Plug in and flash your Intel Edison
2. Make sure both your Intel Edison and computer running Intel XDK IoT Edition are on the same network
3. Download and install Intel XDK IoT Edition
4. git clone the Johnny-Five example [from this location](https://github.com/mashery/edison-guides/tree/master/recipies/Johnny-Five%20Examples)
5. Follow the instructions to get your Edison online
6. Add your computer to Intel Edison's whitelist
7. Open settings and ensure that NPM install is run automatically on your Intel Edison
8. Run "build," then run "start" to begin the blink sketch

### Running The Examples

Before running each example with Intel IoT XDK Edition, you must modify the "package.json" file to set the target script. The default is "blink.js." 

[All sketches may be found here](https://github.com/mashery/edison-guides/tree/master/recipies/Johnny-Five%20Examples)

### Blink Sketch

![](http://rexstjohn.com/wp-content/uploads/2015/02/IMG_1289.jpg)

Take out the LED sensor and plug in an LED, make sure the "long" wire goes in the outer slot marked "+."

![](http://rexstjohn.com/wp-content/uploads/2015/02/IMG_1288.jpg)

Plug the LED sensor into "pin 4." Change the "blink.js" script from pin 13 to pin 4 then upload and run to make the LED blink.

You can view the source code here: [blink.js](https://github.com/mashery/edison-guides/blob/master/recipies/Johnny-Five%20Examples/blink.js)

### Light Sensor Sketch

![](http://rexstjohn.com/wp-content/uploads/2015/02/IMG_1292.jpg)

![](http://rexstjohn.com/wp-content/uploads/2015/02/Screen-Shot-2015-02-04-at-4.56.30-PM.png)

Plug the light sensor into Analog slot 0. Modify the package.json to make the target file "light.js." 

You can view the source code here: [light.js](https://github.com/mashery/edison-guides/blob/master/recipies/Johnny-Five%20Examples/light.js)

### Sound Sensor Sketch

![](http://rexstjohn.com/wp-content/uploads/2015/02/sound.jpg)

![](http://rexstjohn.com/wp-content/uploads/2015/02/soundsensor.png)

Plug the sound sensor into Analog slot 0. Modify the package.json to make the target file "sound.js." 

**NOTE: This example also uses an Led component plugged into the D4 slot. This version is no shown in the photos above, but corresponds to the Johnny-Five version of the example.**

You can view the source code here: [sound.js](https://github.com/mashery/edison-guides/blob/master/recipies/Johnny-Five%20Examples/sound.js)

# Buzzer

![](http://rexstjohn.com/wp-content/uploads/2015/02/buzzer.jpg)

Plug the Buzzer into pin 4. Modify the package.json to make the target file "buzzer.js." 

**NOTE: This example requires the j5-songs module, which is installed by executing `npm install j5-songs`**

You can view the source code here: [buzzer.js](https://github.com/mashery/edison-guides/blob/master/recipies/Johnny-Five%20Examples/buzzer.js)

### Rotary Angle Sensor

![](http://rexstjohn.com/wp-content/uploads/2015/02/rotator.jpg)

![](http://rexstjohn.com/wp-content/uploads/2015/02/rotary.png)

Plug the rotary sensor into Analog slot 0. Modify the package.json to make the target file "rotary.js." 

You can view the source code here: [rotary.js](https://github.com/mashery/edison-guides/blob/master/recipies/Johnny-Five%20Examples/rotary.js)

### Temperature Sensor

![](http://rexstjohn.com/wp-content/uploads/2015/02/IMG_1294.jpg)

![](http://rexstjohn.com/wp-content/uploads/2015/02/temp.png)

Plug the temperature sensor into Analog slot 0. Modify the package.json to make the target file "temperature.js."

You can view the source code here: [temperature.js](https://github.com/mashery/edison-guides/blob/master/recipies/Johnny-Five%20Examples/temperature.js)

### LCD 

![](http://rexstjohn.com/wp-content/uploads/2015/02/LCD.jpg)

Plug the LCD sensor into the I2C slot shown in the image. Modify the package.json to make the target file "lcd-screen.js."

You can view the source code here: [lcd-screen.js](https://github.com/mashery/edison-guides/blob/master/recipies/Johnny-Five%20Examples/lcd-screen.js)

### Buttons

![](http://rexstjohn.com/wp-content/uploads/2015/02/buttonblack.jpg)

![](http://rexstjohn.com/wp-content/uploads/2015/02/buttontouch.jpg)

![](http://rexstjohn.com/wp-content/uploads/2015/02/touchrelease.png)

Plug the button (either the touch or black tipped button) into pin 4. Modify the package.json to make the target file "button.js."

You can view the source code here: [button.js](https://github.com/mashery/edison-guides/blob/master/recipies/Johnny-Five%20Examples/button.js)

### Servo

![](https://raw.githubusercontent.com/wiki/mashery/edison-guides/images/servomovie.gif)

![](https://raw.githubusercontent.com/wiki/mashery/edison-guides/images/servosetup.JPG)

![](https://raw.githubusercontent.com/wiki/mashery/edison-guides/images/servoangle.png)

Plug the Servo into Pin 3 (D3) on the shield. Remember to change the target in `package.json` to the file `servo.js`

You can view the source code here: [servo.js](https://github.com/mashery/edison-guides/blob/master/recipies/Johnny-Five%20Examples/servo.js)

### Relay

![](https://raw.githubusercontent.com/wiki/mashery/edison-guides/images/relaysetup.JPG)

![](https://raw.githubusercontent.com/wiki/mashery/edison-guides/images/relaystate.png)

Plug the Relay into Pin 4 (D4) on the shield. Remember to change the target in `package.json` to the file `relay.js`

You can view the source code here: [relay.js](https://github.com/mashery/edison-guides/blob/master/recipies/Johnny-Five%20Examples/relay.js)