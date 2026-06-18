> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

## Compatible Ingeteam inverters
* Ingeteam STORAGE 1Play TL M (3-6 kW)

## Communication wiring
The Ingeteam inverter works via CAN. The LilyGo board can have both a CAN battery and a CAN inverter connected on the same pins. When the board is used with two CAN devices at the same time that have termination resistors in all ends, the terminating resistor needs to be removed from the board. Please measure CAN termination if you have issues. This is explained in [CAN-troubleshooting](https://github.com/dalathegreat/Battery-Emulator/wiki#can-wiring-troubleshooting)

ℹ️ Always check the termination resistance of the system! That way you know if resistor needs to be removed or not.

ℹ️ Grounding is extremely important. Make sure the battery case is connected to protective earth, and the shield part of the twisted pair CAN is connected to PE also! Failing to do this will result in CAN errors.

## Which protocol to use
For this inverter type, use the option called "BYD Battery-Box Premium HVS over CAN Bus" under the "Inverter Protocol" setting

<img width="484" height="68" alt="image" src="https://github.com/user-attachments/assets/a85dbc6b-0464-4176-bddd-21a719f9ea15" />

## Installation examples
A completed integration using LEAF battery and an Ingeteam inverter:
![bild](https://github.com/dalathegreat/BYD-Battery-Emulator-For-Gen24/assets/26695010/e0b5654f-0140-4e12-b884-e1796807abf0)

