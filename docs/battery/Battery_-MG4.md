** UNDER CONSTRUCTION **

## Specifications

| Year |  Model | Battery capacity | Supported? | Rated Voltage | Voltage Range |
| :--------: | :---------: | :---------: | :----------: | :----------: |  :----------: |
| 2024- | MG4 EH32 | 49kWh LFP       |   ✅⚠️ Testing ongoing | 315V | 250-365V 
| 2022- | MG4 EH32 | 51kWh LFP       |   ✅⚠️ Testing ongoing | 327V | 260-379.6V   |
| 2022- | MG4 EH32 | 64kWh NMC       |   ✅⚠️ Testing ongoing| 380V | 291.2-452.4V |
| 2022- | MG4 EH32 | 77kWh NMC       |   ✅⚠️ Testing ongoing| 380V | 302.4-469.8V |
| 2026- | MG4 EH?? | 64kWh LFP       |    TBC                 |  ??? | ???          |
| 2026- | MG4 Urban| 43kWh ?         |    TBC                 |  ??? | ???          |
| 2026- | MG4 Urban| 54kWh SemiSolid |    TBC                 |  ??? | ???          |


## Current status

There are three current challenges:

- Most packs (~75% of ones tested) will close contactors when requested, and report the state-of-charge over the PT EXT CAN, but others do not. It is not clear why not, perhaps they are crash-locked and need resetting.

- It is necessary to use the PT EXT (non-FD) bus for closing the contactors, but the PT (FD) bus for reading extended information (including temperatures). The buses can be linked together and it does work, but getting the diagnostic information is slow as the interface is very overloaded.  The better option would be to have the PT EXT on the T2-CAN's 2515 interface with the PT bus connected to the 2518 add on board, but it is not clear how best to do this with BE, which normally only uses one bus per battery.

- It is not known whether the packs are balancing.

Both locked packs (using external contactor control and coulomb-counting) and non-locked packs (using BMS contactor control and SoC) are currently being tested.

## Software configuration
For this battery type, use the option called "MG4 battery" under the "Battery config" setting

![be](https://github.com/user-attachments/assets/8fb9bcd5-d6ef-4244-a7e1-85e20a467dc5)

## Connectors

The MG4 battery has an HV connector (Orange), and a 12 pin Low Voltage signal connector (Black/Red). There are also two coolant ports that can be used for thermal management, left is inlet, right is outlet (optional)

## Low voltage connector

![ESS_connector_pinout](https://github.com/user-attachments/assets/0795c45d-6387-440e-825d-8842876f9160)

## Low voltage socket

![39d3687d-7d61-4841-a597-aa59d4bf7a2a](https://github.com/user-attachments/assets/72f9c20d-9ba6-4e33-b265-d71948dd2923)

<img width="545" height="382" alt="mg4LV" src="https://github.com/user-attachments/assets/9e5c4138-e508-4b6b-939f-37065e6e7f13" />


This is the Low voltage connector plug: https://www.aliexpress.com/item/1005004677986133.html

The one you need is the female.

There's are non-wired versions avaialable too for doing your own crimping but this looks easier to implement.

<img width="344" height="348" alt="alilvplug" src="https://github.com/user-attachments/assets/6871d48c-dd5d-4b36-8415-9cb62751c30a" />


Lots of useful information here: [MG4 ESS SM.pdf](https://github.com/user-attachments/files/25114213/MG4.ESS.SM.pdf)

### Power connections

The battery needs a 12V-14V supply, and draws 600mA continuous with the contactors closed, and ~3A briefly when closing the contactors. 

To reduce potential issues with the isolation measurement, it is preferable to have a 12V supply that is isolated from the grid (eg, powered from a 2-pin double-insulated adapter).

### CAN connections

The battery has three CAN buses:

**CAN PT** is the powertrain bus, which connects the main powertrain components. The battery outputs its vital statistics (voltages, SoC and temperatures) on this bus. OBD requests (0x7e5 & 0x7DF) work over this bus. It is an FD interface which requires a 50000kbit CAN  2Mb CANFD connection.  

**CAN PT EXT** is the powertrain extension bus and it listens for the messages which control the contactors. It's a regular CAN interface at 50000kbit.

**CAN BMS** seems to be the raw data from the BMS, we're not currently sure what information it contains.  It's a regular CAN interface at 25000kbit.


The PT and PT EXT buses may be able to be bridged together (connecting the high/low pins in parallel) which provides the necessary information as well as allowing OBD/UDS communication (necessary to reset the battery if isolation failure detection occurs).

## High Voltage Interlock (HVIL)

The HVIL connections don't seem to be an issue.

## Physical Size

The 51kWh packs are 1880mm x 1440mm x 110mm and 400kg, the 64/77kWh are 1880mm x 1440mm x 125mm and 410/450kg (including the mounting rails).  At the top of the pack is an Energy Distribution Module (EDM) which mounts the contactors, High and Low Voltage connections and the Battery Management Unit (BMU). If getting from a wrecker/breaker try to get the Power Distribution Box (PDU) as it has a number of useful connectors that can be reused.

![image](https://github.com/user-attachments/assets/01da2eeb-488b-49a0-aeec-a0c0877a49bf)

![PXL_20251119_080901770 MP](https://github.com/user-attachments/assets/8e91c263-8cf2-43be-854f-78798528c3d2)

![565167202_24727149553573186_4603669578284620967_n](https://github.com/user-attachments/assets/30d5c69e-8828-436a-92e8-b6a98ac62792)

![goes-here](https://github.com/user-attachments/assets/aa4dad13-8870-4695-b182-521248e2ca03)



You will also need to use an isolated 12V power supply (a 2-pin power supply with no earth pin) to power the BMS as well as the Battery Emulator hardware (unless you are using isolated CAN), to ensure that there is no current path between the BMS case and the inverter's ground.
