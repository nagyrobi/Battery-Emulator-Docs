> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

## Note on stationary storage :notebook: :zap: 
1. To use the eCMP battery in stationary storage, the BMS needs to be isolated to keep contactors engaged. This requires opening the battery, exposing yourself to 400V. Only proceed with this battery if you are OK with High Voltage work. For the full procedure, see [this section of the wiki](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Stellantis-eCMP-(Citroen,-DS,-Opel,-Peugeot)#disabling-isolation-monitoring-via-hw-modification)
2. Also note that CAN communication needs to be completely electrically isolated to keep contactors engaged. This can easiest be achieved by using a "LilyGo T-2CAN" board, or adding a separate CAN Bus isolator, links in the [Lightning strike wiki page](https://github.com/dalathegreat/Battery-Emulator/wiki/Lightning-strike)

Failure to fulfill the two requirements will lead to contactors opening after 60 seconds of use (2 minutes on some packs), due to Isolation DTC being set inside the battery.

### Supported Stellantis e-CMP batteries
The following eCMP ( Peugeot, Citroën, DS, Opel/Vauxhall ) batteries are currently supported

- Citroen ë-C4 (2020-) ✔️
- DS DS3 (2020-) ❓ 
- Opel/Vauxhall Corsa-e/Mokka-e (2019-) ✔️
- Peugeot e-208/e-2008(2020-) ✔️

### Supported 50kWh & 75kWh VAN? platform batteries
The same Stellantis eCMP platform integration can be used for **some** Toyota/Citroen/Fiat/Opel/Peugeot/Vauxhall van batteries. These batteries come in 50 and 75kWh sizes. It is still unclear what packs work, and which require more integration. It is also unclear what specific platform these batteries are.

#### V1 vs V2 VANs
Only V1 VAN packs work, V2 does not. You can spot the V1 by looking at the small HV connector which has 4 screws. On the V2, this connector is larger and has two screws.

<img alt="image" src="https://github.com/user-attachments/assets/eaf73a8d-995f-47da-8ccf-48e190b56171" />

- Toyota Proace / Proace Verso Electric ✔️
- Citroën e-Jumpy / e-SpaceTourer ✔️
- Fiat Scudo / Ulysse Electric ✔️
- Opel (Vauxhall) Zafira / Vivaro ✔️
- Peugeot Partner / Expert / Traveller ✔️
- Maybe more, feel free to add

### Supported 44kWh & 82kWh "STLA medium" platform batteries
Work in progress, values not valid yet
- Peugeot e-3008 III (e-P64, 2024–present)  ❓ 
- Peugeot e-5008 III (e-P67, 2024–present)  ❓ 
- Opel Grandland II (2024–present)  ❓ 

### Battery dimensions
The 50kWh car battery weighs approximately 350kg

The 50kWh VAN battery weighs approximately 382kg
The 75kWh VAN battery weighs approximately 534kg

The 50kWh battery (108 2x cells) has an operating voltage between 356 and 448 VDC (108 double cells x 3.3 - 4.15V)

The eCMP platform comes in three different physical sizes, A, B and C type:

| Designation |  Type | Models | Length (mm) | Width (mm) | Height (mm) |
| :--------: | :---------: | :---------: | :---------: | :---------: | :---------: |
| A | eCMP L1 | eP2JO (Corsa-e), eP21 (208), eD34 (DS3), eP2QO (Mokka) | 2090| 1280| 280
| B | eCMP L2 V1 | eP24 (2008) | 2145| 1280| 280
| C | eCMP L2 V2 | eC41 (C4) | 2145| 1280| 280

![image](https://github.com/user-attachments/assets/5ca3b9fd-e8e7-4845-a48e-15f114df6585)

## Software configuration
For this battery type, use the option called "Stellantis ECMP battery" under the "Battery Protocol" setting

![image](https://github.com/user-attachments/assets/3e1a7684-045e-40a9-aa94-60d13f858e0b)

### HV connection
There are High Voltage Interlock (HVIL) connections that need to be seated on the battery. Depending on which battery you get, there will be multiple pins to jumper.

Example of jumpered HVIL on unused HV connector
![image](https://github.com/user-attachments/assets/3664687f-5375-4a1d-a7e2-6d728fef3afa)

Polarity on cable side
<img alt="image" src="https://github.com/user-attachments/assets/9c578303-187b-44d3-b066-734042d8fe07" />


<a name="HVIL"></a> To disable completely the HVIL, just short the 2 wires from the BMS connector:

![image](https://github.com/user-attachments/assets/93296fba-66ec-4333-962a-19dfa3e37b24)


### Wiring pinout
The following pinout has been reverse engineered on an ë-C4

|  |  |  |  |
| --- | --- | --- | --- |
| 1 | green | CAN H | (Connect to Battery Emulator CAN H)
| 2 | white | CAN L | (Connect to Battery Emulator CAN L)
| 5 | yellow | 12V WAKE UP | (Connect to permanent 12V)
| 6 | green | Crash signal | (Connect to permanent 12V)
| 7 | red | 12V permanent | (Connect to permanent 12V)
| 10 | grey | 12V permanent | (Connect to permanent 12V)
| 11 | yellow | HVIL | (Connect to pin 12) [*](#HVIL)
| 12 | pink | HVIL | (Connect to pin 11) [*](#HVIL)
| 14 | light grey | GND | (Connect to GND for the 12V feed)

This platform shares its low voltage connector with the [Stellantis SMP platform](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Stellantis-SMP-platform)

### Part numbers and purchase links
Did your battery not come with all the required cables/plugs? No worries, here are the part numbers and purchase links!

#### High voltage connectors
- https://a.aliexpress.com/_EImj7ZG

- J9D3-14N236

![J9D3-14N236](https://github.com/user-attachments/assets/bdde386e-7bbd-4c8b-8b68-f66426d6cfae)

- J9D3-14N238

![J9D3-14N238](https://github.com/user-attachments/assets/39d38cb7-a4fa-4a9b-9d98-796912fd1b1a)

- 5QE971015 (~110cm long)

![5QE971015B](https://github.com/user-attachments/assets/4641b579-7144-454b-b589-e2e3e7a1b37b)

- 5Q0971015 (~310cm long)

![5Q0971015](https://github.com/user-attachments/assets/f51c237d-6776-431f-8e77-7a15cfb05e27)


#### Class-Y Capacitors 10NF 400V:
- Aliexpress: https://a.aliexpress.com/_EH7Rw0k

#### Low voltage connector

LFP low voltage connector in stock on Mouser incase you are not able to get the harness with connector when you buy the battery. The connector has the pin numbering stamped on it.

<img alt="image" src="https://github.com/user-attachments/assets/6158042c-ae88-4330-b61c-3fab20b6d1d7" />

- Connector: 27ZRO-B-1A
- Pins 0.3 to 0.5 mm$`^2`$: SZRO-A021T-M0.64 
- Pins 0.75 to 0.85 mm$`^2`$: SZRO-A031T-M0.64 
- Dummy Plug: WPHDP-H-1A-H
- Aliexpress: https://aliexpress.com/item/1005010560783903.html

### Disabling isolation monitoring via HW modification

> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

The eCMP BMS performs real time insulation monitoring. When installing the battery to a stationary storage system, the solar inverter will start to perform insulation monitoring. This means the vehicle monitoring is no longer required, and unfortunately on the eCMP this monitoring will incorrectly detect leakage and open contactors after 1 minute when in use.

#### Step 1, isolating BMS by floating it

To get around this issue, we need to disable the insulation monitoring on the BMS. The only known way at this stage is to open up the battery, and insulate the BMS grounding points. This effectively disables the isolation monitoring from interfering with the contactors

To perform this, open up the battery and locate the BMS. Isolate the part circled in red.

![image](https://github.com/user-attachments/assets/97ab3cae-6055-4da7-b40b-8c060a31a741)

#### Step 2, Install capacitors

On this PCB inside the battery, place 2x Y capacitors 10nF between:
- BAT+ and GND
- BAT- and GND

![image](https://github.com/user-attachments/assets/596d3673-cde4-4c02-8cb6-7788e396086d)

![image](https://github.com/user-attachments/assets/3b12dba5-67f9-41e1-bee6-6f6cf88ba79f)

In a (75kwh) Van pack it looks a bit different, there doesn't seem to be a BMS ground in the contactor enclosure. But! You can run a wire from the BMS ground through the pack (30cm distance) to meet up in the contactor box. The pcb in the contactor box also looks a bit different. The top yellow wire is B-, the bottom pink wire is B+:

<img alt="WhatsApp Image 2026-04-18 at 22 06 23 (3)" src="https://github.com/user-attachments/assets/21497144-1718-4406-899a-375725554e42" />
 
In the BMS enclosure, the 2 bottom blue wires of the top plug are BMS ground: 

<img alt="WhatsApp Image 2026-04-18 at 22 06 23 (2)" src="https://github.com/user-attachments/assets/faefbb0d-1d5a-4ff7-a1e3-60bf6394f2d4" />
 
And an overview of the components in a Van Pack: 

<img alt="WhatsApp Image 2026-04-18 at 22 06 23" src="https://github.com/user-attachments/assets/8abab5de-e0cc-4409-a704-762ec6b9e1ae" />
(open the image in a new tab to see the details)
<br><br>
End result: 

<img alt="WhatsApp Image 2026-04-18 at 22 06 23 (4)" src="https://github.com/user-attachments/assets/b2f1d4aa-1f5b-4724-9ddf-666187d9448d" />
<br><br>
Alternatively, the same capacitors can be installed on the OUTSIDE of the battery, for a much safer install, not requiring dismantling the contactor box

![capacitors](https://github.com/user-attachments/assets/3874996b-60b6-4db8-8d99-6aee2bdec403)


![image](https://github.com/user-attachments/assets/88fd79af-64b6-4e45-8568-2138d44d6520)




#### Step 3, Insulate CAN bus from inverter
CAN needs to be on separate GND plane compared to inverter. Use a CAN opto isolator, or a board that has isolation between CAN chips like for instance the LilyGo T-2CAN. You can also add a CAN Bus isolator, links in the [Lightning strike wiki page](https://github.com/dalathegreat/Battery-Emulator/wiki/Lightning-strike)

### Unlocking the battery
Under the "More Battery Info" page you can run collision unlock, contactor stuck unlock, and isolation error unlocking procedures. Remember to press the "Open contactors" button via the main page before running these requests, otherwise they wont work.

![image](https://github.com/user-attachments/assets/bdf607c4-fab9-434d-9556-0f4473c7c886)

How to deal with red screens:

![image](https://github.com/user-attachments/assets/40cb6998-f3c7-47d0-ad78-49377551bf76)

If you have this red screen, check if you did connect the main contactors box (the second one is not needed, it's only used for DC charging)
Also you might have stored errors in BMS. When trying to clear the codes you MUST press multiple times the buttons (the commands are not always executed successfully so you need to press them more than once). Then reboot BE and do a hard reset (cut 12V then turn on again)

### Reverse engineering info
Can Logs can be found here: https://drive.google.com/drive/folders/1S-Nf0dN5nZi71HhXIoM3GTydEk_VHHuM?usp=drive_link

### Troubleshooting tips
- Try feeding the battery from separate 12V supply than your BE device. A 12V lead-acid battery is fine for troubleshooting.

### Double battery operation :battery: :battery: 
ECMP integration supports running two packs in parallel
For double ECMP batteries you need:
- 2 separate AC/DC PSU or another way to isolate the GND from the 2 BMS
- 2 HV DC contactors for the 2nd battery to join the 1st one (as long as the DC voltage difference is 2V or lower)
- an additional CAN communication for BE in order to talk to the 2nd battery
- an additional fuse for the 2nd battery
For T2CAN you can get a MCP2518FD and set it up as classic CAN. The 2 onboard CAN connectors will be used for the 2 battery pacs:  native (CANB) for 1st battery, CANA for 2nd battery and the additional MCP2518FD for the inverter
Currently you cant clear the 2nd battery error codes from BE, only the 1st battery.

Schematics below. As always - take extra care when working with HV batteries
<img alt="double-ecmp-battery-emulator" src="https://github.com/user-attachments/assets/51adb5ee-c9ee-4fbc-a108-1517359cc35e" />


NOTE: When  