### WIP

### Geely Geometry C
There are two variants of the Geometry C battery

- 53kWh CATL NCM xxxV Nominal (xxxkg) 268.8~417.6V operating range
- 70kWh CATL NCM 374V Nominal (395kg) 285.6~443.7V operating range

![image](https://github.com/user-attachments/assets/05151684-7736-4f99-bce8-7540da7cfceb)

## Software configuration
For this battery type, use the option called "Geely Geometry C" under the "Battery Protocol" setting

<img width="591" height="113" alt="image" src="https://github.com/user-attachments/assets/fbe82e6b-d9ab-4af4-b720-dd45cb3fdfde" />

### Wiring diagram

![image](https://github.com/user-attachments/assets/51a1f649-fca4-4058-9f0a-e4b0c1319203)

![image](https://github.com/user-attachments/assets/ac889dd1-5b83-406c-b186-5d560dd473ae)

### LV and HV connectors
The battery has 2x HV outputs, and 2x LV connectors. The top LV connector (A) is the main to use, which has the HB CAN-H/L. The bottom one has the fastcharging CAN (not required for stationary usage)

The battery contains a terminating resistor for HB-CAN

![image](https://github.com/user-attachments/assets/d4fcbeea-5121-489d-926d-5f187d29b6ff)

?The interlock signals on both HV connectors need to be shorted together?

|  BMS pin |  Signal Type |  Note |
| :--------: | :---------: | :---------: |
| 1 |  12V + | Connect to permanent 12V supply |
| 2 |  Ground  | Connect to ground for 12V supply |
| 3 |  HB CAN-H  | Connect to Battery-Emulator CAN-H |
| 4 |  HB CAN-L  | Connect to Battery-Emulator CAN-L |
| 5 |  Ground  | Connect to ground for 12V supply |
| 6 |  Collision Signal  | Not connected! |
| 7 |  ACC  | Connect to permanent 12V supply |
| 9 |  Fast charging socket PT1000+  | ??? Not connected ??? |
| 10 |  Fast charging socket PT1000-  | ??? Not connected ??? |

![image](https://github.com/user-attachments/assets/1b91b26f-84c9-4abb-af4c-fdd2aa40f190)
