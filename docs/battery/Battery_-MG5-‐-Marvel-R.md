# Current status


| Car | kWh | Chemistry | Battery type  | Part number | Status |
|----------|----------|----------|--------|----------|---------|
| MG5  | 52.5  | NMC  |    EU150A52S  | 10847655 | Tested and working |
| MG5  | 61    | NMC  |    EU174A61S  | 11163326 | Not tested |
| MG5  | 50.3  | LFP  |    X  | x | Not tested |
| MG Marvel R  | 69.92 | NMC  |  EU199A69S |           | Reported working |
| MG Marvel R  | 69.9  | NMC  |  EU199A70S | 10953172  | Reported working (v10.11+) |

Until it is merged, the [this branch](https://github.com/dalathegreat/Battery-Emulator/pull/2417) may be needed with some MG5 (and MarvelR/ZS packs).

# MG5 Batteries

There are three types of batteries found in the MG5, a 52.5 kWh NMC, a 61.1 kWh NMC and a 50.3 kWh LFP pack, see details below.
<img width="1059" height="311" alt="image" src="https://github.com/user-attachments/assets/c6d1ebad-0595-4eaf-8d86-4e48fa996611" />

You can recognize the battery by the checking the label of the battery, the cell capacity and kWh is given, see photo below.
![thumbnail_PXL_20251107_214138973 MP1](https://github.com/user-attachments/assets/0b094141-5b2d-4c70-831c-e14ff050df7a)

You can also recognize the different types by the cooling in/outlets. For the 50.3kWh and 61.1 kWh battery they are located right next to the connectors on the EDM(the connector extension coming out of the battery), see below:

<img width="791" height="460" alt="image" src="https://github.com/user-attachments/assets/810c689f-2c39-4e01-a9b6-92c907a785c8" />



While for the 52.5 kWh battery they are farther way, not on the EDM:

<img width="1054" height="595" alt="image" src="https://github.com/user-attachments/assets/d4eb6ff4-0dbf-4cb6-968f-9ae6bac74ac7" />

# MG Marvel R Batteries

<img width="1428" height="398" alt="image" src="https://github.com/user-attachments/assets/1ed1f68e-69d6-41dc-b7b3-549a731b0dce" />



# Hardware setup

There are four connectors on the battery:

The Manual service disconnect(MSD): this is just a plug that inhibits the closing of the relays when not present. It needs to be removed when doing maintenance, always remove it when you are working on the battery yourself.

The auxiliary HV connector: It is only present on the 52.5kWh battery. It supplies the PTC battery heater unit. We dont need it. I have potted it with epoxy and put a 3d printed cover over it. This connector has  two HVIL (high voltage interlock) pins that signal the car when the HV connector is connected. Since the HVIL loop is not check by the BMS in the battery, it is not needed to short the pins in this connector.

The high voltage connector(HV009): This is the main HV connector from the battery to the car. It has three pins, a positive pin, a negative pin and another negative pin specifically for fast charging. Since we don't use the fast charging connection, we only need to connect the normal positive and negative pin. Although they look very similar,  the type of HV connector for the 52.5 kWh battery is different from the other two, they don't fit on each other.
The 52.5 kWh battery has an amphenol HVC3P80MV108227U19 connector, it fits with a high voltage cable MG part number 10824432.
The 50.3 kWh and 61.1 kWh batteries should fit with HV cable MG part number 10863642(to be confirmed)

<img width="1469" height="600" alt="image" src="https://github.com/user-attachments/assets/b5434ca3-945c-408a-961c-a227726be6d2" />

The low voltage connector(EB212): This connector is the same for all battery versions. It fits the molex connector part 643193211. You can either assemble the connector on your own with crimp terminals(64322 and 64323) and plugs(643191201 and 643251010) and a cap(643191201) or you can buy a premade connector from alieexpress.

<img width="1064" height="1003" alt="image" src="https://github.com/user-attachments/assets/10684d0f-05b6-4fe4-a745-9780b39205a2" />

To make the battery work, you need to connect:

+12V to the KL30 power pins G1 and G4, this provides power to the BMS and the EDM(relay control).
Ground to the ground pins H1 and H4. 
+12V with a 1k resistor to wakeup enable pin D3, this will wake up the BMS.
CANH to the PT CAN H and Hybrid CAN H pins C1 and A1. 
CANL to the PT CAN L and Hybrid CAN L pins D1 and B1. 

The battery draws approx 250mA when only the BMS is active and about 500mA when the relays are closed, to close the relays you need approx 3A inrush current.

There are three CAN buses on the battery:

PT CAN(powertrain CAN): this is the main CAN bus of the car that manages the communication between the complete powertrain. it is 500 kb/s. This CAN bus responds to UDS requests and we can read the battery current and voltages from it.

Hybrid CAN: This bus communicates between the CCU(the cars internal charger) and battery. We need it to receive the CAN messages to close the contactors. This bus is also 500kb/s. Since we need both PT CAN and Hybrid CAN we both connect them to together to the BE. 

Fast charge CAN: this CAN bus  sends data during DC fast charging to the EVC(electronic vehicle controller) which translates it into hardware signals to the fast charging station. This bus is 1Mb/s and uses extended CAN ID's. We dont need it since we are just using the slow charging pathway.

Here below an example connection diagram. It uses an external HV DC/DC that converts the 400V to 12V to charge a lead acid car battery.
<img width="2066" height="600" alt="Screenshot From 2026-01-08 23-02-56" src="https://github.com/user-attachments/assets/043a6f54-27b4-4640-a505-479bffd74fd8" />


# sofware configuration

The MG5 code only runs on hardware that has more than 4Mb of flash memory.

You need to select the MG5 battery and select the correct battery chemistry NMC or LFP and battery interface. Then configure the capacity and all the other options as you want. In order to use the DTC commands you need to enable logging via webserver.

The pause button works, although only if the current is bigger than 1.8A. The open and close contactor button also work. 
In the battery specific options the contactors can also be controlled. This will however not update the contactor state in the  main webserver screen, it is best to verify the contactor state in the logging tab. When logging is enabled, it should always send a message when the state changes, connected=contactors closed, disconnected or something else=contactors open. But be sure to also verify by measuring before connecting/disconnecting any cables.

There is also an option to request the DTC errors and clear the error codes, this is interesting for debugging purposes. When the button is pressed, the BE will request the DTC error codes of the battery, you can see them in the logging screen. It will show the status of each error code, to get the explanation of each DTC code you check the battery diagnostics manual. Whenever a request is made to close the contactors, the BE will also automatically clear all the error codes.

<img width="1666" height="348" alt="image" src="https://github.com/user-attachments/assets/5bfccdaf-ea07-465e-b917-fc05d7b92d20" />

<img width="1362" height="1528" alt="image" src="https://github.com/user-attachments/assets/cd863cc6-469e-4be0-9e00-b03c03c206ef" />









