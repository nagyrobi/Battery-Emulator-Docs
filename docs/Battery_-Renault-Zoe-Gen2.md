> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

## FAQ: Renault Zoe Battery Gen2
The Renault Zoe Gen2 has good support in the Battery-Emulator project. Note that confused packs need NVROL reset, more info on that further down in this Wiki

## Variants of the Zoe
There are 4x batteries available for the Zoe, This page focuses on the Gen2 50/52kWh battery
* [22kWh 2012-2019, Gen1](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Renault-Zoe-Gen1)
* [41kWh 2016-2019, Gen1](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Renault-Zoe-Gen1)
* 50kWh 2019-, Gen2
* 52kWh 2020-, Gen2

Stickers signaling that the battery is the Gen2 50/52kWh battery
![test](https://github.com/user-attachments/assets/f5eeef97-0cc7-42e1-8659-e2bbb07c94c7)

## Software configuration
For this battery type, use the option called "Renault Zoe Gen2 50kWh" under the "Battery Protocol" setting

<img width="593" height="73" alt="image" src="https://github.com/user-attachments/assets/986663b5-f444-4608-8f01-fe87f9bd5158" />

## Zoe Gen2 pictures and pinout
Credit goes to ljames28 for the excellent repo: https://github.com/ljames28/Renault-Zoe-PH2-ZE50-Canbus-LBC-Information

Beware that the plug for previous generations battery fits the Gen2 version but the pinout is different so it will not work.
Allso note plug pinout is seen from the rear of the plug where the wires come out. If you cannot source a plug and are comfortable opening the battery, theres a handy industry std connector just on the inside, but the colors of wires change. (white visible below)
![image](https://github.com/user-attachments/assets/4465c968-0313-43d3-891f-a9bffd249863)
![connections](https://github.com/user-attachments/assets/1d5aa171-3da0-4ca1-b13e-3038e2d1f7e9)
![image](https://github.com/user-attachments/assets/ad6e4e05-2c7d-46c1-ace9-fc0b728329da)



## Low voltage wiring diagram
Connect the pins from the battery to the Battery-Emulator, according to this diagram:

**Please note these circuit diagrams are currently work in progress, check validity yourself before powering on**

Example Wiring Diagram: LilyGo T-CAN485 + Zoe Gen2 + optional equipment stop button

![image](https://github.com/user-attachments/assets/17f5e4e6-7fc5-422d-af9a-fc431361af3b)

Example Wiring Diagram: LilyGo T-CAN485 + Zoe Gen2 + optional equipment stop button + CAN Filter (as required for some invertors)

![image](https://github.com/user-attachments/assets/d17cd1da-b9cd-4cea-9581-ad978865f9a5)

Example Wiring Diagram: LilyGo T-2CAN + Zoe Gen2 + optional equipment stop button

![image](https://github.com/user-attachments/assets/9808b678-1221-40a4-8ffb-9ea98a73be90)

Example Wiring Diagram: Stark CMR + Zoe Gen2

<img width="1064" height="764" alt="image" src="https://github.com/user-attachments/assets/d1f5507c-12c8-42d3-8015-d8de2acc90d9" />


> [!NOTE]
> This Zoe battery contains GND switched precharge relay and positive contactor. There is no negative contactor to control


> [!WARNING]
> It is very important to not mix up the wiring between precharge/positive-contactor. Running all the power thru the precharge will result in it blowing up 

<img alt="image" src="https://github.com/user-attachments/assets/5cf336f2-baeb-4acf-a227-158504104e61" />


## Part list
Incase your battery is missing parts, here is a list of the spare part numbers along with purchase links

## Part numbers for Renault Zoe Gen2 batteries

|  Product |  Purchase Link |
| :--------: | :---------: |
| Battery communication connector, Yazaki 7283-8854-30 (plastic shell, female) + contacts and seals acc. datasheet or complete set at Aliexpress. Battery has the male connector. |  [AliExpress](https://de.aliexpress.com/item/4000174903780.html)   |
| High voltage connector complete cable assembly 297A26138R (However these 'may' also work 297A6-5SH1A OR 297A22581R) or RCS800 connector shell (1x) 13974469 + contacts (2x) 13893887 + retainer (2x) 13893889 + seal (2x) 13893888|  [Ebay](https://www.ebay.com/p/17067914686)   |
| *Safety switch/fuse OEM Part no. 297C 126 45R | Renault UK quoted £43.89+VAT   |

*Please note not all safety switches are the same there is at least 2 versions, using the incorrect one could result in blown fuses or worse.
* Also part no. 993B1 5333R is for the sticker on top of the safety fuse not the safety fuse itself.

**The correct part number can be found by looking at the area as shown below.**
![image](https://github.com/user-attachments/assets/4103cdc2-95fb-45fe-b297-93526f919a5f)


My 52kwh battery came with following part:
![image](https://github.com/user-attachments/assets/9c11de1b-dcbd-4f60-93d7-3c9ff1a3eebe)

Renault Zoe Gen 2 safety switch has continuity between terminals 1-2 and 3-4, as shown above. (Renault Zoe Gen 1 has continuity between terminal 1-4 and 2-3, there is also a fuse inside which Gen2 switch does not have).

### High Voltage Interlock (HVIL)
The battery has two HVIL circuits that need to be shorted together to get the battery to think that connectors are seated.

- Service disconnect needs to be fitted (Note, get the correct one!)
- High voltage connector needs to be fitted

If one of these are missing, the Event HVIL Failure (EVENT_HVIL_FAILURE) will be triggered. To recover from this mode, power everything down, fit both connectors, and power back up. Note that there are multiple versions of the service disconnect switch for Renault batteries, so get the one specifically for the Zoe Gen2

## NVROL reset

### Why do we need to perform NVROL reset?
"If the pack is in a state where it is confused about the time, you may need to reset it's NVROL memory. However, if the power is later power cycled, it will revert back to his previous confused state. Therefore, after resetting the NVROL you must enable "temporisation before sleep", and then stop streaming CAN message 0x373. It will then save the data and go to sleep. When the pack is confused, the state of charge may reset back to incorrect value every time the power is reset which will make it hard to use the battery. Balancing of the battery will also be halted in this state"

### How do I perform this reset?
Under the Webserver for the Battery-Emulator, select the "More Battery Info" page, and then press the "Perform NVROL reset" button, and "OK"

![image](https://github.com/user-attachments/assets/e5b74fa7-24c7-476e-b6b4-0710748b6dfc)

![image](https://github.com/user-attachments/assets/a10a748d-3d4d-4988-a705-c5ec04269239)

![image](https://github.com/user-attachments/assets/d5833052-91cb-4b7a-96fc-91f28241642b)

### Example procedure off NVROL to balance battery :b: 
Zoe gen2 52kWh. First balancing started at last. Procedure that worked on my setup sequentially:
- Nvrol reset performed
- 30-ish seconds wait
- Verify temporization active from More Battery Info page
- Remove battery can plug from Stark CMR
- Bms power down (removed plug from stark)
- Stark CMR power down
- Connect all back together
- Stark CMR power up
- Balancing started immediately, visible from Cellmonitor page

> [!NOTE]
> The reset requires a 30 seconds sleep after completion. To sleep properly, all CAN communication is halted. If the Battery is connected to the same CAN channel as an inverter, this inverter will prevent the sleep cycle. So in order to properly do the NVROL-sleep, a dedicated CAN channel for the Zoe battery is required

## Example integration
Wallmounted Zoe 41kWh battery:

![image](https://github.com/user-attachments/assets/b0ae3208-20d0-49ce-bdd9-02c55e61906b)

![image](https://github.com/user-attachments/assets/e76f45c0-2298-4718-acf3-5234c18a0eb4)

## Troubleshooting
The Zoe2 pack has fuses that can be blown. Telltale sign of this being blown is that the voltage was dropping below what was read by BE, so the voltage was read via CAN as 358 but actual voltage measuring was 310-320v

<img alt="image" src="https://github.com/user-attachments/assets/a433d927-9877-4a1f-ba64-a7b1e05acfb6" />

