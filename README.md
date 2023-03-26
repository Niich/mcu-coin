# mcu-coin
## Design Goals
- make a device that is similare in form factor to military chalange coins
- production cost needs to be in the range of traditional coins ($5-$15)

## V1.0 features
- based on ESP32C3
- 100mAh 302020 battery
- 7 addressable RGBs
- 2 GPIO buttons
- hardware power on/off and boot select **
    - to reduce power draw when off and to guard against bad firmare bricking the device since once it is assembled, battery soldered in and parts glued together, removing power to reset device would be very difficult.
- USB charging
- wireless charging ***
- WiFi ***
- bluetooth ***


** not working on original design but manual fix found

*** Currently not working


![pcb top view](https://github.com/Niich/mcu-coin/blob/main/docs/img/v1.0-pcb-top.jpg?raw=true)
![pcb bottom view](https://github.com/Niich/mcu-coin/blob/main/docs/img/v1.0-pcb-bottom.jpg?raw=true)
![schematic main](https://github.com/Niich/mcu-coin/blob/main/docs/img/main-sch.jpg?raw=true)
![schematic antenna](https://github.com/Niich/mcu-coin/blob/main/docs/img/top-antenna-sch.jpg?raw=true)

## v1.0 known issues
- [X] SW2 boot select button/GPIO9 
    - **Expected:** when held during boot ESP enters flashing mode.
    - **Observed:** device does not enter boot mode when button is held during boot.
    - **Fix:** After reviewing the ESP docs and the schematic issue was found to be that the button was connected to 3.3v instead of ground. Fixed by cutting trace to +3.3 and bridging the pin to the ground plane.
    ![image of incorrect boot select layout](https://github.com/Niich/mcu-coin/blob/main/docs/img/v1.0-sch-boot-button-bad.png?raw=true)
    ![image of incorrect boot select layout](https://github.com/Niich/mcu-coin/blob/main/docs/img/v1.0-sch-boot-button-good.png?raw=true)
- [X] SW1 on-off/GPIO6 
    - **Expected:** when device is off pressing this button should turn on the device. while device is active quick presses should be detected on GPIO6. Long pressing when "on" should turn off the device. 
    - **Observed:** Button does not turn the device on. but once on the press can be detected on GPIO6 and long press does turn the device off.
    - **Fix:** After reviewing the schematic issue was found to be that the button was connected to "MainPower" instead of +BATT. meaning that when the device is off there is no voltage on the switch so pressing it did nothing. Fix is connecting the button to +BATT and cutting trace to "MainPower"
    ![image of incorrect boot select layout](https://github.com/Niich/mcu-coin/blob/main/docs/img/v1.0-sch-power-bad.png?raw=true)![image of incorrect boot select layout](https://github.com/Niich/mcu-coin/blob/main/docs/img/v1.0-sch-power-good.png?raw=true)
- [ ] Wireless charging
    - **Expected:** when device placed on wireless charging pad it charges
    - **Observed:** Pad detects device but power delivery never stabalises it seems to be failing the negotiation. Issue has been observed on two differant wireless power transmitters
    - **Fix:** Unknown
- [ ] 2.4Ghz wireless (WifI/bluetooth)
    - **Expected:** device connects to WiFi.
    - **Observed:** When 2.4Ghz functions are enabled in firmware the device crashes. 
    - **Fix:** Unknown
- [ ] Acrylic spacer and diffuser
    - **Expected:** Top and bottom PCB are seperated and atteched to a 3mm acrylic spacer.
    - **Observed:** Although the 302020 battery is listed as 3mm thick it is actualy slightly larger so the acrylic part is not thick enought to connect both sides without bending and placing pressure on the LiPo battery
    - **Ideas:** Make a thicker spacer. 1/8" might be ehough since that is 3.17mm