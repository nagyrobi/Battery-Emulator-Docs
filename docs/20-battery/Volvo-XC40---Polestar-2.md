> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

# Volvo XC40, C40 / Polestar 2 (SPA platform) battery wiki

## Buying tips

Make sure the contactor assembly (Left picture, silver box) is included with the battery. Some places remove it and sell it separately. Without this you wont get contactors,current sensor, precharge, fuses etc., rendering the battery unusable.

![image](https://github.com/user-attachments/assets/a6b27aa5-f730-418a-8da5-4bb88d64fffe)

> [!NOTE]
> It is OK to buy a battery from an airbag crashed vehicle. It is possible to clear that fault code via the "More Battery Info" page

> [!CAUTION]
> If you want to use Battery-Emulator to read info, check cells, on a standalone battery before buying, comment out any sending of 0x140 messages in the VOLVO-SPA-BATTERY.cpp file. Otherwise it will try to close contactors, and without the DC/DC converter it will permanently lock the contactors as welded. See further down for more info on DC/DC requirement

### Measurements
148x176cm and about 500kg

## Battery specifications / Serial numbers
The following SPA platform batteries are supported, checkbox on those confirmed by users to work

* Volvo XC40 2021-present
   * 64 kWh (usable), 69 kWh (gross) ✅
   * 75 kWh (usable), 78 kWh (gross) ✅
   * 79 kWh (usable), 82 kWh (gross) Extended range ✅
* Polestar 2 2020-present
   * 64 kWh (usable), 69 kWh (gross) Standard range
   * 75 kWh (usable), 78 kWh (gross) Long range ✅
   * 79 kWh (usable), 82 kWh (gross) Long range ✅

![bild](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/4ff233b6-1e9a-4492-a166-45a6d78af3f1)

## Software configuration
For this battery type, use the option called "Volvo / Polestar 69/78kWh SPA battery" under the "Battery Protocol" setting

<img width="592" height="72" alt="image" src="https://github.com/user-attachments/assets/a806096a-cd6b-490c-90c2-4bef79b962b2" />


## Wiring diagram, low voltage
Connect HVIL2_EXT_IN and HVIL2_EXT_OUT together with a cable. (this will close the HVIL loop in BECM.)
Dont forget the driveline connector. When using the 4wd it's only for on the connector to the rear axle. If you have a different type it can be multiple connectors. For the wiring diagram see https://www.loopybunny.co.uk/schematics/MY24_Schematic.pdf

The BECM has no built in 120ohm resistor. (BECM = Battery Energy Control Module) 
Make sure the terminating resistors are correct. CAN networks should have two 120 Ohm resistors in each end of the network. With everything OFF, you can measure resistance between CAN-H and CAN-L. The result should be 60 Ohm.

Attached below are pictures of the BMS pinout. Connect the pins to the LilyGo and 12V supply like this:
* Pin 42 to LilyGo CAN - H
* Pin 43 to LilyGo CAN - L
* Pin 24 & 33 to +12V 
* Pin 47 or 48 This wire needs to be connected to the battery casing
* Pin 48 GND (47 and 48 are internally connected, so any of them is fine)

![bild](https://github.com/dalathegreat/Battery-Emulator/assets/117033960/e4ac7515-86fe-482c-b216-d205493f90d8)
![bild](https://github.com/dalathegreat/Battery-Emulator/assets/117033960/56981584-4efd-4f97-8180-f7b1a2977b7c)

To use the CAN contactors on Volvo XC40 and Polestar2, you need to connect the GND from the battery connector (PIN47) to the battery case. This GND is used for insulation monitoring. Without it, the battery will not be able to check the insulation integrity and will not engage contactors.

## Wiring diagram, High voltage :zap: 
Here are the ports that you can use on the battery to get voltage out:
![image](https://github.com/user-attachments/assets/00fa951a-da98-4421-853d-b276990d7e16)

Positive LH, Negative RH

![image](https://github.com/user-attachments/assets/e69bcd77-2f91-4517-a401-bdbaabb2ade2)

Test wires attached:

![image](https://github.com/user-attachments/assets/94943262-6ece-4a84-8934-391d483305fc)

> [!CAUTION]
> Battery can lock itself if started without DC-DC converter on HV side!

In order to start the battery you need to have capacitance and current draw ready on HV lines. This can be achieved by connecting a DC-DC converter (which you can purchase from the link below) to the high-voltage output from the front motor. If you skip this step and try to start the battery directly, an irreversible fault code will trigger (Contactor welded). After this fault code is set, you won't be able to engage the contactors via the CAN bus anymore.

Connect the DC/DC converter to the high voltage output lines shown above. Pay attention to the polarity to avoid damaging the DC/DC converter

The DC/DC converter can also be used to charge a 12V lead acid battery, or left totally unused. The purpose of it is to mainly just enable contactors safely without triggering any fault codes.

DC/DC connected: 

![image](https://github.com/user-attachments/assets/23dd7068-3e9d-4c9f-b137-176fc414b0a3)

## Part numbers
Incase your battery is missing some wires/disconnect switches, here are the OEM part numbers and purchase links. Do note that it might be cheaper to source from your local scrapyard!
|  Product |  Purchase Link |
| :--------: | :---------: |
| Service disconnect switch 32324494 |  [Volvoshop](https://www.volvopartswebstore.com/products/volvo/Drive-Motor-Battery-Pack-Disconnect-Switch/17175319/32324494.html?srsltid=AfmBOorBmq44EIa0XG8wFXfUVbIYV8hX9a3dqO7GA3DVw_9dIVlpyGXg)   |
| Low voltage 48pin connector |  [Aliexpress](https://a.aliexpress.com/_ExZIFZS)   |
| Low voltage 48pin connector with cable (take black one) |  [Aliexpress](https://www.aliexpress.com/item/1005004818455812.html?srcSns=sns_Copy&spreadType=socialShare&bizType=ProductDetail&social_params=61005201509&aff_fcid=852b913266dc4024815ddb438eb8b7d6-1740411633747-06052-_EzfBvMG&tt=MG&aff_fsk=_EzfBvMG&aff_platform=default&sk=_EzfBvMG&aff_trace_key=852b913266dc4024815ddb438eb8b7d6-1740411633747-06052-_EzfBvMG&shareId=61005201509&businessType=ProductDetail&platform=AE&terminal_id=84fbcc707632424bb0d05791812b9bdb&afSmartRedirect=y)   |
| DC-DC converter |  [Aliexpress](https://a.aliexpress.com/_EvAwmxw)   |

## Handy tips :bulb: 

### Contactor closing with Deye 30kW, 50kW ,Solis S6-series and Fronius Gen 24.
There is a known issue with Deye inverters, that when starting it up with a Volvo battery that requires DC/DC power present, there is a risk that the Deye will cause an isolation issue that prevents contactor closing.

To solve this, Power off inverter by switching off the grid and PV. After the inverter has shut down, you can power on the battery. After it powers on, and the invertor starts, than power one the grid and PV connection. So basically the battery needs to be the thing that starts the Deye, Solis and Fronius first.

You will also need a robust 12V power supply, as contactors (models from 2024 onward) draw approximately 30–40W.

### Opening the contactor assembly

Use a heat gun to warm up the glued seal between the cover and the bay. Use a lever to open the cover then.
 
![image](https://github.com/user-attachments/assets/2ed5f90c-d2ee-4281-a6fa-640a322971ed)

### No CAN comm issue
When 12V power is applied to the battery and the LilyGo board, there is occasionally an issue where the CAN bus and the transceiver in the BMS control unit do not "wake up," making communication impossible. So far, I’ve found only one solution to this problem — adding an external MCP2515 module and configuring it for communication with the BMS. In this setup, everything works smoothly and without any issues, ensuring reliable data transmission over the CAN bus.

Another thing to test if issue with comm is to leave pin 24 (+12V) disconnected during the first startup of BECM.

![image](https://github.com/user-attachments/assets/46eba26b-c432-41c0-af01-3185dfbec21d)

## Reading DTCs
1. To read the DTCs you have to open another browser tab with the CAN-logger view.
2. Enter filter 1588, press OK. Then press "Stop & Back to main page" (the filter setting will persist), reopen CAN logger and it should be cleared and the filter is active. 
3. Press "More battery info" in the other tab and press read DTC.
4. Refresh the CAN-log and you should have the readout there. (BE performs a bunch of diag requests once a minute that also will end up in the log with that filter, but that is normal)

DTCs are presented with 4 bytes, last byte of each code is status. (Permanent, Intermittent)

Codes might be divided between consecutive CAN frames, but just add them together like marked in the image below.

![DTCs_2](https://github.com/user-attachments/assets/de469558-d555-48d2-8180-07c45466aace)

## DTC explanation
- 058D00 - Battery Monitor Module Voltage Monitoring Performance
- 0AA668 - Battery Voltage System Isolation Fault
- 0AAB68 - Battery Voltage Isolation Sensor Circuit Intermittent
- 0AA600 - Battery Voltage System Isolation Fault
- 0B5900 - Actuator Supply Voltage A Circuit Low
- 0B5900 - EV Battery Voltage Sense "G" Circuit
- 0B5E00 - EV Battery Voltage Sense "H" Circuit
- 0B6300 - EV Battery Voltage Sense "I" Circuit
- 0D079A - Battery Charging System Positive Contactor Circuit
- 0E2F00 - High Voltage Fuse "B"
- 1A7216 - EV Battery Module 19 Temperature Sensor. General Electrical Failures. Circuit voltage below threshold
- C10000 - Lost Communication With Engine Control Module (ECM)
- C29A00 - Lost Communication with EV Battery Pack Sensor Module

## Unlocking a permanently locked BMS

If the BMS is permanently locked, reflashing the BMS is required. Some .hex files available on Discord

<img alt="image" src="https://github.com/user-attachments/assets/228bed76-124f-4197-bbad-d0182da2a343" />

Miniwiggler v3 dap attached to BMS board via PCI-e from old motherboard to avoid soldering

<img alt="image" src="https://github.com/user-attachments/assets/6033fb51-1c8c-4cbf-950f-7c780c35597c" />

<img width="900" height="485" alt="image" src="https://github.com/user-attachments/assets/7614b1ee-e647-4235-8a50-ee85e8a8bd67" />

<img width="621" height="480" alt="image" src="https://github.com/user-attachments/assets/eb9002b7-acbf-4b50-bcd5-f87c76f6d1ab" />

<img width="624" height="475" alt="image" src="https://github.com/user-attachments/assets/b5795b50-df63-4eb7-aaff-7768ab7ad183" />

<img width="624" height="475" alt="image" src="https://github.com/user-attachments/assets/12c7cf54-7f8a-4ab5-b2c4-22c914ca4163" />



