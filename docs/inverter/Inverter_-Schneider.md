> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

## Compatible Schneider inverters
* Schneider XW Pro Hybrid Inverter :warning: Testing ongoing SCHNEIDER_CAN

## Communication wiring
The Schneider inverter works via CAN. The CAN connection is done at the Gateway

![image](https://github.com/user-attachments/assets/06483be2-bd7e-4b18-88d9-b7f9a15393c2)

![image](https://github.com/user-attachments/assets/38fffb7d-52c2-4784-81fa-e0e7c8ee162a)


ℹ️ Always check the termination resistance of the system! That way you know if resistor needs to be removed or not.

ℹ️ Grounding is extremely important. Make sure the battery case is connected to protective earth, and the shield part of the twisted pair CAN is connected to PE also! Failing to do this will result in CAN errors.

## Which protocol to use
For this inverter type, use the option called "Schneider V2 SE BMS CAN" under the "Inverter Protocol" setting

<img width="493" height="71" alt="image" src="https://github.com/user-attachments/assets/169f796f-2cd7-440b-b942-3906049db6d9" />

## Installation examples
