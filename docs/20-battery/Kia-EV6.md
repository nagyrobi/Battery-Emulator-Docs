> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.


> [!IMPORTANT]  
> The EGMP battery platform cannot communicate over CAN. It only supports CAN FD!


Pinout on battery
![image](https://github.com/dalathegreat/Battery-Emulator/assets/128810834/1d56606e-12bf-4614-af20-1a6902c143ea)
![image](https://github.com/dalathegreat/Battery-Emulator/assets/128810834/3ea55e29-2f9c-4ffc-a8bd-760ef8c538c1)

![image](https://github.com/user-attachments/assets/8e4c6cac-1be5-4ddb-a2a1-3f3a55e44650)
![image](https://github.com/user-attachments/assets/7b7d36f0-74bb-48e6-9698-83bb1b76e315)
![image](https://github.com/user-attachments/assets/bd9c66ab-d07b-4e7e-b50c-8e8be3f65d49)




Batterie Data (from Kia/Hyundai Service Manual)
![image](https://github.com/dalathegreat/Battery-Emulator/assets/128810834/3fbf2eb3-d95b-4469-bd30-177b0b295695)

Batterie BMU/CMU Info
![image](https://github.com/dalathegreat/Battery-Emulator/assets/128810834/8bde9eaf-88d7-4c64-93e5-9c8c5bc2d2b7)
![image](https://github.com/dalathegreat/Battery-Emulator/assets/128810834/080e79b7-0387-41cf-ac85-28bc0559590f)
![image](https://github.com/dalathegreat/Battery-Emulator/assets/128810834/1b6a7d9c-0701-43fa-82c7-eca76dbdee3a)
![image](https://github.com/dalathegreat/Battery-Emulator/assets/128810834/b4ef2aeb-7ba8-4979-b46b-0cee2df91a18)
![image](https://github.com/dalathegreat/Battery-Emulator/assets/128810834/e3ff382f-49c2-4de0-83b6-d6898a939cf4)
<img alt="Zrzut_ekranu_2025-07-16_111425-1" src="https://github.com/user-attachments/assets/7de7052d-973a-432d-a1fd-fffabd6ebde0" />
<img alt="Zrzut_ekranu_2025-07-16_111419" src="https://github.com/user-attachments/assets/a04d09fb-d5f0-4d83-8f1f-5ba11107497a" />


Images etc.

Similar wiki: [E-GMP platform](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Hyundai-E%E2%80%90GMP-platform-(58.2-%E2%80%90-77.4-kWh))

***** Below find pin connection required ******
 1,2,12 - 12v / 
 3+14 - shunt / 
 7+8 - resistor / 
 10+11 - can-fd (to MCP board) / 
 13 - 5v / 
 24+25 - shunt / 
 27+28 - resistor / 
 29+30 - resistor / 
 31+32+33 - gnd / 
 
You also need to shunt the 3 interconnect in the 3 HV connectors. You can measure if the shunt is connected correctly with a voltmeter. If closed correctly the shunt should measure 2.5V