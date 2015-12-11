

In this tutorial we will learn how to set up a Raspberry Pi development board and get it to run a blinking LED loop in [johnny-five](https://github.com/rwaldron/johnny-five/) inside of [node-red](http://nodered.org/) and connect it to the Internet of Things.

## Install Raspbian

You will need the [Raspbian](https://www.raspberrypi.org/downloads/raspbian/) Jessie image installed on the pi.  The doc was written with the `2015-21-11` release.

You will also need to have network configured.  Wifi or wired ethernet will do.


## Update the init script.

As of this writing, node.js (version 0.10.29) and the node-red application come pre-installed on raspbian, however for us to use johnny-five we'll need to run node-red as the root user.

Edit the `/etc/init.d/nodred` script in your favorite text editor on the pi.
Switch the line containing `USER=pi` to `USER=root`


## Trying node-red

Run `sudo node-red-start` the open a web browser to `http://<YOUR PI's IP ADDRESS>:1880`

![node-red](http://nodered.org/images/node-red-screenshot-sm.png)

The startup also created a `/root/.node-red` folder which contains settings node-red to allow you to set an admin password as well as configure things like the port number.


## Adding npm

The raspbian image doesn't include npm, however it is easy to install:
`sudo apt-get update`

`sudo apt-get install npm`

Unfortunately this only gets us npm version 1.4.21 which wont work for us, but that's easily updatable:
`sudo npm i npm -g`


## Adding johnny-five

We'll need to add some packages to node-red for johnny-five in the root's .node-red directory:

`sudo su -`

`cd .node-red`

`npm i node-red-contrib-gpio`

That takes a couple of minutes to finish on a raspberry pi 2, and gets us the GPIO and johnny-five nodes.  At this point we could plug in an arduino uno to a USB port and get started, however let's add [raspi-io](https://github.com/nebrius/raspi-io) so that we can use the pi's [onboard pins](https://github.com/nebrius/raspi-io/wiki/Pin-Information).

`npm i raspi-io`

At this point we'll need to reboot.

`sudo reboot`

Then start node red up again.

`sudo node-red-start`

## Blinking an LED

Once again, point your browser to `http://<YOUR PI's IP ADDRESS>:1880`

Drop a johnny-five node into the node-red workspace and double click it.  Click the edit button to configure a new nodebot, and select Raspberry Pi as the nodebot type and click Add.

![add pi nodebot](https://github.com/monteslu/node-red-contrib-gpio/blob/pics/pics/add_pi_nodebot.png)

Then in the onReady code block you can do something like:

```javascript
var led = new five.Led('GPIO4');

led.blink(500);
```
Click the deploy button on the upper-right of node-red.

If you have an LED connected to GPIO4 it should start blinking!

## Let's add some Internet of Things messaging.

Node-red comes with things like websocket and MQTT nodes.  Another really cool set of nodes to add to help connect devices are the nodesfor the open source [meshblu](https://github.com/octoblu/meshblu) project.

From the same `/root/.node-red` directory, we can run:

`npm i node-red-contrib-meshblu`

This gets us some new input and output nodes for sending IoT messages.

![meshblu nodes](https://github.com/monteslu/node-red-contrib-meshblu/blob/master/screenshot.png)


## More johnny-five components.

The [node-red-contrib-gpio](https://github.com/monteslu/node-red-contrib-gpio) nodes already includes all of johnny-five, and there's a ton of other johnny-five [related libraries](https://github.com/rwaldron/johnny-five/wiki/Related-Libararies) we can easily include by installing them with npm.

Let's use [oled-js](https://github.com/noopkat/oled-js) for example.

`npm i oled-js oled-font-5x7`

Now in our johnny-five node we can do something like this:

```javascript
var Oled = require('oled-js');
var font = require('oled-font-5x7');

var opts = {
    width: 128,
    height: 64,
    address: 0x3C
};

var oled = new Oled(board, five, opts);
oled.turnOnDisplay();
oled.clearDisplay();

oled.setCursor(1, 1);
oled.writeString(font, 1, 'Hello, node red !', 1, true, 2);

```

![hello nodered](https://github.com/monteslu/node-red-contrib-gpio/blob/pics/pics/pi_oled.jpg)


## Doing something with IoT message data.

Perhaps we want incoming messages to do something to our nodebot.

We can use node-red to connect an inbound message node to the input of our johnny-five node.

![hello nodered](https://github.com/monteslu/node-red-contrib-gpio/blob/pics/pics/iot_j5.png)

In our johnny-five node we can listen for events and do something with the data, like writing it to our oled screen:

```javascript
node.on('input', function(msg){
    oled.clearDisplay();
    oled.setCursor(1, 1);
    oled.writeString(font, 1, String(msg.payload), 1, true, 2);
});
```

## Make a twitter button.

We can connect the output of our johnny-five node to any other type of node.

![hello nodered](https://github.com/monteslu/node-red-contrib-gpio/blob/pics/pics/j5_tweet.png)

Then just wire up a physical button to your board, handle button press events by passing a new message along:

```javascript
var button = new five.Button("GPIO4");

button.on("press", function() {
    node.send({payload: "johnny-five is awesome!"});
});
```
