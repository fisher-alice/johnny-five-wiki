The JY-MCU Bluetooth Serial Port module defaults to 9600 or 38400 baud depending on the firmware loaded on it. Since Firmata runs at 57600 baud, you'll need to reconfigure the module before making a connection with Johnny-Five. If you've ever dealt with an early dialup modem, you'll feel right at home as this is done by sending AT commands to the module.

## Step 1: Connect the JY-MCU module to the Arduino for configuration

We will program the Arduino to send AT commands to the module to configure it via a SoftwareSerial connection. Wire the TX and RX pins of your module to your Arduino. They need wired in a crossover configuration, so from the module to the Arduino wire TX to pin 10 and RX to pin 11.

![Fritzing Diagram](http://i.imgur.com/xLAbKup.png)

Then upload the following Sketch to your Arduino which creates a connection between the Arduino's serial port and the JY-MCU. Modify the ROBOT_NAME and the BLUETOOTH_SPEED values before uploading if you want a custom name or have changed the baudrate before. This is a slightly modified version of the Sketch found here: [https://gist.github.com/garrows/f8f787dac6e85591737c#file-setupbluetooth-ino](https://gist.github.com/garrows/f8f787dac6e85591737c#file-setupbluetooth-ino)

```Arduino

#define ROBOT_NAME "RandomBot"

// If you haven't configured your device before use this
#define BLUETOOTH_SPEED 9600
// If you are modifying your existing configuration, use this:
// #define BLUETOOTH_SPEED 57600

#include <SoftwareSerial.h>

// Swap RX/TX connections on bluetooth chip
//   Pin 10 --> Bluetooth TX
//   Pin 11 --> Bluetooth RX
SoftwareSerial mySerial(10, 11); // RX, TX


/*
  The posible baudrates are:
    AT+BAUD1-------1200
    AT+BAUD2-------2400
    AT+BAUD3-------4800
    AT+BAUD4-------9600 - Default for hc-06
    AT+BAUD5------19200
    AT+BAUD6------38400
    AT+BAUD7------57600 - Johnny-five speed
    AT+BAUD8-----115200
    AT+BAUD9-----230400
    AT+BAUDA-----460800
    AT+BAUDB-----921600
    AT+BAUDC----1382400
*/


void setup()
{
  Serial.begin(9600);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for Leonardo only
  }
  Serial.println("Starting config");
  mySerial.begin(BLUETOOTH_SPEED);
  delay(1000);

  // Should respond with OK
  mySerial.print("AT");
  waitForResponse();

  // Should respond with its version
  mySerial.print("AT+VERSION");
  waitForResponse();

  // Set pin to 0000
  mySerial.print("AT+PIN0000");
  waitForResponse();

  // Set the name to ROBOT_NAME
  mySerial.print("AT+NAME");
  mySerial.print(ROBOT_NAME);
  waitForResponse();

  // Set baudrate to 57600
  mySerial.print("AT+BAUD7");
  waitForResponse();

  Serial.println("Done!");
}

void waitForResponse() {
    delay(1000);
    while (mySerial.available()) {
      Serial.write(mySerial.read());
    }
    Serial.write("\n");
}

void loop() {}
```

## Step 2: Check config

The setup() function will take about 6 seconds to run. You can connect to the Arduino with Serial Monitor and you should see the following output.

```
Starting config
OK
OKlinvorV1.8
OKsetPIN
OKsetname
OK57600
Done!
```

If you saw that congratulations, you're done this step.

If you see fhe following output instead, you will probably have to change BLUETOOTH_SPEED to 57600 and upload it again. This is because the JY-MCU's chip ia already running at a different baud rate.
```
Starting config





Done!

```

If you are having troubles uploading the firmata firmware to the device, make sure that nothing is connected to pins 0 and 1 when uploading as this can interfere with the upload process.

More information about the AT commands which the module will respond to, can be found at the following links:

[http://byron76.blogspot.com/2011/09/one-board-several-firmwares.html](http://byron76.blogspot.com/2011/09/one-board-several-firmwares.html)

[http://byron76.blogspot.com/2011/09/hc05-firmware.html](http://byron76.blogspot.com/2011/09/hc05-firmware.html)

[http://forum.arduino.cc/index.php?topic=101452.msg774290#msg774290](http://forum.arduino.cc/index.php?topic=101452.msg774290#msg774290)

[http://mcuoneclipse.com/2013/06/19/using-the-hc-06-bluetooth-module/](http://mcuoneclipse.com/2013/06/19/using-the-hc-06-bluetooth-module/)

## Step 3: Reupload StandardFirmata

Once the baud rate is properly set, re upload the StandardFirmata sketch to your board. If you don't do this it might seems that your bluetooth module is getting a connection, the green light will stop blinking, but you won't  be able to connect.

## Step 4: Wire the module to the Arduino's hardware port

Once the baud rate is properly set & Firmate reloaded, connect the TX and RX pins to Arduino pins 0 and 1 (same crossover style configuration as before).

![Fritzing Diagram](http://i.imgur.com/fjMCXVx.png)


## Step 5 : Pair the module

Pair to the module from your host device, once you have paired with your bluetooth device the serial port should be visible with the 'ROBOT_NAME' you used in Step 1. It will be something like /dev/tty.ROBOT_NAME-DevB. Use this name to [tell Johnny-Five which port to use](https://github.com/rwldrn/johnny-five/blob/master/docs/board-with-port.md)

## Step 6: Profit!

Your NodeBot has been _unleashed!_ Go forth and build!


---
_*Thanks to Patrik Thalin for the JY-MCU Fritzing part which is [available here](http://www.thalin.se/2013/01/fritzing-veroboard-and-breadboard.html)._