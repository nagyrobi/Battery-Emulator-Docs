> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

## Battery general info
Maxus eDeliver 3.
There are different battery models:
- 35kWh / 52.5kWh NCM by UABC "United Auto Battery System Co.,Ltd" (model year 2020, 2021)
- 50,2 kWh LFP by CATL "Contemporary Amperex Technology Co., Limited" (model year 2022+)

<img width="1128" height="756" alt="image" src="https://github.com/user-attachments/assets/96bab589-d2db-41c9-a416-c261397a09bb" />

<img width="1128" height="756" alt="image" src="https://github.com/user-attachments/assets/9cf1935f-f3c4-4d5d-8246-b801dc001365" />


## Battery overview

<img width="1333" height="697" alt="image" src="https://github.com/user-attachments/assets/ed13e82a-ce69-4ed3-8c56-6ba49d4b528f" />

Left to right,  Low voltage connector, service disconnect switch, high voltage connector

## Low voltage wiring

Apart from the HV plug, the BMS has two plugs "A" and "B", A seems to be responsible for charging, B for everything else.
Service manual wiring attached, (PT CAN = PowerTrain CAN):

<img width="447" height="367" alt="image" src="https://github.com/user-attachments/assets/3d47cf3d-8763-471e-a222-615e2539e93a" />

Other BMS connections
<img width="1542" height="645" alt="image" src="https://github.com/user-attachments/assets/da2eab9e-3c1f-4f56-8f1a-c51781dfc7cb" />

The battery has an interlock on Connector B that needs to be shorted
<img width="583" height="518" alt="image" src="https://github.com/user-attachments/assets/e36ccdf4-4171-4a8b-9f04-5cf5f125493a" />

### How do I connect the battery to the Battery-Emulator?
TODO
- Connector B: Pin 3 to Pin 2 to satisfy interlock detection

## High voltage wiring
Cable for heater?

<img width="974" height="372" alt="image" src="https://github.com/user-attachments/assets/8fe3d85f-d52c-457b-9a3f-5adabc5bef55" />

