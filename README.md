# mcu-coin
info about coin design and firmware


## v1.0 known issues
- [X] SW2 boot select button/GPIO9 
    - **Expected:** when held during boot ESP enters flashing mode.
    - **Observed:** device does not enter boot mode when button is held during boot.
    - **Fix:** After reviewing the ESP docs and the schematic issue was found to be that the button was connected to 3.3v instead of ground. Fixed by cutting trace to +3.3 and bridging the pin to the ground plane.
![image of incorrect boot select layout](https://github.com/Niich/mcu-coin/blob/main/docs/img/v1.0-sch-boot-button-bad.png?raw=true)
![image of incorrect boot select layout](https://github.com/Niich/mcu-coin/blob/main/docs/img/v1.0-sch-boot-button-good.png?raw=true)
- [ ] SW1 on-off/GPIO6 
    - **Expected:** when device is off pressing this button should turn on the device. while device is active quick presses should be detected on GPIO6. Long pressing when "on" should turn off the device. 
    - **Observed:** Button does not turn the device on. but once on the press can be detected on GPIO6 and long press does turn the device off.
    - **Fix:** After reviewing the schematic issue was found to be that the button was connected to "MainPower" instead of +BATT. meaning that when the device is off there is no voltage on the switch so pressing it did nothing. Fix is connecting the button to +BATT and cutting trace to "MainPower"
- [ ] 