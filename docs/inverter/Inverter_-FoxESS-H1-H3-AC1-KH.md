> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

# ⚠️ Word of caution, CAN overvoltage ⚠️
FoxESS inverters can have high voltage potential on the CAN chip. They can be 110V when measuring between CAN and PE. It can burn up your Battery-Emulator CAN chips if there is a path to protective earth. This becomes a problem if the board you are using has GND on the same plane as PE. Then the 110V diff might leak over and damage the chips. A way to avoid this is to use a PSU to power the Battery-Emulator board that is not connected to PE. For instance a 2-prong phone charger would effectively be isolated from PE. 
<br>Note: This does not impact the lilygo T-2CAN, it is galvanically isolated, Foxess cannot fry the T-2CAN!

![image](https://github.com/user-attachments/assets/6e4efc3d-6839-4834-a703-8adca89a8403)

Another way to tackle this is with the use of a CAN isolator between the inverter and the rest of the system. Examples found in the [lightning strike wiki](https://github.com/dalathegreat/Battery-Emulator/wiki/Lightning-strike#suggested-hardware)

![image](https://github.com/user-attachments/assets/2ba857f6-d2aa-48e4-94c4-3314b7b7ff4e)

# Word of caution, isolated CAN

> [!IMPORTANT]  
> This inverter does not handle a CAN connected EV battery on the same channel.
If the inverter which likes to see only extended CAN frames sees standard automotive CAN frames, the inverter will enter a fault state.

This can be solved in a few ways:

   - One option is to use [add on MCP2515](https://github.com/dalathegreat/Battery-Emulator/wiki/CAN-add%E2%80%90on-(MCP2515)) board
   - Another options is to use [add on CAN-FD MCP2518](https://github.com/dalathegreat/Battery-Emulator/wiki/CAN%E2%80%90FD-add%E2%80%90on-(MCP2518FD)) board 
   - Third option is to use [Stark CMR hardware](https://github.com/dalathegreat/Battery-Emulator/wiki/Hardware:-Stark-CMR)
   - Fourth option is to use a [CAN filter](https://github.com/dalathegreat/Battery-Emulator/wiki/CAN-filter-hardware) between inverter and the rest of the system 

## Compatible FoxESS inverters
* FoxESS H1
   * Use `FoxESS compatible HV2600/ECS4100` primarily.
   * Can also use `SolaX Triple Power LFP over CAN bus` protocol, but some values will be wrong
* FoxESS H3
   * Uses `FoxESS compatible HV2600/ECS4100` protocol
* FoxESS AC1
   * Uses `SolaX Triple Power LFP over CAN bus` protocol
* FoxESS KH
   * Works with both `SolaX Triple Power LFP over CAN bus` and `FoxESS compatible HV2600/ECS4100` protocols
* FoxESS KP
   * Uses `FoxESS compatible HV2600/ECS4100` protocol
## Communication wiring
The FoxESS inverter works via CAN. Connect the Inverter side CAN-H & CAN-L to the Battery-Emulator

> [!IMPORTANT]  
> Different versions of the Foxess have different pinouts. Check user manual to see which pins are CAN-H / CAN-L

![image](https://github.com/user-attachments/assets/60c3b180-035d-4ebe-820c-83edeae1625e)

## Which protocol to use
For this inverter type, use the option called "FoxESS compatible HV2600/ECS4100" under the "Inverter Protocol" setting

<img width="495" height="63" alt="image" src="https://github.com/user-attachments/assets/4ef7e184-0b84-4296-96b7-de925626be7d" />

## Note on smartmeter

Note in the CHINT DDSU666 (single phase) meters with the fox. _I bought a generic meter off a UK electrical wholesaler initially as the FoxESS instructions didn’t say it had to be a Fox branded version. It read accurately on the meter, but gave rogue readings to the inverter so was inoperable. I just installed a FoxESS branded meter and it works perfectly out the box (settings 8n1, Ch01, 9600)_

<img alt="image" src="https://github.com/user-attachments/assets/f611bdd0-c9ec-4614-8a76-3e16e6536bc7" />

<img alt="image" src="https://github.com/user-attachments/assets/59e0dd1f-2c1a-4775-850c-a9fa5d67dc11" />


## Troubleshooting

|  Problem |  Possible fix |
| :--------: | :---------: |
| Inverter stuck in "Waiting..." | Check that high voltage is present on inverter terminals, and that polarity is right way |
| Event CAN_INVERTER_MISSING active | Check that can_config is set properly for .inverter . It also might be a good idea to restart the inverter itself. It sometimes does not recover the startup routine if you have disconnected wires on the fly |
| Inverter switched off ("Switch off" on screen) / Inverter not using battery | ‘long press to activate’ on the inverter screen. Countdown will begin and inverter will start once it finishes |
| Inverter keeps losing internet/wifi connection | If you have early firmware running on an H1-G2 inverter (e.g. after a factory reset), some users have reported issues with flaky connections when using the official WiFi adaptor (also known as the datalogger). The symptom (as well as it dropping off the network) is the status light on the datalogger going from a slow blink (connected) back to rapid flashing (connecting). The solution is to use Fox ESS cloud platform to update your inverter and datalogger to newer firmware, however this cannot be achieved while Battery Emulator is connected. First disconnect the BMS port, then use the cloud platform as an installer to push firmware updates to the inverter. There are not currently any known issues running the latest Fox ESS firmware. Once you have updated you can reconnect the BMS port to Battery Emulator and your WiFi should be stable |
| Unable to force charge after inverter firmware update | Remote control has been disabled as part of an update. On the inverter, go in to Settings -> Feature -> Remote Control and select **Enable** |
| Inverter reports "Bat Volt Fault" | Can happen when voltage sags under load. Common cause is miswired high voltage lines/precharge, pulling power thru precharge resistor instead of contactors. Especially likely wiring mistake to make on Renault setups. |

## Installation examples
![alt](https://i.postimg.cc/yYzdDqbW/Whats-App-Image-2026-05-06-at-16-41-16.jpg)
FOX H1-6.0-E and renault zoe 26 kwh
Feel free to add your own images here!


# Video Guide
To aid installation Battery Man has produced a video series using the H3 Pro inverter which documents an install with Tesla LFP batteries and both the LilyGo and Stark CMR. There is a full playlist touching on different aspects
![thumb for YT opt 7 FINAL](https://github.com/user-attachments/assets/2fdf5561-852d-453f-b5a5-23e1211fd5f1)
Installing inverter - https://youtu.be/9YnuPMdJaoI?si=odCptB7YAE56yFHq
![THUMB HACK V3](https://github.com/user-attachments/assets/c9c16522-520f-46db-8cfb-8c3b4558bacd)
Work to add inverter integration - https://youtu.be/BYqYpsv5svQ?si=oxXq-E-KLXL0grea
https://youtu.be/PYyTD87KQpo?si=jaSvQWiqEct-WYPS