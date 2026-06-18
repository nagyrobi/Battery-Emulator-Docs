
> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

# Compatible BYD batteries
The code supports a variety of BYD vehicle batteries. Check the product code sticker, and verify that the battery has already been tested with the Battery-Emulator, indicated by the ✅-mark that contactor closing works and the pack has been confirmed working

To get contactor closing to function, start BYD battery first, and Battery-Emulator afterwards. If you start Battery-Emulator before the battery, it wont close contactors before you restart the emulator. Also make sure no FAULT events are active when trying to start, this will open contactors.

|  Product Type |  Byd Model  | Energy | Capacity | Nominal voltage | Status |
| :-----------: | :---------: | :----: | :------: | :-------------: | :----: |
| PV2 | ? | 40 kWh | 150Ah | 300.8V | ✅ 
| PE4 | Seal | 61.66kWh | 150Ah | 409.6V | ✅
| ??? | Atto 3 | 50kWh | ? | ? | ✅ 
| P48 | Atto 3 | 60.48kWh | 150Ah | 403.2V | ✅ 
| VD6 | Seal U DM-i | 18,32kWh | 54Ah | 339.2V | -
| PA4 | Tang | 86.4kWh  | 135Ah | 640V | -
| PE5 | Seal   | 82.56kWh | 150Ah | 550.4V | -
| PE6 | Seal   | 82.56kWh | 150Ah | 550.4V | ✅ (To get contactor to close,power up the battery (12V), wait 5 seconds and reset BE)
| PK3 | Dolphin | 49.92kWh | 150Ah | 332.8V | -
| P07 | T3     | 50.37kWh | 115Ah | 438V | -
| P94 | Dolphin | 44.928kWh | 135Ah | 332.8V | ✅ 
| P99 | ? | 44.928kWh | 135Ah | 320.0V | -
| ? | Song Plus | 82.56kWh | 150Ah | 550.4V | ✅ 
| PM6B | Song Plus | x kWh | 170Ah | 512.0V | ✅ 
| PM7 | Seal U | 71.800 kWh | 170Ah | 422.4V | - 

Confirmed working BYD Seal 60kWh battery example sticker:

![image](https://github.com/user-attachments/assets/790449a8-5fcc-4685-a7ab-c549a4145c9c)

> [!NOTE]  
> If you intend to run two BYD batteries in [parallel](https://github.com/dalathegreat/Battery-Emulator/wiki/Double-Battery), make sure they are both the same model!

### Example battery install, Atto 3 P48 battery

The extended range nominal 60.48 kWh BYD Atto 3 blade battery pack is 1.2m wide by 2.1m long and 120mm high, weighing 402kg. 
The battery chemistry is LFP (LiFePO4). The battery configuration is 126s1p, estimated capacity of fully charged pack is 126 cells at nominal 3.2V x 150Ah connected in series = 60.48 kWh. Fully charged the pack produces total voltage of 441V max (3.5V/cell) and at 10% SOC, about 403V (3.2V/cell), as measured and monitored by OBD2 CarScanner during charging.

![](https://github.com/juancruz1953/Images/blob/main/Mounted%20bat.png)
 
Viewed from the front, left is the low voltage connector, central is two refrigerant lines and right is the HV connector, with +ve on RHS.
<img alt="image" src="https://github.com/user-attachments/assets/3ac1467f-e4b5-4241-8023-875c2ee35c6d" />

The front connectors end of the battery also includes the contactor block. There is a well-hidden 800V/350A fuse near the positive contactor(coil:12VDC/contactor:250A) on the RHS of the block, along with a mini pre-charge contactor (coil:12VDC/contactor:10A) and a pre-charge resistor, which is underneath the HV connector. There is no Tesla-like pyro fuse that blows when airbags are deployed; the system just opens the contactors.

## Software configuration
For this battery type, use the option called "BYD Atto 3/Seal/Dolphin" under the "Battery Protocol" setting

<img width="599" height="115" alt="image" src="https://github.com/user-attachments/assets/020f6bd9-7f4e-4d51-8b58-198bc2e4376c" />

## Video example
Here is a great video made by "Flying Tools" showcasing how to connect the BYD Atto 3 battery

https://www.youtube.com/watch?v=YBYWBapnnyM

## Battery specifications

## LV Connectors
The connection diagram is derived from reverse engineering the pins. Most of these designations require confirmation, although the CAN-H and CAN-L have been properly assigned and confirmed working for PE4, PE6 and P48 battery
 
<img width="489" height="205" alt="BYD_Atto_BK51_pinout" src="https://github.com/user-attachments/assets/47efe4ca-eff8-4fc8-8b03-2c1cb02ddfe6"/>
<img width="489" height="257" alt="BYD_Atto_BK51_wiring" src="https://github.com/user-attachments/assets/d7a6202b-e222-40bd-8775-baa04a1be094"/>

<img alt="image" src="https://github.com/user-attachments/assets/7ec4c2c3-e985-4ef6-9ad6-307e1721f642" />

## Contactor Block Modification
In the event of the battery being locked, the pre-charge and two contactors can be wired to manually switch on, or preferably to automatically activate via 3 SSRs with the GPIO pins on the Lilygo board (see https://github.com/dalathegreat/Battery-Emulator/wiki/Contactor-Control-via-GPIO-pins).
When accessing the internals of the battery, wear the appropriate safety gloves and follow safe procedures to avoid shorting across HV terminals. To access the contactor block, first remove the top cover, which fortunately is not sealed down; ~ 76 screws and 2 central top bolts require removal. In the pictorial description that follows, details of the full removal of the contactor block is shown, to identify the various parts. With connection points identified, it is now not necessary to remove the block as these 12V connection points are accessible from the top of the block. This current protocol involved connecting 3 circuits individually to the precharge and two contactor relays. This was achieved merely by wiring in extra lines on top of existing wiring connector points. With hindsight, a more effective alternative is included in the discussion below (Unlocking a crashed battery).
https://github.com/juancruz1953/Images/blob/main/Atto3ContactorBlockRewire.pdf

## Parts list
Here are some of the part numbers and purchase links, incase your battery came without them
|  Part |  Product Link | Notes |
| :--------: | :---------: | :---------: |
| LV connector |  [AliExpress](https://a.aliexpress.com/_EugRLIo)   | 19pin 1192800MB 1192800FB BYD
LV connecor Pre-wired  | https://a.aliexpress.com/_EHMKS3i
| HV cable | ---- | OEM numbers: 1364774600 & SC2EM215300A or SC2EM-2105300

Example, high voltage cable # 1364774600

![](https://github.com/juancruz1953/Images/blob/main/HVcableAtto3.jpg)

### Note on reusing HV cable :zap: 
If you are using the HV cable that came with the battery, and plan to cut off the ends to crimp on new terminals, be aware that the cables contain an outer shield layer. It is very important to properly insulate this, so you do not short high voltage to protective earth accidentally.

When preparing the cable, special attention must be taken the cable's shielding:

<img alt="image" src="https://github.com/user-attachments/assets/5f55011d-134c-48d1-844f-3eccf25d0584" />

First make sure to leave at least 8mm space:

<img alt="image" src="https://github.com/user-attachments/assets/3b3b730d-98ae-48ce-8fcc-9d3f7d275621" />

Then apply hot glue or other insulation adhesive:

<img alt="image" src="https://github.com/user-attachments/assets/ccdad47a-dd1b-49cb-9f07-ae8e924b1fde" />

Final insulation layer applied:

<img alt="image" src="https://github.com/user-attachments/assets/2685cb2f-e7bc-4750-a062-63f954c5b5f1" />

It is recommended to check your handiwork, by performing an insulation test on the cable after completing the work

<img alt="image" src="https://github.com/user-attachments/assets/57804548-9f0c-4d06-8801-f77ab69ccc09" />

## How do I know if I have a crashed&locked battery?
If the contactors do not engage when sending CAN towards the battery, the pack is most likely locked.

Another way to check is to inspect the "More battery info" on the webserver. This page contains the SOC% value sent by battery (SOC Highprec). If this value stays the same when charging, discharging, the battery is locked.

## SOC Drift overtime
Some users experience a phenomenon where the battery’s SOC appears to drift when using SOC measured by BMS, causing the charging process to stop before the SOC reaches 100%. This drift may gradually increase over time, reducing the available capacity of the battery. Typical value is 1-2% SOC underreported drift per day

> [!NOTE]
> There is no permanent fix yet, but the following workaround can help recover some capacity.
> Perform this procedure only when the drift noticeably affects available capacity.

- Go to the "Calibrate SOC" option. From version 10.3.0 this can be done from the "More Battery info" page.
- Observe what the current "Capacity current: xx AH" is
- Set the "Calibration target capacity:" to the same setting as the current capacity (We do not want to calibrate AH, so it is important to set these to the same)
- Set the "Calibration target SOC:" option to the desired SOC% you want.
- Finally, press the "Calibrate SOC" button

<img width="493" height="199" alt="image" src="https://github.com/user-attachments/assets/c37f7670-e43c-4f14-b4da-a1c7199b947f" />

Repeat this procedure when necessary.

## How do I unlock a crashed battery?
There are two methods to try and unlock the battery. The methods are via More Battery Info page (easy), and alternatively via CAN Replay (harder)

> [!IMPORTANT]
> To be able to unlock, you need separate control over B+ and IGN pin going towards battery (The two 12V pins on the battery). These need to be powered on/off in a specific sequence

- Pin 4 12v+ BMS
- Pin 5 12v+ ignition
- Also make sure 12V supply has atleast 12.8V and 2A available before starting the unlock procedure

### More Battery Info Unlock
There is a button in the "More Battery Info" page in the webserver, that when pressed will attempt to unlock the crashed battery.

![image](https://github.com/user-attachments/assets/61a5621a-5aa4-4a1c-88a3-92e9414e9226)

### CAN replay
One user reported success by manually sending the CAN log file while the battery 

[resetLockedBYD_v1.txt](https://github.com/user-attachments/files/20038227/resetLockedBYD_v1.txt)

User 1: About the power cycle and how I did it..
- Battery-Emulator compiled with only TEST_FAKE_BATTERY and no inverter selected
- Started with no power to either 12V constant or 12V ignition. I have separate switches for them though.
- So first Battery-Emulator hardware is powered on.
- Then 12V constant switch on, wait a few seconds and ignition on.
- At this point no communication is present on CAN, complete radio silence.
- Run unlock commands, upload resetLockedBYD_v1.txt in the CAN replay page in Battery-Emulator, and transmit it towards the battery
- Once again radio silence.
- Switch off 12V ignition.
- Wait a few seconds and then switch off 12V constant.

User 2 success story:
- I have B+ and the 2 ignition wires on seperate switches.
- I turn on B+ first then ignition.
- Software was setup for BYD ATTO 3, v8.13.0
- Then I ran unlock procedure via "More Battery Info" page
- After the unlock I turned off ignition then B+ and the Liligo together then reverse process turning back on.
- Contactors closed after.

