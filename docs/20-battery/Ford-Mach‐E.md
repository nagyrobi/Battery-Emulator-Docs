## Ford Mach-E / E-Transit wiki

## Battery specifications
A checkbox indicates that the battery has been tested and confirmed working

- Mach-E Model years 2020-2022
   - 75.7 kWh
   - 98.8 kWh
- Mach-E Model years 2023-present
   - 75.7 kWh NMC
   - 78.2 kWh LFP 600kg ✅
   - 98.8 kWh NMC
- E-Transit Model years 2022-present
   - 66kWh net, 77kWh gross ✅
   - 89kWh net :question: 

<img alt="image" src="https://github.com/user-attachments/assets/f03fcded-204f-45e7-8a74-b4bff9bd23f5" />

75kWh battery, from backside, showing only one HV connector


## Wiring diagrams

[Powertrain_2.pdf](https://github.com/user-attachments/files/18338107/Powertrain_2.pdf)


[Powertrain_7.pdf](https://github.com/user-attachments/files/18338108/Powertrain_7.pdf)

[Socket C4239.docx](https://github.com/user-attachments/files/18338109/Socket.C4239.docx)

[Systemblock diagram:](https://github.com/user-attachments/files/22634878/Beschreibung.H.System.pdf)

## Wiring diagram, Low Voltage

Warning - If BE powers down with contactors closed the contactors remain closed. Use Relay or Stark CMR to supply power to the BECM (BMS) to power down the BECM if Battery Emulator goes down. 

A replacement LV connector can be purchased from AliExpress. 
https://www.aliexpress.com/item/1005008121256506.html

Detailed LV connector C144 pin description

[C144.pdf](https://github.com/user-attachments/files/22634843/C144.pdf)

<img width="450" alt="image" src="https://github.com/user-attachments/assets/ac6e2c64-678c-4310-9651-18acdffaf2c0" />

<img width="450" alt="image" src="https://github.com/user-attachments/assets/df23902c-92b0-49ae-a159-34fbd98cba8a" />

[<img width="900" alt="MachE-2 SMA inverter setup" src="https://github.com/user-attachments/assets/1f7fc5c2-b6e8-434a-ae73-afe1f3cc7d83" />](https://github.com/user-attachments/assets/1f7fc5c2-b6e8-434a-ae73-afe1f3cc7d83)


For communication only:
Connect the following pins:
- Pin 1 to 12V constant (BMS+)
- Pin 6 to 12V constant
- Pin 26 to GND for 12V
- Pin 15 or 21 to CAN-L on Battery-Emulator
- Pin 16 or 22 to CAN-H on Battery-Emulator

For communication AND contactors:
Connect the following pins:
- Pin 1 to 12V constant (BMS+)
- Pin 5 to 12V constant
- Pin 6 to 12V constant
- Pin 8 to 12V constant (Closing battery contractors)
- Pin 25 to GND for 12V
- Pin 26 to GND for 12V
- Pin 15 or 21 to CAN-L on Battery-Emulator
- Pin 16 or 22 to CAN-H on Battery-Emulator

Tip: use pin 15 and 16 for CAN Communication. Use pin 21 and 22 to add a 120Ω terminating resistor

Battery uses 1.4 amps to run the 12v 1mm2 cable should be enough.

Note: The battery does NOT contain a terminating resistor, so it is a good idea to add a 120 Ohm resistor between CAN-L and CAN-H on the battery side.



## Wiring, High voltage

<img width="930" height="382" alt="image" src="https://github.com/user-attachments/assets/ce63fdbd-bc2e-4a31-98d5-503f03909bfa" />

### High voltage interlocks
Jumpers need to be fitted on interlock detection pins, to make the battery believe the HV connectors are seated OK. The largest DC fast charge port does NOT need jumpering.

<img width="547" height="274" alt="image" src="https://github.com/user-attachments/assets/a7fd6411-7028-40da-a510-c9b1c0c627e8" />

## High voltage Isolation Monitoring

Depending on your inverter choice high voltage isolation monitoring of the pack will cause a DTC. If the 12 V side is grounded to earth through its power supply this can result in the BMS stopping communication. This will then cause the inverter to error out.

To avoid the issue use Y safety capacitors between the high voltage and ground. Under test 10nf 400v 

## Faults stopping pack use (UNDER DEVELOPMENT)

If the battery 12 V is powered up with an interlock not connected when you try to get the contactors to close they will not. 

This requires the DTC error clearing and then contactors will immediately close. DTC clear can be accessed from the More Battery Info page

<img width="376" height="81" alt="image" src="https://github.com/user-attachments/assets/2c32c0c3-6bac-420a-904c-d2bb76788970" />

DTC Reading is still under development

### DTC descriptions
The following DTCs have been decoded

- B11D5 - Restraints Event - Vehicle Disabled
- U0100 - Lost Communication With ECM/PCM A
- U019B - Lost Communication With Battery Charger Control Module 'A'
- U0140 - Lost Communication With Body Control Module
- U0146 - Lost Communication With Serial Data Gateway Module 'A'
- U0293 - Lost Communication With Hybrid/EV Powertrain Control Module 'A'
- U0298 - Lost Communication With DC/DC Converter Control Module 'A'
- U027C - Lost Communication With Off-Board Charger Control Module
- U0594 - Invalid Data Received From Hybrid/EV Powertrain Control Module 'A'
- P0C44 - Hybrid/EV Battery Pack Coolant Temperature Sensor 'A' Circuit Low
- P0A06 - Motor Electronics Coolant Pump 'A' Control Circuit Low
- P0AA6 - Hybrid/EV Battery Pack 'A' Voltage System Isolation Fault
- P0AA7 - Hybrid/EV Battery Pack 'A' Voltage Isolation Sensor Circuit
- P1A42 - Propulsion System Status Signal Performance
- U3001 - Control Module Improper Shutdown Performance
- U3003 - Battery Voltage
- U351B - High Voltage System Interlock Circuit 'D' Low
- P1A0F - Hybrid Powertrain Control Module - Vehicle Disabled
- P1A43 - Hybrid/EV Battery Contactor Request Signal Performance
- P0C48 - Hybrid/EV Battery Pack Coolant Pump 'A' Control Circuit Low
- P1627 - Module Supply Voltage Out Of Range

#### Status codes
Status (-2F):
 - DTC Present at Time of Request
 - Malfunction Indicator Lamp is Off for this DTC

Status (-AF):
 - DTC Present at Time of Request
 - Malfunction Indicator Lamp is On for this DTC

Status (-2C):
 - DTC Maturing - Intermittent at Time of Request
 - Malfunction Indicator Lamp is Off for this DTC

Status (-28):
 - Previously Set DTC - Not Present at Time of Request
 - Malfunction Indicator Lamp is Off for this DTC

## 3D Prints

4 Pin C295 DC/DC Converter connector
[DC Cover 1 +5mm.zip](https://github.com/user-attachments/files/28693976/DC.Cover.1.%2B5mm.zip)
