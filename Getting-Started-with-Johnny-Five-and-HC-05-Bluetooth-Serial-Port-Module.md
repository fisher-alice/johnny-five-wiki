The HC-05 Bluetooth Serial Port module works different from HC-06 (JY MCU). Firmata runs at 57600 baud, you'll need to reconfigure the module before making a connection with Johnny-Five, this is done by sending AT commands to the module.

## Step 1: Connect the HC-05 module to the Arduino for configuration

We will program the Arduino to send AT commands to the module to configure it via a SoftwareSerial connection. Wire the TX and RX pins of your module to your Arduino. They need wired in a crossover configuration, so from the module to the Arduino wire TX to pin 10 and RX to pin 11.

![Fritzing Diagram](http://i.imgur.com/xLAbKup.png)

Then upload the following Sketch to your Arduino which creates a connection between the Arduino's serial port and the HC-05. Modify the ROBOT_NAME and the BLUETOOTH_SPEED values before uploading if you want a custom name or have changed the baudrate before. HC-05 has defaults baudrate (38400).

To put the HC-05 in AT commands mode you must connect the KEY pin in an arduino pin (I use pin 9) this is because this module works different.

```c

#define ROBOT_NAME "RandomBot"

// If you haven't configured your device before use this
#define BLUETOOTH_SPEED 38400 //This is the default baudrate that HC-05 uses
// If you are modifying your existing configuration, use this:
// #define BLUETOOTH_SPEED 57600

#include <SoftwareSerial.h>

// Swap RX/TX connections on bluetooth chip
//   Pin 10 --> Bluetooth TX
//   Pin 11 --> Bluetooth RX
SoftwareSerial mySerial(10, 11); // RX, TX

/*
  The possible baudrates are:
    AT+UART=1200,0,0 -------1200
    AT+UART=2400,0,0 -------2400
    AT+UART=4800,0,0 -------4800
    AT+UART=9600,0,0 -------9600 - Default for hc-06
    AT+UART=19200,0,0 ------19200
    AT+UART=38400,0,0 ------38400
    AT+UART=57600,0,0 ------57600 - Johnny-five speed
    AT+UART=115200,0,0 -----115200
    AT+UART=230400,0,0 -----230400
    AT+UART=460800,0,0 -----460800
    AT+UART=921600,0,0 -----921600
    AT+UART=1382400,0,0 ----1382400
*/

void setup() {
  pinMode(9, OUTPUT);  // this pin will pull the HC-05 pin 34 (key pin) HIGH to switch module to AT mode
  digitalWrite(9, HIGH);
  Serial.begin(9600);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for Leonardo only
  }
  Serial.println("Starting config");
  mySerial.begin(BLUETOOTH_SPEED);
  delay(1000);

  // Should respond with OK
  mySerial.print("AT\r\n");
  waitForResponse();

  // Should respond with its version
  mySerial.print("AT+VERSION\r\n");
  waitForResponse();

  // Set pin to 0000
  mySerial.print("AT+PSWD=0000\r\n");
  waitForResponse();

  // Set the name to ROBOT_NAME
  String rnc = String("AT+NAME=") + String(ROBOT_NAME) + String("\r\n"); 
  mySerial.print(rnc);
  waitForResponse();

  // Set baudrate to 57600
  mySerial.print("AT+UART=57600,0,0\r\n");
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
OK[VERSION]
OK
OK
OK
Done!
```

If you saw that congratulations, you're done this step.

If you see the following output instead, you will probably have to change BLUETOOTH_SPEED to another value and upload it again. This maybe could be because the HC-05 chip had different baud rate.
```
Starting config





Done!

```

If you are having troubles uploading the firmata firmware to the device, make sure that nothing is connected to pins 0 and 1 when uploading as this can interfere with the upload process.

If you want the supported commands for HC-05 

[https://www.mediafire.com/?bss12s6zaljcl6o](HC-05 datasheet and AT commands)

## Step 3: Reupload StandardFirmata

Once the baud rate is properly set, re upload the StandardFirmata sketch to your board. If you don't do this it might seems that your bluetooth module is getting a connection, the green light will stop blinking, but you won't  be able to connect.

## Step 4: Wire the module to the Arduino's hardware port

Once the baud rate is properly set & Firmata reloaded, connect the TX and RX pins to Arduino pins 0 and 1 (same crossover style configuration as before).

![Fritzing Diagram](http://i.imgur.com/fjMCXVx.png)

## Step 5 : Pair the module

Pair to the module from your host device, once you have paired with your bluetooth device the serial port should be visible with the 'ROBOT_NAME' you used in Step 1. It will be something like /dev/tty.ROBOT_NAME-DevB (in UNIX) and use COMX in Windows (where X is the number of the port). Use this name to [tell Johnny-Five which port to use](https://github.com/rwldrn/johnny-five/blob/master/docs/board-with-port.md)

## Step 6: Profit!

Your NodeBot has been _unleashed!_ Go forth and build!

---
_*Thanks to Patrik Thalin for the JY-MCU Fritzing part which is [available here](http://www.thalin.se/2013/01/fritzing-veroboard-and-breadboard.html)._

If you have problems using the BT module ask in johnny-five gist