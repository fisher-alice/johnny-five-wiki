## AKA "Johnny-Five's Most Wanted"

Johnny-Five supports a wide variety of components, boards, and protocols but the world of hardware is huge. There are occasionally requests for new devices. We track those requests below.

We hope this will be a place where new and existing contributors will come to see how they can help this amazing community. All of these issues and pull-requests are closed but only temporarily. All they need is a motivated contributor.

------------------
### New Boards/IO Plug-Ins
**Asus Tinker Board** [#1421](/rwaldron/johnny-five/issues/1421)

**NanoPC T-4** [#1502](/rwaldron/johnny-five/issues/1502)

**Omega2+** [#1334](/rwaldron/johnny-five/issues/1334)

**Orange Pi** [#1508](/rwaldron/johnny-five/issues/1508)

**Rainbowduino** Discontinued? [#574](/rwaldron/johnny-five/issues/574)

------------------
### New Classes
**Sound Generator/Synthesizer Class** [#1410](/rwaldron/johnny-five/issues/1410)
 - SIDuinoi2c [#1410](/rwaldron/johnny-five/issues/1410)
 - sid-arduino-lib backpack (Doesn't exist yet) [#1410](/rwaldron/johnny-five/issues/1410)
 - YM3812 OPL2 (requires SPI) [#1410](/rwaldron/johnny-five/issues/1410)
 - Adafruit Audio FX Sound Board [#936](/rwaldron/johnny-five/issues/936)

**GSM/Cellular**
 - SIM800l [#1136](/rwaldron/johnny-five/issues/1136)

**Receiver**
 - IR Receiver Module [#434](/rwaldron/johnny-five/issues/434)
 - RC Receiver [#1071](/rwaldron/johnny-five/issues/1071)
 - RFID Receiver [#804](/rwaldron/johnny-five/issues/804)

------------------
### New Devices

**Compass**
 - QMC5883 [#1486](/rwaldron/johnny-five/issues/1486)

**Expander**
 - Propeller Hat [#1139](/rwaldron/johnny-five/issues/1139)
 - 74HC4067 [#939](/rwaldron/johnny-five/issues/939)
 - 74HC4051 [#939](/rwaldron/johnny-five/issues/939)
 - 74HC165 [#456](/rwaldron/johnny-five/issues/456)
 - CD4021B [#456](/rwaldron/johnny-five/issues/456), [#1553](/rwaldron/johnny-five/issues/1553)

**GPS**
 - SIM808 [#1392](/rwaldron/johnny-five/issues/1392)
 - Sparkfun Venus [#1074](/rwaldron/johnny-five/issues/1074)

**IMU**
 - LSM9DSO [#1072](/rwaldron/johnny-five/issues/1072)
 - MMA7455 [#1535](/rwaldron/johnny-five/issues/1535)
 - MPU-9250 [#1446](/rwaldron/johnny-five/issues/1446)

**LCD**
 - QC1602A v2.0 [#658](/rwaldron/johnny-five/issues/658)

**LED.Digits**
 - HT16K33 Backpack [#1022](/rwaldron/johnny-five/issues/1022)
 - 8-bit 595 driver [#969](/rwaldron/johnny-five/issues/969)

**LED.Matrix**
 - HT1632C [#1000](/rwaldron/johnny-five/issues/1000), [#1006](/rwaldron/johnny-five/issues/1006)
 - MY9221 [#574](/rwaldron/johnny-five/issues/574)

**Light**
  - TSL2591 [#1105](/rwaldron/johnny-five/issues/1105)

**Sensor**
 - HX711 Load Cell Amplifier (Requires SPI)[#884](/rwaldron/johnny-five/issues/884)
 - INA219 High Side DC Current Sensor Breakout [#1500](/rwaldron/johnny-five/issues/1500)

**Servo**
 - Five wire servos [#1019](/rwaldron/johnny-five/issues/1019)
 - DMM Servos [#1559](/rwaldron/johnny-five/issues/1559)

**Thermometer**
 - MAX31855K (Requires SPI) [#1408](/rwaldron/johnny-five/issues/1408), [#738](/rwaldron/johnny-five/issues/738)
 - SHT11 [#1233](/rwaldron/johnny-five/issues/1233)
 - MAX6675 (Requires SPI) [#738](/rwaldron/johnny-five/issues/738)

------------------
### New Class Features

**Expander**
 - Add the ability to specify PWM range per pin instead of per expander [#1585](/rwaldron/johnny-five/issues/1585)

**IMU**
 - Access to DMP processed values [#1126](/rwaldron/johnny-five/issues/1126)

**Piezo**
 - Support RTTL, RTTL+ [#639](/rwaldron/johnny-five/issues/639)

**LED**
 - **Syncdrive/inverse/isAnode for non-PWM LEDs** [#1398](https://github.com/rwaldron/johnny-five/pull/1398)

**LED.RGB**
 - **Fade to color method** [#1309](/rwaldron/johnny-five/issues/1309)
 - **Color Correction** [#811](/rwaldron/johnny-five/issues/811)

**Motor**
 - **Start Assist** [#995](https://github.com/rwaldron/johnny-five/issues/995)
 - **Encoder** [#995](https://github.com/rwaldron/johnny-five/issues/995), [#1481](https://github.com/rwaldron/johnny-five/issues/1481)

**Servos**
 - **Bulk Servo Writes** - Add ability to accumulate multiple servo positions and send write command(s) on next tick. Ideal for PCA9685 that can receive multiple values in a single i2c message (and maybe one day bulk analog writes in firmata). [#1340](/rwaldron/johnny-five/issues/1340)

**Thermometer**
 - Add tolerance for temperature changes [#1137](/rwaldron/johnny-five/issues/1137)

------------------
### New Modules
**Fingerprint scanner module** - [#845](/rwaldron/johnny-five/issues/845), [#1353](/rwaldron/johnny-five/issues/1353)

**Gripper module** - [#698](/rwaldron/johnny-five/issues/698)

**Camera** - [#420](/rwaldron/johnny-five/issues/420)

**Apex 5400 Bill Acceptor** - [#1317](/rwaldron/johnny-five/issues/1317)

**RFID/NFC** - [#1108](/rwaldron/johnny-five/issues/1108), [#804](/rwaldron/johnny-five/issues/804)
 - Adafruit PN532 RFID/NFC Breakout and Shield

------------------
### Other Enhancements
**DMX512 Protocol** - This would have to be added to firmata first [#1326](/rwaldron/johnny-five/issues/1326)

**DAC Support## - This will have to be added to firmata first [#1285](https://github.com/rwaldron/johnny-five/pull/1285)
