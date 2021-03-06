# SMuFF-1.1

This is the latest version of the SMuFF firmware to be compiled using PlatformIO.

This is the software package for the Smart Multi Filament Feeder (SMuFF) project as published on [Thingiverse](https://www.thingiverse.com/thing:3431438).
![The SMuFF](https://github.com/technik-gegg/SMuFF-1.1/blob/master/images/SMuFF%20render-2.png)

You have to compile and install this firmware on the Wanhao i3 duplicator mini board (i.e. [AliExpress Wanhao i3-Mini](https://www.aliexpress.com/item/motherboard-i3mini-0ne-motherboard-New-2017-Wanhao-printer-i3-Mini/32849200836.html?spm=a2g0x.10010108.1000001.12.20c22a870NKth9&pvid=f20ef7d9-21cb-4600-b3eb-75382e0c6661&gps-id=pcDetailBottomMoreOtherSeller&scm=1007.13338.122670.0&scm-url=1007.13338.122670.0&scm_id=1007.13338.122670.0])) or on the SKR mini V1.1 board [AliExpress SKR mini](https://www.aliexpress.com/item/33030594091.html?spm=a2g0o.productlist.0.0.e3fe7d4de7t12F&algo_pvid=ffbbb716-871c-4ebd-95eb-b68c9e99cea3&algo_expid=ffbbb716-871c-4ebd-95eb-b68c9e99cea3-2&btsid=b2bcac4f-54c8-4542-9243-e4c24264a3cf&ws_ab_test=searchweb0_0,searchweb201602_,searchweb201603_53).

The Wanhao i3 mini is a very small but powerful controller, usually used to drive a 3D printer. It can handle up to 4 stepper motors, has it's own LC display and SD-Card / rotary encoder and runs on 12V as well as on 24V, it's the ideal tool for this project.  
The SKR mini v1.1 is also very small and yet more powerful because of the 32-Bit STM micro controller chip. On a downside, it comes with no display nor a rotary encoder but you can utilize any stepper drivers you like or have laying around.

This firmware can be adopted to run on other boards as well. Make sure your board of choice has all the components needed, then adopt the settings according to the hardware being used in your *platformio.ini* and *pins.h* files.

The basic configuration (SMuFF.cfg) must to be located on the SD-Card. Thus, changing parameters doesn't require recompiling the firmware. Just edit the configuration JSON file and reboot.
From version 1.6 on, the firmware has been enhanced in order to enable you making changes directly from the UI, which means: No more fiddling in the JSON file.
Also new in the 1.6 version is the option to run GCode scripts from the SD-Card for testing purposes. In the **test** folder you'll find some sample scripts. Copy those to your SD-Card and pick one from within the menu to start the test run. Once started, the test will run infinitelly and can be stopped by clicking the encoder button.

For further information head over to the [Wiki pages](https://github.com/technik-gegg/SMuFF-1.1/wiki).

## Recent changes

**1.67** - Bugfix for SKR in Duet3D mode

+ fixed sending endstop states to wrong serial port for Duet3D. Please notice: In Duet3D mode you **must use** the Serial 1 (the one labeled TFT on the board). Serial 3 will not receive the endstop states, which are needed to make the scripts on the Duet3D work correctly.

**1.66** - Buxfix for Wanhao i3 mini

+ fixed compile time error in ZServo for Wanhao i3
+ fixed compile time warnings for Wanhao i3

**1.65** - Minor changes - mainly for servo variant

+ changed the servo timing (duty cycle was twice as long as it's supposed to).
+ added live position change while configuring the servo positions in the menu (opened/closed).
+ added option to set up the servo pulse cycles (0 means: cycle forever, any value above 0: cycle only *n* times). *Set this value to about 20-30 if you experiencing jitter on the servo.*
+ added "**UseServo**", "**ServoOpened**" and "**ServoClosed**" parameters to M205 GCode command.

**1.64** - Minor changes

+ reworked the acceleration/deceleration algorithm for the stepper motors.
+ changed some default speed settings in the SMuFF.cfg file for SKR (due to the reworked acceleration/deceleration).
+ modified some of the test scripts for automated testing.
+ added M98 GCode to enable startig test runs from GCode (M98 P"filename" - omit the file extension ".gcode"). *Please notice: Since serial communication is disabled during test runs, you'll have to stop them by pressing the encoder button.*
+ reworked timer usage on STM32.
+ reworked servo library.

**1.63** - Minor changes

+ Wiggling the Revolver is now an option in the settings.
+ Added Feed error count to test run info display.
+ Added ok/missed feeds to test run. You'll need a 2nd endstop at the end of the bowden tube, which gets connected to the X+ endstop port on the SKR mini.
+ On feed errors, firmware will now retry 4 times before giving up. On second retry it'll home and reset the Selector. On third retry it'll go through all tools and retract the filament a bit (to prevent other tools/filaments blocking the Selector), then reposition the Selector as well.
+ Modified the servo module, so that it can handle more than one servo. Also, redefined the pins for the servo in the Pins.h. Servos are now driven by the endstops Y+ and Z+ ports.
+ Added experimental code to replace the Revolver stepper motor with an standard sized servo. This is still work in progress.

**1.62** - Not been published

**1.61** - Not been published

**1.60** - New enhancements

+ restructured the Main Menu.
+ added **Settings Menu** - *now, almost all parameters can be changed comfortably through the UI and saved to the SD-Card.*.
+ added long click to main screen for a quick access of the *Settings Menu*.
+ added long click in each setting dialog to close the dialog and discard the changes made.
+ added M500 (save to EEPROM) and M503 (read EEPROM) GCodes. Latter will give you a JSON format output, like the contents of the SMUFF.CFG file.
+ dropped the **Materials** section from within the configuration file due to memory limitations on the ATMega (make sure you remove it completely from your config file, otherwise your controller might hang on startup).
+ added **Reset Feeder Jam** to the *Main Menu*, dropped the double click action for this from last version.
+ did some code clean up, extracted menu- and dialog functions into separate files.
+ moved the ClickEncoder library source into the project since I needed some code enhancements (resetButton).
+ added **Testrun Menu** - an integrated test function (SKR only) - *place scripts with GCodes on the SD-Card which will get executed one by one in an continous loop*.
+ added some test scripts to the **test** folder - *copy those onto your SD-Card o  use them* .

**Please notice:**
This version will be the last one compatible with the **Wanhao i3 mini** (or 8-Bit in general)!
With the development going on, the memory limitations on 8-Bit devices are a real deal breaker.
For example: The only way to reach the *Settings Menu* screen is the long click on the main screen. The way it works on the SKR mini (through the *Main Menu*) crashes on the ATMega because of low working memory. For the same reason I've got to leave out the testrun option.

Hence, I've decided to focus on the 32-Bit devices in future versions.
If you've already assembled your SMuFF with the Wanhao i3 mini controller, it's fine. There's no need to switch over to the SKR mini unless you *really badly need* some of the enhancements and extensions that may come in the future.

**1.59** - Not been published

**1.58** - Not been published

**1.57** - Not been published

**1.56** - Optimization

+ added resetting the "Feder Jammed" state by double clicking the encoder button.
+ removed the ZPwm.cpp / ZPwm.h since it's not needed.

**1.55** - Optimization

+ reworked Revolver movement - much smoother now.
+ optimized stepper motor speeds for SKR in configuration file.
+ Tools swapping is now being stored in the EEPROM.DAT file so that swaps will survive a reset.
+ added Cancel / Retry option when loading fails.
+ sound (beeper) now works flawlessly on SKR (STM32).
+ SKR still has issues with the fan (it's either full speed or no speed).

**1.54** - Not been published

**1.53** - This version has got some major changes:

+ Full integration of the SKR mini V1.1 controller board (STM32) completed.
+ Prusa MMU2 Emulation mode improved even more.
+ Heavy refactoring to make the code better readable.
+ Optimized memory usage (all strings are now located in PROGMEM).
+ replaced the Rotary Encoder library.
+ Header files are now located in the include folder.
+ Pins header file separated into subfolders for different devices (uses the  -I compiler directive).
+ Added Configs folder containing different configuration samples for the modes/controllers.
+ Platformio.ini modified to allow different build environments.
+ Moved datastore from EEPROM to SD-Card (mainly because of the STM32).
+ Indexer for Materials in SMUFF.CFG renamed from Tool0..x to T0..x (because of memory issues).
+ Added *FeedChunks* and *EnableChunks* settings to SMUFF.CFG. Those are needed since the communcation on the 2nd serial port tends to hand in long operations (such as feeding the filament to nozzle) and Prusa won't be able to abort the feed.
+ Added *StepDelay* setting to SMUFF.CFG for the SKR Mini. This is needed because of the speed of an 32 bit board to keep the steppers from stalling.
+ Added schematics of the SKR Mini LCD board.

**1.52** - Not been published

**1.51** - Not been published

**1.50**  - Not been published

**1.4x** - Not been published

**1.4**  - Major changes to gain better compatibility for Prusa Emulation Mode and setup for a different platform (STM32). Latter is unfinished yet. 
The configuration file (SMUFF.CFG) has got a new setting which defines the distance from the filament guide to the Selector (see *SelectorDist* setting).

**1.3**  - Some modifications to gain Prusa MMU2 compatibility

**1.2**  - Initial published version
