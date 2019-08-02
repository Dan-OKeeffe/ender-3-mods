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

## Flashing firmware
### Getting required files/setting up the IDE
- Download the latest version of Marlin firmware [here](http://marlinfw.org/meta/download/)
- Extract it somewhere, and go to `Marlin-X.X.X/Marlin/example_configurations/Creality/Ender-3`
- Read `README.md` and note the latest supported version of U8Glib
- Copy all files from this directory to `Marlin-X.X.X/Marlin`. Overwrite files.
- Download the latest supported version of U8glib
  - In this case it was 1.17 which can be found [here](https://bintray.com/olikraus/u8glib/Arduino/1.17)
- Open the Arduino IDE, and open `Marlin-X.X.X/Marlin/Marlin.ino`
- Go to Sketch -> Include Library -> Add .ZIP Library
- Browse and select the latest supported version of U8glib you downloaded, and hit 'OK'
- Edit the Marlin configuration to enable/disable any features you'd like
  - Hit `Ctrl + F` -> 'Search all sketch tabs' to find these
  - Uncomment `#define arc_support`
  - Uncomment `#define MESH_BED_LEVELING`
  - Uncomment `#define LCD_BED_LEVELING`
  - Uncomment `#define SLIM_LCD_MENUS`
  - To do: provide diff of changes, with explanation
  
### Flash the firmware
- Set the board, processor and port
  - Tools -> Board -> Sanguino
  - Tools -> Processor -> ATmega1284 or ATmega1284P (16 MHz)
  - Tools -> Port -> Choose the port your printer is connected to
- Flash the sketch to the printer (upload button, or Ctrl + U)

## Adding auto bed levelling
- Purchased a '3D Touch' from [here](https://www.aliexpress.com/item/32913903746.html)
- Print the mount [here](https://www.thingiverse.com/thing:2990086)
- Turn off power, unplug, remove 3 hex screws from control box + remove lid. Detach fan cable to remove lid completely
- Clear route from control box to hot end. Cut zip ties, cut tape around control box, take back cable management sheath
- Remove two hex screws on hot end case
- Place 3D Touch and put hex screws back in
- Route wires from 3D touch through sleeve (including extension). Approx 90cm extension required, so extend if necessary
- Connect 5 wires from 3D Touch
  - Remove Z stop & replace with Black + White wire from 3D Touch
  - Connect power + ground via pins on SPI bus
  - Connect brown wire to pin 27 (buzzer pin on display)
- Flash new firmware (to do: add setup)
- Check that 3D touch is responding
  - It should extend/retract twice when turning on for the first time
  - In the menus, test extend and retract
  - Move up from bed (eg. 200mm). Choose to 'Auto home', and tap the 3D touch with your finger. If it doesn't stop, then turn off machine from power (to avoid damaging print head on impact with bed)
  - Auto home. It should move to the center of the bed. If not, check 'Z-switch removed' section above.
  
## Checking repeatability of sensor
