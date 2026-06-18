### CAN wiring troubleshooting

## CAN termination practices

<img width="461" height="149" alt="image" src="https://github.com/user-attachments/assets/76d561dc-78cc-443d-b21f-dbce9abe433d" />

> [!IMPORTANT]  
> CAN wires need to be twisted pair to ensure signal integrity!

Every CAN bus must be terminated with a 120 Ohm resistor at each end of the bus. For quick testing, the exact value of the termination resistors is not always critical. Sometimes a single terminator is sufficient. For final installs, proper termination is essential. If you see strange errors, you should check the termination.

> [!IMPORTANT]  
> To save yourself a lot of trouble, always terminate the CAN bus properly.

If you get events like  BATTERY_MISSING or INVERTER_MISSING, you need to check the CAN wiring. Here are some basic tips:
* Make sure the Webserver settings is configured correctly, with the right CAN configuration. Example, make sure the Battery is configured to use the correct CAN interface if you see BATTERY_MISSING, for instance if you use LilyGo T-2CAN and connect the battery to CAN-A, configure that port under the Battery Interface.
* Make sure you have flashed the correct software onto your Battery-Emulator board (T-CAN485 instead of T-2CAN is a classic mistake)
* Make sure all CAN devices are turned ON. Verify that the device you are troubleshooting is getting 12V, and that the device is consuming power (typically a few 100mA)
* Verify that the 12V source provides enough voltage. 13.5V is good, 11V will cause issues
* Make sure the polarity of High/Low is correct. High goes to High, Low to Low
* Make sure the terminating resistors are correct. CAN networks should have two 120 Ohm resistors in each end of the network. With everything OFF, you can measure resistance between CAN-H and CAN-L. The result should be 60 Ohm. If it shows 120 Ohm, one resistor is missing at an end. If it shows 40 Ohm, you have too many terminating resistors.
* On the LilyGo, the termination resistor can be removed like shown here:
![RemoveThisWhenUsingCANinverter](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/eecda31b-4ce0-4e96-a696-ee78cef3ada2)
* On the Stark CMR board, the terminating resistor can be turned on/off via the switches on the PCB
![image](https://github.com/user-attachments/assets/daca8587-54e0-4ee7-b600-6a5d58059d77)
* Make sure the cable you are using is a twisted pair cable. This is important for signal quality.
* Make sure the cable you are using is shielded. One side of the shield should be connected to a pin labelled SHIELD (or PE if no dedicated shield exists). This improves signal quality
* Try a different powersupply for the board. Powering it via USB from a computer can cause noise on the signal output. Powerbank or phone charger might have cleaner voltage output. If you see CAN_TX_FAILURE occasionally your powersupply might be noisy
* If you are using a board like T-2CAN, it requires 12V to activate the CAN chips, simply powering it via USB is not enough!

### Loopback testing
To verify that the CAN channels on the Battery-Emulator hardware you have are working properly, you can perform a loopback test. This can be done by connecting two CAN channels together

Example, loopback test on Stark CMR to verify that both hardware channels are OK
<img alt="image" src="https://github.com/user-attachments/assets/f1be5044-f2bb-49a2-b2e3-6a399ecde0b3" />

In the Software, enable the following options as a test,
Sono Motors on CAN1, Schneider Inverter on CANFD2

<img alt="image" src="https://github.com/user-attachments/assets/adf438dd-6106-471c-b922-4896cda46c3b" />

Save and reboot. Open the "CAN logger" page

<img alt="image" src="https://github.com/user-attachments/assets/a49e9193-47bd-4fa7-a6c4-20b9d95c37d4" />

You can see  both RX0/TX1 (CAN) function, and RX6/TX3 (CAN-FD). In this successful test both CAN buses can send and receive, since both are visible in the log :heavy_check_mark: 

Bonus: You can also use this handy automated test page for the Stark CMR: https://redispose.se/tests/ For all other hardwares, the above mentioned is a good test!

## CAN grounding practices

CAN networks are vulnerable to lightning strikes. [See the dedicated wiki page for this for more info](https://github.com/dalathegreat/Battery-Emulator/wiki/Lightning-strike) :cloud_with_lightning: 

> [!CAUTION]  
> Grounding everything is especially important for certain inverters. If you fail to ground inverter or battery casing to protective earth (PE), there might be a voltage difference between the two components, which can fry the CAN communication chips on the Battery-Emulator. Always connect every component, and the communication shield wire to protective earth before turning the system on!

See this image for grounding: 

![image](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/9e2324ae-c134-4244-88c5-ff3d60aafb6e)

Here is the best way to ensure that there are no paths for spikes in CAN voltage to fry chips on the boards (Important for [Solax](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Solax) and [Foxess](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-FoxESS-H1-H3-AC1-KH), other inverters are more lenient on what power supply you use)

![image](https://github.com/user-attachments/assets/5aa86774-d227-44f9-b223-3604f2ddb006)

> [!CAUTION]  
> Never connect the signal wire shields in both sides. This creates a ground loop. One side of the shield should be freefloating, like shown in the above pictures.