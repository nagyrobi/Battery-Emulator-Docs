## Read this first
Preface, the entire Battery-Emulator project sets out to achieve safe re-use of EV batteries. By building your own battery, you will be taking larger risks. Cell balancing wire taps, shunts, busbar connections, BMS integration, temperature monitoring, fuses, contactors, interlocks, etc. all will have to be implemented by yourself instead of using a pre-made product. As with all things custom, there are higher risks of human error. This page covers emulating High Voltage protocols, so while most tinkerers might be familiar with 48V DIY batteries, building a 100S or more 400VDC battery is an entirely different beast that can be lethal. Take extra precaution when working on a custom DIY HV battery, you have been warned.

## Caution
If you are unsure of your technical knowhow, avoid building a high voltage battery from scratch

## Custom DIY battery with EMUS G1 BMS
The Battery-Emulator has support for the EMus G1 BMS. 
With this BMS you can construct your own high voltage battery, and connect the BMS via CAN to the Battery-Emulator. 
This allows you to use a DIY battery (instead of an EV battery) with any normal Battery-Emulator supported inverter.

## Where do I get the hardware?

www.emusbms.com

## Setup

Emus G1 BMS needs to be configured to emulate Deye_hv_can inverter protocol

<img width="580" height="136" alt="image" src="https://github.com/user-attachments/assets/472fc81d-afee-4a5b-ac65-17e7bcd8ad47" />

Set CAN ID base to 0x19B5

<img width="338" height="303" alt="image" src="https://github.com/user-attachments/assets/739966f2-a41d-453c-ba8b-8d26f312b2c5" />

Set Battery type to Pylon and baud rate to 250

<img width="538" height="310" alt="image" src="https://github.com/user-attachments/assets/359543a1-dae9-4bc7-9801-a7ebf884be59" />

Battery Emulator will show all needed information and also populate cellmonitor page with individual cell voltages and if they are balancing or not.

