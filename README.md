# ender-3-mods
Modifications and notes on the Creality Ender 3, 3D Printer

Printed about 500g of PLA, and it works well so far.

To do:
- Flash a bootloader
- Upgrade firmware
- Add auto bed levelling

## Flashing a bootloader
Using an Arduino Uno as ISP
### Requirements
- Arduino Uno + USB connector
- Computer (Windows, Mac, or Linux is fine)
- 5x F-F jumper cables
- 1x F-M jumper cable

1. Install Arduino IDE (note: Ubuntu apt includes a very old version. Better to download and install the tar from [Arduino.cc](https://www.arduino.cc/en/Main/Software)
2. Take the top cover off the Ender 3 (three hex screws)
3. With the Arduino unplugged, connect the 
4. There is a bug on the Linux version of the Arduino IDE preventing communication between the Arduino IDE and Ardunio ISP([see here](https://github.com/arduino/Arduino/issues/5520)). To fix this, open `ardunio-X.X.X/hardware/arduino/avr/programmers.txt` and change `arduinoasisp.protocol=arduino` to `arduinoasisp.protocol=stk500v1`.
5. Open the Arduino IDE, and open the ArduinoISP sketch (File -> Examples -> 11.ArduinoISP -> ArduinoISP)
6. Add the Sanguino board
  - File -> Preferences -> Additional Boards Manager URLs = `https://raw.githubusercontent.com/Lauszus/Sanguino/master/package_lauszus_sanguino_index.json` then hit 'OK'
  - Tools -> Board -> Boards Manager
  - Search for 'Sanguino' and hit 'Install' then 'Close'
6. Connect the Arduino to the computer via USB
7. Check board, processor and port
  - Tools -> Board -> Ardunio/Genuino Uno
  - Tools -> Processor -> ATmega1284 or ATmega1284P (16 MHz)
  - Tools -> Port -> Choose the port your Arduino is connected to
7. Flash the sketch to the Arduino (Upload button, or Ctrl + U)
8. Change board to Sanguino and burn bootloader
  - Tools -> Board -> Sanguino
  - Tools -> Programmer -> Arduino as ISP
  - Tools -> Burn Bootloader
