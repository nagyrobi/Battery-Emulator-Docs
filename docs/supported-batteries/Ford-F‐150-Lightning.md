> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

## Compatible batteries
The model years 2022-2025 came with the following batteries
- 107kWh gross (98kWh net usable) 
- 143kWh gross (131kWh net usable)

There are stickers on the battery that informs gross capacity

<img width="1018" height="303" alt="image" src="https://github.com/user-attachments/assets/1bbadf7f-5592-4210-ae8a-04579b18c643" />

### Physical Dimensions

| Parameter | Value |
|----------|-------|
| Pack Size (L × W × H) | <!-- e.g. 2400 × 1500 × 150 mm --> |
| Weight | <!-- e.g. 540 kg --> |

> **Battery holder frame design:** <!-- Link to frame design if available -->

## Software configuration
For this battery type, use the option called "xyz" under the "Battery Protocol" section

## Part numbers 
Part numbers for connectors/cables, along with purchase links to ebay/aliexpress

| Component | Part Number | Purchase Link |
|-----------|-------------|---------------|
| <!-- e.g. HV Connector --> | <!-- e.g. TE 123456 --> | [eBay](#) / [AliExpress](#) |

## Wiring, Low voltage connector

<img alt="image" src="https://github.com/user-attachments/assets/02e0e14c-9cdb-40f9-8985-2713b82f1f70" />

A replacement LV connector can be purchased from AliExpress. 
https://www.aliexpress.com/item/1005008121256506.html

Detailed LV connector C144 pin description

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

| Parameter | Value |
|----------|-------|
| 12V Consumption — Peak Start | 2.0A |
| 12V Consumption — Continuous | 1.5A |
| CAN type | CAN |
| Contactor Control | CAN-controlled |

## Wiring, High voltage connector

<!-- Add a photo of the HV connector and a wiring diagram below -->

| Parameter | Value |
|----------|-------|
| Interlock Required | Yes |
| Number of Interlocks | <!-- e.g. 2 --> |

The interlocks on these connectors need to be seated:

## Troubleshooting tips

<!-- Document common issues and their solutions -->

## Example picture from completed install

<!-- Add a photo of a finished installation for reference -->