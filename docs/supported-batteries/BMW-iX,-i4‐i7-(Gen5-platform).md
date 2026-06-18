> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

> [!WARNING]  
> CAN bus contactor control is in beta only, and has been known to permanently lock out the battery - therefore non developers should use the GPIO contactor control ONLY (Tick "Contactor control via GPIO" in settings). Re-using the iX batteries require opening the battery to change internal wiring for contactors. To do this safely, the correct personal protective equipment (PPE) is required. When handling the inside of a BMW iX battery, please make sure you check your local electrical safety legislation requirements.

# BMW Gen5 BEV Platform - (iX, i4, i5, i7)

BMW uses a shared modular platform across various vehicles with a common BMS (SME). BMW i4 for example has the SE26 or SE27 configuration.

Unlike i3, Gen5 now uses CAN-FD on the external side, and ISO-SPI between SME > Cell modules.

Here is a list of all different BMW iX batteries, and their specifications / voltage ranges

| Technical data | SE10 | SE11 | SE12 | SE13 | SE16 | SE26 | SE27 | SE30 | SE50 |
|---|---|---|---|---|---|---|---|---|---|
| Vehicles | iX | iX | iX1 | iX1, iX2 | iX3 | i4 | i4, i5 | i7 | iX |
| Number of battery cells (lithium-ion battery)       | 500 | 180 | 156 | | 188 | 288 | 324 | 408 | 450 |
| Chemistry                                           | NCA | | NMC | | NMC | NMC | NCA | NCA | NCA |
| Configuration                                       | 100s5p | 90s2p | 78s2p | | 94s2p | 96s3p | 108s3p | 102s4p | 90s5p | 
| Number of cell modules                              | 5 cell modules (8s5p) <br> 6 cell modules (10s5p) | 10 cell modules (9s2p) | | | 8 cell modules with 18 battery cells <br> 2 cell modules with 22 battery cells | 4 dual-cell modules (two 12s3p) | 3 cell modules (4s3p) <br> 4 dual-cell modules (two 12s3p) | | |
| Nominal Voltage                                     | 368 V | 330.3 V | 286.3 V | | 345 V | 354 V | 398.5 V | | |
| Voltage range                                       | Min. 280 V - max. 430 V | Min. 252 V - max. 378 V | Min. 218.4 V - max 327.6 V | | Min. 263.8 V - max. 394.8 V | Min. 268.8 V - max. 408 V | Min. 302 V - max. 464 V | Min. 285.6 V | Min. 252.0 V |
| Battery capacity                                    | 303.0 Ah | 232.0 Ah | 232.0 Ah | | 232.0 Ah | 198.6 Ah | 210.6 Ah | 280.8 Ah | 303.0 Ah |
| Capacity per cell                                   | 60.6 Ah | 116.0 Ah | 116.0 Ah | | 116.0 Ah | 66.2 Ah | 70.2 Ah | 70.2 Ah | 60.6 Ah |
| Max. storable energy quantity                       | 111.5 kWh | 76.6 kWh | 66.45 kWh | | 80 kWh | 70.27 kWh | 83.9 kWh | 105.7 kWh | 100.35 kWh |
| Max. useful energy quantity                         | 106.3 kWh | 70.6 kWh | | | 73.8 kWh | 68 kWh | 80.7 kWh | 101.7 kWh | |
| Dimensions of the housing (length x width x height) | 2410 mm x 1742 mm x 141 (303) mm | 2410 mm x 1742 mm x 141 (303) mm | | | 2228 mm x 1586 mm x 311 mm | 2261 mm x 1708 mm x 285 mm | 2261 mm x 1708 mm x 285 mm | | |
| Total weight                                        | 649 kg | 521 kg | 436 kg | | 518 kg | 500.9 kg | 564.5 kg | | |
| Cooling system                                      | Coolant | Coolant | Coolant | Coolant | Coolant | Coolant | Coolant | Coolant | Coolant |

SE11 battery being transported on a trailer

<img alt="image" src="https://github.com/user-attachments/assets/a5e87534-044c-489e-a761-210db6ac2065" />


## Software configuration
For this battery type, use the option called "BMW iX and i4-7 platform" under the "Battery Protocol" setting

<img width="654" height="152" alt="image" src="https://github.com/user-attachments/assets/63b01717-62f4-4cc1-b03d-dd45246823b4" />

Also remember to configure the allowed charging power, since we do not read this value via CAN.

## Note on CAN-FD
The Gen5 BMW battery architecture uses CAN-FD, so if you plan on integrating this battery, you will need to get the LilyGo T-2CAN, plus a [CAN-FD chip add-on](https://github.com/dalathegreat/Battery-Emulator/wiki/CAN%E2%80%90FD-add%E2%80%90on-(MCP2518FD)).

Alternatively, if you want to make it even easier, get the [Stark CMR](https://github.com/dalathegreat/Battery-Emulator/wiki/Hardware:-Stark-CMR) hardware which has built in support for CAN-FD. This is the recommended path!

# Pin Assignments at Plug Connector A332*1B

A332*1B is the external low voltage connection. Connector is a Hirschmann 805-587-545 16way 1.2 SealStar FA Connector. Some packs have the LV connection at the front of the pack and appear to use a back to back connector instead.

If you are having trouble sourcing the Hirschmann connector, a cheap alternative can be found on Aliexpress. [Purchase Link](https://nl.aliexpress.com/item/1005005722083920.html) . IMPORTANT SIDE NOTE: its available in 2 different types. The difference is the locating pin. Be sure to order the one with the locating pin on the 8-16 side, and not on the 1-9 side.

<img width="588" height="395" alt="image" src="https://github.com/user-attachments/assets/86b6118d-5d22-4e34-bec4-0d5ff918a76f" />



| Pin | Type | Description / Signal Type               | Connection / Measuring Information               |
|-----|------|----------------------------------------|-------------------------------------------------|
| 1   | E    | Supply, terminal 30                    | Fuse F242 Power distribution box, rear           |
| 2   | M    | Ground                                 | Ground point                                    |
| 3   | E    | Terminal 30c signal                    | High-voltage safety connector                    |
| 4   | E/A  | Wake-up signal                         | Body Domain Controller                           |
| 5   | --   | not used                               |                                                 |
| 6   | --   | not used                               |                                                 |
| 7   | E/A  | High-voltage interlock loop signal (loop these two via 33ohm resistor)     | High-voltage safety connector                    |
| 8   | E/A  | High-voltage interlock loop signal (loop these two via 33ohm resistor)    | High-voltage safety connector                    |
| 9   | E    | Crash signal                           | Crash safety module         |
| 10  | --   | not used                               |                                                 |
| 11  | E/A  | CAN-FD Low  |                            |
| 12  | E/A  | CAN-FD High                 |                |
| 13  | E/A  | not used   |(possibly alternate pins for CAN-FD on some variants)                            |
| 14  | E/A  | not used                  |(possibly alternate pins for CAN-FD on some variants)               |
| 15  | A    | Activation                             | Coolant shutoff valve 2                          |
| 16  | M    | Ground                                 | Coolant shutoff valve 2                          |




# HV Connectors

| Count                                      | Connector                                         | Cable/Cap                                         |
|-----------------------------------------------------|----------------------------------------------|----------------------------------------------|
|1 |DC charge connector |Protective Cap Hv Battery 889520 - BMW (12-90-9-796-829)
|1 or 2 |main connectors | Rosenberger HVS420 - Protective Cap for HV Battery 889520 - BMW (12-90-9-796-829)
|1 |CCU/AC Connector (100A fused)|  Hirschman HPS40-2 - Suitable cable is 5A2DB59-03

# HV Connector Blank - 3D Printed

Here are some 3d printable covers for the large rear connector, smaller front connector and internal blanking covers for BMU (If you disconnect the additional HV outputs internally)
https://www.thingiverse.com/thing:6845382/files

# Pin Assignments on BMS Internal Connector

![image](https://github.com/user-attachments/assets/c0a52557-87e9-4dda-a5e3-21fc9e01ca04)

# Pin Assignments on contactor connector (Inside BMS)

![image](https://github.com/user-attachments/assets/766296e9-1509-488d-abde-d4a849110c91)

# Example Wiring Diagram
Since the integration has not figured out how to close contactors,we let the Battery-Emulator take control over the contactors instead.

This diagram assumes using manual contactor control (you will have to make up a 4 pin passthrough inside the battery from the SME/BMU to the spare pins on the external case). Pinout for the contactor connector is above. You can combine all 3 negative contactor connections, leaving the other 3 spare pins for the individual positives. 2WD and 4WD can vary for CANFD pinout - so check!

> [!IMPORTANT]
> You need a high current capable 12V supply. If you are powering the BMS via the Stark CMR, you need to power it via the 7A capable Precharge circuit, see the Stark Wiki for more info

![example wiring](https://github.com/user-attachments/assets/341cbda5-2fc5-46bf-a674-2aa203236b29)

## Note on Diagnostic trouble codes (DTC)
You can read active DTCs via the More Battery Info page. Note that some code will always be active, plus if your battery has been crashed in the past there will be more codes.

Example, working setup with balancing confirmed, on a crashed pack. These DTCs still active

<img alt="image" src="https://github.com/user-attachments/assets/234fceae-4204-4b25-b1d4-d31c3dd57e86" />

