# Hardware:
* [Shield for the Arduino](https://www.sparkfun.com/products/10854)
* [Explorer for your Computer](https://www.sparkfun.com/products/8687)
* [XBee](https://www.sparkfun.com/products/11215) x2

Or
* [XBee Kit](https://www.sparkfun.com/products/11445)

# Software:
* [XbeeConfigTool](https://www.dropbox.com/s/hiqv52whj1ak8av/XBeeConfigTool.zip)

# Setting up the XBees
1. Open the XbeeConfigTool zip and start the XbeeConfigTool. Assuming you're on a Mac, open application.macosx and open the XBeeConfigTool app.
1. Connect one of your XBees to your machine with the XBee Explorer.
![XBee Explorer](https://raw.githubusercontent.com/jweakley/gobot/master/pictures/xbeeExplorer.JPG)
1. Select the port that corresponds to your XBee.
1. Select __programming mode__ for this XBee.
1. Select 57600 baud.
1. Leave all the id stuff alone.
1. Click __configure__.
1. When it's all done unplug the first XBee. This will be the one that will be connected to your computer.
1. Plug the second XBee into the explorer and re-open the app (sometimes the port list doesn't refresh).
1. Select the port that corresponds to your XBee.
1. Select __Arduino fio radio__ mode for this XBee.
1. Select 57600 baud.
1. The __My ID__ field should've incremented by one on its own.
1. Click __configure__.
1. When it's all done configuring unplug the second XBee, plug it into whatever shield you're using on the arduino.
![Arduino Shield with XBee](https://raw.githubusercontent.com/jweakley/gobot/master/pictures/xbeeShield.JPG)
1. Plug the first XBee back into the explorer. There is now a link between these two and they will communicate.

# Gotchas
* Make sure that the UART/Software Serial Switch is set to __UART__ and not __DLINE__.
![UART/Software Serial Switch](https://raw.githubusercontent.com/jweakley/gobot/master/pictures/xbeeShieldSwitch.png)