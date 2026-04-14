# Compatibility

This project should work on slot-loading iMac G3s. The earlier tray-loading models have a different video connector, and there is a different (simpler) process to use one of those as a monitor.

This guide is for installing 3 circuit boards, and using an Arduino (ATmega 328) so the iMac can be used as a monitor for any VGA output. If you want to install a raspberry pi in the iMac, it is not necessary to use the Arduino. There are other resources in this repository showing how to set that up. 

This guide does not go into detail about how to use the speakers in the iMac. 

# Ordering parts

## Printed Circuit Boards (PCBs)

To order PCBs, upload a zip file of the design (in Gerber format) to the PCB manufacturer of your choice. There are many options, such as JLC PCB, OSH Park, and PCBWay. These zip files are ready to upload:

[J20 board gerbers zip](https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/schematics_and_pcbs/imac_g3_333mhz_slot_loading_J20_adapter_board/gerbers/imac_g3_333mhz_slot_loading_J20_adapter_board.zip)

[J22 board gerbers zip](https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/schematics_and_pcbs/imac_g3_slot_loading_J22_adapter_board/gerber/imac_g3_slot_loading_J22_adapter_board.zip)

[Down converter board gerbers zip](https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/schematics_and_pcbs/imac_g3_down_converter_board_adapter/gerber/imac_g3_down_converter_board_adapter.zip)

Since the design uses large through-hole parts, paying to have the board manufactured would likely be more expensive than it's worth. Fortunately, for that same reason, it is a straightforward soldering project for any skill level.

Also note that the J20 board has undergone a major revision, so there are two versions of it in pictures. The newer version has only one ATmega chip and the VGA port is at an angle.

## Components

_Tip: Get at least two of everything! If something goes wrong, you will be glad to have extras._

There is a BOM provided for each of the three boards with DigiKey part numbers, ready to upload and order. A couple of parts may be obsolete or out of stock, in which case you'll need to find replacements. There also may be better prices from a different vendor.

This is a brief explanation of most of the components, to aid in making informed decisions about trade offs when choosing alternative parts.

### J20 board

[Bill of materials](https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/schematics_and_pcbs/imac_g3_333mhz_slot_loading_J20_adapter_board/bill_of_materials.csv)

[Interactive BOM for the J20 board](https://htmlpreview.github.io/?https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/schematics_and_pcbs/imac_g3_333mhz_slot_loading_J20_adapter_board/bom/ibom.html)

C1, C2, and the crystal are for the ATmega chip. The crystal needs to be 16MHz and [the datasheet says the capacitors can be 12-22pF](https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf).

The headers that plug into the Mac (2x10 pin) can be any old regular headers that fit in a 2.54mm pitch breadboard. If possible, get headers that are long on both sides so they stick out of the top of the board enough to attach a jumper wire. This will be useful later for debugging or testing. Also note that you can break apart a long row of pins, or combine smaller sets of pins to cobble together 2 rows of 10, so don't worry about ordering something precisely 2 rows of 10 pins.

The J3 and J5 (2x3 pin) headers are for debugging, programming, and interfacing with the ATmega chip. They can also be regular old 2.54mm headers. They don't touch the iMac, and only need to stick out on the top side of the board to accept your debugging and programming cables.

The terminal blocks listed in the BOM are actually detachable units of 2 sockets, so don't worry too much about getting exactly the right sizes of those. If there are the right total number, you can mix and match.

[This issue mentions why the relay was chosen](https://github.com/qbancoffee/imac_g3_ivad_board_init/issues/9).

Other components should probably be left as they are or replaced with an exact match.

### J22 board components

[Bill of materials](https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/schematics_and_pcbs/imac_g3_slot_loading_J22_adapter_board/bill_of_materials.csv)

[Interactive BOM for the J22 board](https://htmlpreview.github.io/?https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/schematics_and_pcbs/imac_g3_slot_loading_J22_adapter_board/bom/ibom.html)

The headers that plug into the mac on this board are also regular 2.54mm headers.

R3, C1 and the 3.5mm jack are only there to permit use of the microphone built into the iMac. If they are missing, the screen and speakers should still function.

R1 and R2 are for the indicator LEDs.

### Down converter board components

[Bill of materials](https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/schematics_and_pcbs/imac_g3_down_converter_board_adapter/bill_of_materials.csv)

[Interactive BOM for the Down Converter breakout board](https://htmlpreview.github.io/?https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/schematics_and_pcbs/imac_g3_down_converter_board_adapter/bom/ibom.html)

It may be worth it to shop around for compatible alternatives to the big Molex Micro-Fit connector, since the price and stock seem to fluctuate, and the difference can be $5-10. Amphenol's version is called MiniTek Pwr and it looks like it should be compatible with the connector in the Mac. It may be necessary to bend some structural supports to fit it on the PCB.

Some iMacs do not have a fan, so the fan header may not be necessary depending on your plans.

## Other stuff

### Arduino In-circuit Serial Programmer (ISP)

To program the ATmega, you can use an [arduino of some sort to use as a programmer/ISP](https://docs.arduino.cc/built-in-examples/arduino-isp/ArduinoISP/). You can also use an [ESP8266](https://github.com/vince-br-549/ESP8266-as-ISP), but if you don't already have one laying around, an Arduino UNO would likely be the simplest and best documented option.

Technically you can remove the chip, place it into an arduino UNO, and program it that way, but the ISP is vastly more convenient and there's less risk of damaging the pins on the ATmega chip.

### USB serial bridge

You will also need something that can be used as a serial to USB bridge. Pretty much any microcontroller dev board (arduino, raspberry pi pico, ESP32, etc.) or a dedicated TTL converter will work. In theory you could reuse the programmer arduino, but it will make life a _lot_ easier if this is a separate device so that you can switch back and forth easily.

### Speakers, amplifier, and audio equipment

[This list](https://github.com/qbancoffee/imac_g3_ivad_board_init/wiki/Convert-and-iMac-G3-slot-loader-into-a-standard-VGA-monitor#additional-items-you-might-need-in-order-to-add-an-sbc-andor-audio-amplifier) has some information about what to buy if you wish to use the speakers in the iMac.

This guide does not cover any details of adding an amplifier for the speakers. [This part of the assembly video shows the speaker/amplifier connection process](https://youtu.be/guVSI1aqcJg?t=2101).

# Disassembly

If your iMac has got a functioning logic board, this would be a good time to take a video of it powering on so you know what CRT sounds, LED colors, and other behavior to expect later. This will help in the case where you end up trying to diagnose a problem with the new boards.

Consult the [service manual](https://lastin.dti.supsi.ch/VET/sys/Apple/iMAC-G3/iMAC-Service_Guide-2.pdf) for disassembly instructions. It is extremely detailed and helpful for taking apart the computer without damaging it.

The screws are all different shapes and go in weird places. Take pictures and keep track of where they came from, or consult the exploded view in that manual.

In short:

- Remove the bottom cover, EMI shield, disk carrier, and logic board (attached to the down converter board).
- Separate the logic board and down converter board, and replace the down converter board in the computer.
- Keep the screws that were holding the logic board into the chassis; you will use them to attach the new PCBs.
- You do **not** need to remove the chassis or the top cover.

Stop taking things apart when it looks like this (plus or minus some speakers):

![imac with logic board removed](https://raw.githubusercontent.com/qbancoffee/imac_g3_ivad_board_init/refs/heads/master/images/chassis.jpg)

# Assembly

There is a [video showing the complete process of assembling the imac](https://github.com/qbancoffee/imac_g3_ivad_board_init/wiki/Convert-and-iMac-G3-slot-loader-into-a-standard-VGA-monitor).

## Boards

The screw holes that attach the boards to the metal chassis are the ground points, so make sure they are fully connected.

### J20 board

[Interactive BOM for the J20 board](https://htmlpreview.github.io/?https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/schematics_and_pcbs/imac_g3_333mhz_slot_loading_J20_adapter_board/bom/ibom.html)

[Assembly video](https://youtu.be/qILuajxscnE)

[Schematic](https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/pdfs/j20_schematic.pdf)

![J20 board 3D render](https://raw.githubusercontent.com/qbancoffee/imac_g3_ivad_board_init/refs/heads/master/images/J20_board_rev2.png)

If you want the monitor to send information about its supported resolutions, bridge JP1 and JP2 with some solder.

When it comes time to solder the large 20 pin headers, do so with the pins installed in the iMac to make sure the screw hole will line up properly. Refer to the video for details.

### J22 board

[Interactive BOM for the J22 board](https://htmlpreview.github.io/?https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/schematics_and_pcbs/imac_g3_slot_loading_J22_adapter_board/bom/ibom.html)

[Assembly video](https://youtu.be/JCPRYKbwKyk)

[Schematic](https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/pdfs/j22_schematic.pdf)

![J22 board 3D render](https://raw.githubusercontent.com/qbancoffee/imac_g3_ivad_board_init/refs/heads/master/images/J22_board.png)

When it comes time to solder the large 26 pin headers, do so with the pins installed in the iMac to make sure the screw hole will line up properly. Refer to the video for details.

### Down converter board

[Interactive BOM for the Down Converter breakout board](https://htmlpreview.github.io/?https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/schematics_and_pcbs/imac_g3_down_converter_board_adapter/bom/ibom.html)

[Schematic](https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/pdfs/down_converter_breakout_board_schematic.pdf)

![Down converter board 3d render](https://raw.githubusercontent.com/qbancoffee/imac_g3_ivad_board_init/refs/heads/master/images/down_converter_breakout_board.png)

Don't try to solder this one while it's installed in the Mac, but do double check which side of the PCB the large connector is on before soldering it.

If you have a fan, bridge the appropriate jumper for the voltage it uses.

## Wiring

After all three boards are installed in the iMac, connect the wires as shown in this diagram. Not every terminal/port will be connected.

If your iMac has a white LED instead of a green one, use the amber LED connector instead of the green LED one.

![Basic wiring diagram showing how all 3 boards are connected](https://raw.githubusercontent.com/qbancoffee/imac_g3_ivad_board_init/master/images/basic_wiring.png)

![Actual picture of all 3 boards installed](https://raw.githubusercontent.com/qbancoffee/imac_g3_ivad_board_init/refs/heads/master/images/all_boards_installed.jpg)

# Programming the arduino/ATmega 328

_Note: If you plan to put a Raspberry pi in the computer anyway, you can skip the Arduino and [just use the Raspberry pi to initialize the monitor](https://www.youtube.com/watch?v=isjo-4vQxwM). The releavnt python file is [init_ivad.py](https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/init_ivad.py)_

## IDE setup

Install and open the Arduino IDE [from the Arduino website](https://www.arduino.cc/en/software/).

Note: if you are using the Linux AppImage, you may have to run the AppImage file with the `--no-sandbox` argument.

## Installing libraries

Install [SoftwareWire](https://github.com/Testato/SoftwareWire) and [EEPROMWearLevel](https://github.com/PRosenb/EEPROMWearLevel) from the Arduino IDE's library manager.

## Installing the ATmega328 board package

Install MiniCore [using the instructions from the Minicore repository](https://github.com/mcudude/minicore?tab=readme-ov-file#how-to-install). This adds the option to select the ATmega 328 as a board in the Arduino IDE.

## Modifying the Wire library

Instructions from the sketch:

> In order for this program to work, the i2c transmit buffer length
> constants must be changed in two files. The Wire library has two
> buffers it uses for i2c transmissions
>
> `BUFFER_LENGTH` in
> `arduino_install_folder/hardware/arduino/avr/libraries/Wire/src/Wire.h`
>
> and
>
> `TWI_BUFFER_LENGTH` in
> `arduino_install_folder/hardware/arduino/avr/libraries/Wire/src/utility/twi.h`
>
> Both of these must be changed from 32 to 128 to be able to transmit
> the edid byte array in one shot.

On Ubuntu running the IDE as an AppImage, the path to the arduino install folder is `~/.arduino15/packages/arduino/`. It may be somewhere else on other systems.

## Programmer setup

If you are using an arduino as an ISP, follow [these instructions to get it ready.](https://docs.arduino.cc/built-in-examples/arduino-isp/ArduinoISP/)

In short:

- Connect the soon-to-be programmer to the computer with a USB cable
- Open the ArduinoISP sketch from the examples menu
- Select the type of board of the soon-to-be programmer
- Upload the ArduinoISP sketch

## Uploading the imacG3IvadInit sketch

Download or clone the github repo, and open the [`imacG3IvadInit` folder](https://github.com/qbancoffee/imac_g3_ivad_board_init/tree/master/imacG3IvadInit) in the Arduino IDE. 

Connect the MOSI, MISO, SCK, GROUND, and RESET pins on the programmer to the matching pins on the J20 board. You do not need to connect 5V if the board is installed in the iMac and the iMac is plugged into the wall (just plug it in, don't turn it on yet).

Select the ATmega 328 as the target board. In the Tools menu, set:

- `Programmer` to `Arduino as ISP` (or whatever the ISP is if it's not that)
- `Variant` to `328P/328PA`
- `Clock` to `External 16MHz`

From the Tools menu select `Burn Bootloader` (this only needs to be done once, before programming the ATmega chip for the very first time).

When that is done, from the Sketch menu, select `Upload Using Programmer`. For me, this took about 20 seconds and an LED on the programmer lights up while it is uploading.

If you get an error from the IDE at this stage, there may be a problem with the connection or configuration. Check your connections. This may take some googling to resolve.

There may be a warning `Warning: no eeprom data found in Intel Hex file`; this does not seem to hurt anything or indicate a real problem.

If you don't get any errors, then the board should be programmed and ready to go!

# Smoke test

The boards are installed, wires are connected, and the ATmega is programmed. If all has gone well, after pressing the power button, the monitor should power on and display an image.

### If this doesn't immediately work, don't panic. There are some settings that may still need to be adjusted.

More specifically:

1. Plug in the VGA cable to the computer with the image to display, and into the J20 board
1. Plug the imac into the wall
1. Press the power button
1. Power light should turn on
1. There should be some crackly static sounds from the CRT
1. There should be a "doink" sound from the CRT
1. The image from the computer should show up on the screen within 5-10 seconds

## Ways this could go wrong and how to fix them

### Absolutely nothing happens when you press the power button

- Make sure that the imac plugged into a power outlet that is definitely working
- Try flashing the ATMega chip again. Does the Arduino IDE report any errors?
- Check that all the boards are screwed down fully. It doesn't have to be that tight, but the metal around the screw hole must make contact with the chassis or there will not be a ground connection.

### The LED doesn't turn on but the CRT makes some noises

- If you have an iMac with a white LED, try connecting the Power LED cable to the slot labelled "amber LED" instead of the one labeled "green LED".

### The LED is on but the CRT doesn't make any sounds whatsoever

- If the LED is on, then the power has been switched on and the ATmega chip is doing something. Try checking the soldering and other connections.
- Try connecting to the ATmega via serial and seeing if it prints status information when you type "p".

### The CRT crackles, but it doesn't go "doink" and there's no image displayed

- This may be okay. Try adjusting the output settings on the computer and see if something shows up.
- This problem initially happened with a 600 MHz iMac DV. It eventually worked with the default initialization code after turning down the transmission speed. [The code for that is available here, along with some other changes](https://github.com/zacharesmer/imac_g3_ivad_board_init/blob/master/imacG3IvadInit/imacG3IvadInit.ino). The part that sets the speed slower is adding `softWire.setClock(50000);` somewhere before `softWire.begin();`. For that mac I also added some delays to more closely match a recording of the original logic board.
- The initialization sequence may be incompatible with your computer. There is a second one commented out in the Arduino sketch that you can try, or you can record your own logic board's init sequence with a logic analyzer. This is unlikely to be the case, but trying it may help reveal the actual problem. 

### The CRT sounds like it turns on for a moment, then turns back off

- Check that all the boards screwed down fully. It doesn't have to be that tight, but the metal around the screw hole must make contact with the chassis or there will not be a ground connection.
- Disconnect the reset line from the Arduino programmer and try it again

### The power LED is on and the CRT made all the noises, but the screen is still blank

- The display settings on the computer probably weren't automatically set, and you may need to manually adjust them. Read on...

# Adjusting Computer Output Settings


The display can accept video in these three resolutions/frame rates:

- 1024x768 @ 75 Hz
- 800x600 @ 95 Hz
- 640x480 @ 117 Hz

If it isn't receiving one of those, it stays blank. The monitor is supposed to send [EDID signals](https://en.wikipedia.org/wiki/Extended_Display_Identification_Data) to inform the computer of the acceptable resolutions/refresh rates, but sometimes the computer does not understand.

If you do not see any of those resolutions available in your display settings, you can override that and set values not advertised in the EDID signal.

## Manually specifying display settings on Linux (Xorg)

These steps use the modelines in [the modelines.txt file](https://github.com/qbancoffee/imac_g3_ivad_board_init/blob/master/imac_g3_modelines.txt).

Find the name of the VGA connection in the output of `xrandr`. Mine was `VGA-1`:

Then run these commands to add the modelines. This should make them available to select in the display settings.

```bash
xrandr --newmode "1024x768_75" 78.75 1024 1044 1140 1328 768 781 784 820 +hsync +vsync ratio=4:3
xrandr --newmode "800x600_95" 62.40 800 821 901 1040 600 609 612 644 +hsync +vsync ratio=4:3
xrandr --newmode "640x480_117" 49.90 640 657 721 832 480 481 484 514 +hsync +vsync ratio=4:3
xrandr --addmode VGA-1 "1024x768_75"
xrandr --addmode VGA-1 "800x600_95"
xrandr --addmode VGA-1 "640x480_117"
```

Select one of these new modes in your display settings, or run

```bash
xrandr --output VGA-1 --mode 1024x768_75
```

An image should appear on the screen. It may be in the wrong place or the wrong colors, but that will be fixed in the next step.

These commands will not persist after you log out. To do that, you will need to edit your xorg.conf or find some other way to load the modelines on startup.

## Windows

TODO, please share if you've done this!

# Adjusting the display's internal settings

These are the settings stored within the actual monitor (brightness, color balance, etc.). 

The J20 board listens for serial data on its Serial RX and Serial TX pins (115200 baud 8N1 UART). Connect a serial to USB adapter/bridge to those pins, and also to ground. TX on one side connects to RX on the other side, and vice versa. 

There is not a ground pin in the serial rx/tx header, but you can use the ground pin from the programming headers, alligator clip it anywhere on the metal chassis, or stick it anywhere else that connects to ground. If it is not connected to a common ground, though, it will not work.

It may be necessary to install a driver or configure the serial adapter so that it shows up on the computer. On Linux you may have to set up a udev rule.

There is [GUI app available](https://github.com/qbancoffee/imac_g3_ivad_board_init/tree/master/ImacG3DVDisplayProperties) to adjust the settings. [Video of it in use](https://www.youtube.com/watch?v=zrzcvThxDeY).

You can also send commands directly using a program like PuTTY or Minicom. To test your connection, typing the letter "p" should print out some information. Note that the device can't be in use at the same time by the GUI app and by PuTTY/minicom, so close one before trying to use the other. What each key does is in a comment in the Arduino code.

This is an [alternate version of the sketch](https://github.com/zacharesmer/imac_g3_ivad_board_init/blob/master/imacG3IvadInit/imacG3IvadInit.ino) that is more ergonomic for keyboard use, but it is not compatible with the GUI app. (Type ? in this version for help)

Note that anytime you re-upload the Arduino code, the stored settings will be erased. Otherwise, once saved to the EEPROM they should persist across power cycles.

# More information

There is a lot more information and detail in the rest of the [GitHub repository](https://github.com/qbancoffee/imac_g3_ivad_board_init). There is a [wiki page about more of the reverse engineering process](https://github.com/qbancoffee/imac_g3_ivad_board_init/wiki/Convert-and-iMac-G3-slot-loader-into-a-standard-VGA-monitor#additional-items-you-might-need-in-order-to-add-an-sbc-andor-audio-amplifier), more pictures, links to various videos and other related work.

There is also [the original MacRumors forum thread](https://forums.macrumors.com/threads/imac-g3-mod-video-connector.1712095/) that is a great timeline of the discoveries and the process.

Good luck!
