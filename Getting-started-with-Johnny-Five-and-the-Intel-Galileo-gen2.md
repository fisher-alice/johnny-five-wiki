

In this tutorial we will learn how to set up an Intel Galileo development board and get it to run a blinking LED loop in [johnny-five](https://github.com/rwaldron/johnny-five/).

> **NOTE:** You will need a 16 or 32GB micro SD card for your Galileo to complete this tutorial

## Update firmware
Before we get started we want to make sure that we have the latest firmware installed on our Galileo. This process can be different depending on the Operating System you are using on your development machine. Therefore Intel provides detailed instructions for this process for each supported OS.

* [OSX](https://communities.intel.com/docs/DOC-22885)
* [Windows](https://communities.intel.com/docs/DOC-22872)
* [Linux](https://communities.intel.com/docs/DOC-22886)

> **NOTE:** If you get the error message `Target Firmware Version Query Failed` it usually means that the Galileo is not available on the selected serial (USB) port in the IDE. Sometimes it may happen if there is a sketch running on the board.
Try rebooting the board and your computer, then connect the USB cable and re-select the correct serial port in the IDE.

## Installing Yocto Linux Image
Before we can get johnny-five to run on the board, we first need to download the Yocto Linux image for the Galileo and install it on our micro SD card; you can download the latest image [here](http://iotdk.intel.com/images/iot-devkit-latest-mmcblkp0.direct.bz2)

Once the image is downloaded we will need to extract it and install it on our micro SD card. This process once again can be different depending on your OS.

* [OSX](https://software.intel.com/en-us/node/530415)
* [Windows](https://software.intel.com/en-us/node/530353)
* [Linux](https://software.intel.com/en-us/node/532598)

## Getting remote access to your board
> **Note:** If you are on Windows , you can use [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) to make ssh connections.

Now that we have Yocto Linux Installed on the micro SD card and have booted our board with it, it's time to get access to it.

There are a few [options](https://learn.sparkfun.com/tutorials/galileo-getting-started-guide#using-the-terminal) when it comes to getting terminal access to the Galileo. But by far the easiest way is just to plug in an ethernet cable and access it via SSH.

To do this we first need to obtain the IP address of the Galileo. This can be found using applications like [fing](http://www.overlooksoft.com/fing) or [Bonjour Browser](http://hobbyistsoftware.com/bonjourbrowser).

![Bonjour Galileo IP address](http://i.imgur.com/DkJnObd.png)

Now that we have our IP, all we need to do is type
```bash
ssh root@x.x.x.x
```

We should now have a root terminal session on the Galileo

> **Tip:** To exit the ssh session and get back to your local terminal type `exit`


## Setting the date
It's always a good idea to be sure that the OS on the board has it's date set correctly, especially when using npm. You can very easily get the date from another system on your network by typing:
```bash
date -u `ssh username@x.x.x.x date -u '+%m%d%H%M%Y.%S'`
```
where x.x.x.x is a system with the correct date

> **NOTE:** If you don't have access to such a machine , you can also follow [this guide](https://github.com/janunezc/GALILEO/wiki/Set-System-Date-and-Time) on setting the date for the Galileo.

## Running a blink example

The Yocto Linux image already has node installed so all that's left to do is write some Johnny-Five code, push it to the board and then run it.

We will do this in a folder on our local machine and then copy it to the Galileo to run. You can either set up the Galileo with an LED on pin 13 or you can just watch the built in LED labeled `L` right next to the USB port on the board. This example is the same as the standard Johnny-Five blink example, except it specifies the use of the `galileo-io` IO-Plugin so it will run directly on the board.

Let's create an *index.js* with the following code:
```js
var five = require("johnny-five");
var Galileo = require("galileo-io");
var board = new five.Board({
  io: new Galileo()
});

board.on("ready", function() {
  var led = new five.Led(13);
  led.blink(500);
});
```

And now we can copy *index.js* over to the board using scp:
```bash
scp index.js root@x.x.x.x:~
```

Now ssh back into the board and let's run our code.
First we run `npm install johnny-five galileo-io` to install our dependencies, and then `node index.js` to run our program.

We should now have a blinking light on pin 13!

## Further reading

* [Can't update Galileo firmware](https://communities.intel.com/message/237438)
* [On-Board: Intel Galileo Programming with JavaScript and Node.js](http://bocoup.com/weblog/intel-galileo-javascript-nodejs/)
