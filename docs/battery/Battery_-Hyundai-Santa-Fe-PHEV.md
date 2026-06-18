> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

# Hyundai Santa Fe PHEV battery
Information on re-using the 13.8 kWh lithium-ion polymer battery out of a 2018-Present Hyundai Santa Fe **PHEV** vehicle

ℹ️ The pack is separated in two packs, be sure to get both pieces!

![bild](https://github.com/user-attachments/assets/cf0fe2a8-f87c-45ff-8ee5-0b8cc4293b67)

# Communication wiring
![bild](https://github.com/dalathegreat/BYD-Battery-Emulator-For-Gen24/assets/26695010/ae128163-d557-4e62-95fc-3feee75cd099)

ℹ️ Note, the P-CAN and H-CAN networks need to be joined together. The Battery-Emulator needs to send messages towards both CAN buses for the battery to be able to close contactors.

Connect the following:

- P/H-CAN High -To LilyGo CAN-H
- P/H-CAN Low - To LilyGo CAN-L
- Pin 1 & 2 - To 12V supply
- Pin 3 & 14 - Short together by installing the Service Plug
- Pin 33 & 32 - To GND for 12V supply
- Pin 12 - To 12V Supply (this is the wakeup signal) 

![bild](https://github.com/user-attachments/assets/5eef57fc-0951-4a0a-8ff0-24c0061a00cb)

## Software configuration
For this battery type, use the option called "Santa Fe PHEV" under the "Battery Protocol" setting

<img width="603" height="74" alt="image" src="https://github.com/user-attachments/assets/f37188ae-3a8d-412d-be92-f23455b1715c" />

# Credits
Credits go to maciek16c for the CAN findings!
Massive thanks to GoSmart on the Discord for testing this!
https://github.com/maciek16c/hyundai-santa-fe-phev-battery
https://openinverter.org/forum/viewtopic.php?p=62256