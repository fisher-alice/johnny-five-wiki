The JY-MCU Bluetooth Serial Port module defaults to 9600 or 38400 baud depending on the firmware loaded on it. Since Firmata runs at 57600 baud, you'll need to reconfigure the module before making a connection with Johnny-Five. If you've ever dealt with an early dialup modem, you'll feel right at home as this is done by sending AT commands to the module.

## Step 1: Establish a serial connection to the JY-MCU module

If you've got an FTDI cable/interface you can connect directly to the module using that. BAM! Step 1 done!

If not, just use your Arduino to make the connection. First, wire the TX and RX pins of your module to your Arduino. They need wired in a crossover configuration, so from the module to the Arduino wire TX to pin 10 and RX to pin 11.

![Fritzing Diagram](http://i.imgur.com/xLAbKup.png)

Then upload the following Sketch to your Arduino which creates a connection between the Arduino's serial port and the JY-MCU. This is a slightly modified version of the Sketch found here: [http://arduino.cc/en/Reference/SoftwareSerial](http://arduino.cc/en/Reference/SoftwareSerial)

```c
#include <SoftwareSerial.h>

SoftwareSerial mySerial(10, 11); // RX, TX

void setup()  
{
  // Open serial communications and wait for port to open:
  Serial.begin(57600);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for Leonardo only
  }

  Serial.println("Ready to forward AT commands to JY-MCU: ");

  // set the data rate for the SoftwareSerial port
  mySerial.begin(38400); // Or 9600, depending on your firmware version
}

void loop() // run over and over
{
  if (mySerial.available())
    Serial.write(mySerial.read());
  if (Serial.available())
    mySerial.write(Serial.read());
}
```

## Step 2: Set your terminal to a character delay of 5ms

The JY-MCU Bluetooth module is too fast for you to simply type out the AT command in most terminal software, so you'll need to copy & paste in the full command you intend to run.

But ironically, the module is also too slow to respond to characters at full speed! So you'll need to use a terminal program which allows you to configure the delay time between characters. Many blog articles and forum posts suggest a 3ms character delay, but you may need to increase it to 5ms or more.

Suggestions for terminals which can do this:

 * Windows: [RealTerm](http://realterm.sourceforge.net/)
 * Cross-platform: [CoolTerm](http://freeware.the-meiers.org/) is [confirmed to be working on OSX](http://forum.arduino.cc/index.php?topic=110504.msg830034#msg830034). 

## Step 3: Connect and send some AT commands!

Once you've connected to the appropriate port send just `AT` and the JY-MCU should respond with `OK`.

If you're curious about the specific version of firmware on your module, issue an `AT+VERSION` command.

The command to change the baud rate is `AT+BAUDx` where `x` is a hexadecimal number 1-C representing the desired rate from 1200-1382400bps. To set the baud to 57600, issue `AT+BAUD7`.

More information, including more AT commands which the module will respond to, can be found at the following links:

[http://byron76.blogspot.com/2011/09/one-board-several-firmwares.html](http://byron76.blogspot.com/2011/09/one-board-several-firmwares.html)

[http://byron76.blogspot.com/2011/09/hc05-firmware.html](http://byron76.blogspot.com/2011/09/hc05-firmware.html)

[http://forum.arduino.cc/index.php?topic=101452.msg774290#msg774290](http://forum.arduino.cc/index.php?topic=101452.msg774290#msg774290)

## Step 4: Wire the module to the Arduino's hardware port

Once the baud rate is properly set, connect the TX and RX pins to Arduino pins 0 and 1 (same crossover style configuration as before).

![Fritzing Diagram](http://i.imgur.com/fjMCXVx.png)

Pair to the module from your host device, [tell Johnny-Five which port to use](https://github.com/rwldrn/johnny-five/blob/master/docs/board-with-port.md) and...

## Step 4: Profit!

Your NodeBot has been _unleashed!_ Go forth and build!


---
_*Thanks to Patrik Thalin for the JY-MCU Fritzing part which is [available here](http://www.thalin.se/2013/01/fritzing-veroboard-and-breadboard.html)._