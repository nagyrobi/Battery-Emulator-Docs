> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

# BMW i3 60 / 94 / 120AH battery pack info

## Supported BMW batteries

All i3 batteries have the following length/width/height 1660mm x 964mm x 174mm

- BMW i3 60 Ah, 18.2 kWh, 233kg, 2013+
- BMW i3 94 Ah, 27.2 kWh, 256kg, 2017+
- BMW i3 120 Ah, 37.9 kWh, 273kg 2019+
- Mini Cooper Electric (F56) 94Ah, 32.6kWh 2019+

## Word of caution when buying i3 batteries ⚠️

> [!IMPORTANT] 
> The BMS (SME) inside the i3 battery will store crash data. If the battery comes from a really hard crash that triggered enough airbags, there is a possibility that the SME has entered a locked state, and will never engage the contactors. The only way to unlock the battery is with an expensive BMW tool called "EoS Tester" that costs 10k€. 
Update: According to a user you can swap the SME board with one from eBay or other places online and it will work with battery emulator, though not with a BMW i3 since it will need to be EoS tested.


<a name="CAUTIONCONTACTORSWELDED"></a>
> [!CAUTION] 
> When shutting down a working i3 battery system, no load can be present on the HV system. First shut down inverter before shutting off the battery, OR use the PAUSE button in the Webserver to ensure that 0A of current before shutting down the battery. The i3 has extremely sensitive welding detection. If there is over a few A of current during opening of contactors, it will set the "Contactors Welded" state and lock the battery permanently

The EoS tester can be rented from some places, a great i3 expert is available in CZ, https://www.i3upgrade.cz/
Another alternative when dealing with a locked battery, is to open up the battery and bypass the contactors. Nobody has reported if this works yet (feel free to edit this wiki!), worst case you could also replace the i3 BMS with an [RJXZS](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-RJXZS-BMS)

An indicator if the battery is not in lock state is the range indicator of the crashed car. If it displays battery range/percentage, the battery is *probably* not in lock state even if a few airbags have gone off.

Crashed BMW i3 battery being reset with an EoS tester:

[<img src="https://github.com/user-attachments/assets/781376d0-bf34-4a67-aebb-283b9179f42e" width="300">](https://github.com/user-attachments/assets/781376d0-bf34-4a67-aebb-283b9179f42e)

## Software configuration
For this battery type, use the option called "BMW i3" under the "Battery Protocol" setting

<img width="487" height="90" alt="image" src="https://github.com/user-attachments/assets/682d4deb-03f6-4123-832a-cc110b61363c" />

## Connection diagram

### High voltage connector
Right beside the HV connector there is a plug with 2 small pins, these need to be bridged either with the original plug, or shorted with a jumper for the battery to be able to turn on (Interlock detection)

[<img src="https://github.com/dalathegreat/BYD-Battery-Emulator-For-Gen24/assets/26695010/c2928a1c-2760-4727-9bf5-b3f24eda46cb" width="300">](https://github.com/dalathegreat/BYD-Battery-Emulator-For-Gen24/assets/26695010/c2928a1c-2760-4727-9bf5-b3f24eda46cb)

<img width="550" height="726" alt="image" src="https://github.com/user-attachments/assets/d526d1e6-23a8-4f32-8e03-7e9954b41762" />


### High voltage connector (E196*1B)
* Pin 1 = HV+
* Pin 2 = HV-
The HV connector has + and - marked on it.

#### HVIL part of high voltage connector (E196*01B)
* Pin 1 HVIL (loop to 2)
* Pin 2 HVIL (loop to 1)
Can also be bridged with jumper wires.

#### <a name="HVCableMod"></a> High voltage cable modification

The bmw I3 uses a 35mm² high voltage cable. To connect it to a terminal block and go down in size to a more manageable 10mm² you need
ferrules for these stranded wires to not damage them.
This can be done by cutting off the old connector and using a ferrule and crimping them. These tools are not so common for consumers.
An alternative for this is modifying the connector and use the current connector as ferrule so you don't have to buy or rent tools to achieve a non-stranded wire for the thermal block with size 35mm²

Click on Details  ⬇
<details>

[<img src="https://github.com/user-attachments/assets/6bb38ac4-f915-4f51-bd37-9041cbbdd993" width="200">](https://github.com/user-attachments/assets/6bb38ac4-f915-4f51-bd37-9041cbbdd993)
[<img src="https://github.com/user-attachments/assets/e96e9231-138e-442d-a2c7-7c5d831a6891" width="200">](https://github.com/user-attachments/assets/e96e9231-138e-442d-a2c7-7c5d831a6891)
[<img src="https://github.com/user-attachments/assets/36675922-fd47-4a0e-914e-438ea75e747b" width="200">](https://github.com/user-attachments/assets/36675922-fd47-4a0e-914e-438ea75e747b)
[<img src="https://github.com/user-attachments/assets/75e13c01-18ba-4430-b2c2-8c7e8cd3b159" width="200">](https://github.com/user-attachments/assets/75e13c01-18ba-4430-b2c2-8c7e8cd3b159)
[<img src="https://github.com/user-attachments/assets/b1a65071-efc1-4809-8109-f7be20be528d" width="200">](https://github.com/user-attachments/assets/b1a65071-efc1-4809-8109-f7be20be528d)
[<img src="https://github.com/user-attachments/assets/bd6218e7-b816-48a3-8ba8-f2322fc27cee" width="200">](https://github.com/user-attachments/assets/bd6218e7-b816-48a3-8ba8-f2322fc27cee)
[<img src="https://github.com/user-attachments/assets/12e7941b-ccbc-4dc0-8ee6-30355c0500b5" width="200">](https://github.com/user-attachments/assets/12e7941b-ccbc-4dc0-8ee6-30355c0500b5)
[<img src="https://github.com/user-attachments/assets/f704a520-e5e2-460f-90a0-b67b572802db" width="200">](https://github.com/user-attachments/assets/f704a520-e5e2-460f-90a0-b67b572802db)
[<img src="https://github.com/user-attachments/assets/f553e41a-94f6-45b9-a009-3dd03d2d7eb9" width="200" height="267">](https://github.com/user-attachments/assets/f553e41a-94f6-45b9-a009-3dd03d2d7eb9)
</details>

### Low voltage connector (A191*1B)
The LV connector is located on the back of the battery pack, next to the A/C cooling port. A/C connector is not required for operation.

[<img src="https://github.com/dalathegreat/BYD-Battery-Emulator-For-Gen24/assets/26695010/90265223-54e6-487e-94c3-c848789b5771" width="300">](https://github.com/dalathegreat/BYD-Battery-Emulator-For-Gen24/assets/26695010/90265223-54e6-487e-94c3-c848789b5771)

It has the following pinout:

[<img src="https://github.com/dalathegreat/BYD-Battery-Emulator-For-Gen24/assets/26695010/41648295-7018-4fe5-9174-8906b3aee616" width="500">](https://github.com/dalathegreat/BYD-Battery-Emulator-For-Gen24/assets/26695010/41648295-7018-4fe5-9174-8906b3aee616)

Connect the wiring as follow:
* Pin 1 30C - Connect to to 12V, 10A fuse optional
* Pin 9 15WUP-Signal (Green/GreenRed) - Connect to 12V, 5A fuse optional. Control this pin with ASR-10DD relay or a Pololu Power Switch controlled by PIN 25 in the LilyGo.
* Pin 7 (Red) - Connect to to 12V, 5A fuse optional
* Pin 2 Ground (BrownBlack) - Connect to Ground
* Pin 4 CAN-H (WhiteYellow) - Connect to LilyGo CAN-H - twist with CAN-L cable and put a 120Ω resistor across to CAN-L.
* PIN 10 CAN-L (WhiteBlue) - Connect to LilyGo CAN-L
* Pin 6 I_LOCK (BlueRed) (connect to Pin 12 with a 33Ω resistor in between)
* Pin 12 I_LOCK (BlueRed) (connect to Pin 6 with a 33Ω resistor in between)
* Pin 3 Refrigerant valve (NOT USED)
* Pin 8 Refrigerant valve (NOT USED)

#### 15WUP-Signal

The GPIO that controls the WUP signal depends on your BE hardware:
- LilyGo T-CAN485: GPIO 25 (For double bat GPIO 32 is used for secondary BMW i3 battery)
- Stark CMR: GPIO 25 (For double bat GPIO 32 is used for secondary BMW i3 battery)
- LilyGo T-2CAN: GPIO 40 (For double bat GPIO 38 is used for secondary BMW i3 battery) 
-Since a recent update WUP has been moved to GPIO 1! This requires you to buy qwic wires or jst sh 1.0. These can be found on Amazon or AliExpress with a few cm or wire crimped to them.


The wakeup signal needs to be actuated by the Battery-Emulator, and as soon as messages start to come through from the battery we reply. This ensures a reliable startup. Same goes for rebooting/shutting down the battery. The Battery-Emulator sets WUP to low incase we need to command the BMS off.

Since the LilyGo board has 3.3V logic on the GPIO pins, we need to use a solid state relay in order to boost the 3.3V -> 12V. Example connection using 1x ASR-10DD solid state relay:

> [!CAUTION] 
> To avoid [welded contacts](#CAUTIONCONTACTORSWELDED) Ensure you have a 12V backup system to avoid unwanted contact closings under load in case of a blackout

[<img src="https://github.com/user-attachments/assets/454434ec-770f-4733-af2b-d6012c8ecfb5" width="700">](https://github.com/user-attachments/assets/454434ec-770f-4733-af2b-d6012c8ecfb5)

#### Example wiring diagram
Below an example wiring diagram

[<img src="https://github.com/user-attachments/assets/ff994eb1-e15a-4bbb-9c80-261a2de44d4a" width="700">](https://github.com/user-attachments/assets/ff994eb1-e15a-4bbb-9c80-261a2de44d4a)

##### Stark Box + i3 battery + Fronius Gen24
<img alt="image" src="https://github.com/user-attachments/assets/b1f533fc-8bfc-4cdd-85c8-a035c94ec5d3" />

##### Stark Box + 2x i3 battery + Fronius Gen24
<img alt="image" src="https://github.com/user-attachments/assets/0a2d3dee-e920-4d75-95e9-1329e4691c00" />

##### SMA Sunny Tripower to Liligo and BMW i3
[<img alt="SMA i3" src="https://github.com/user-attachments/assets/96d7a08e-04c1-4799-8d1b-9140ef9c7c6a" width="700">](https://github.com/user-attachments/assets/96d7a08e-04c1-4799-8d1b-9140ef9c7c6a)

## Parts list
* BMW i3 battery
   * 60Ah
       * 61252353679 / 140116 / 7625051 / ​2353644 / ​728838
   * 94Ah
       * 61252412020 / 72883817 / 8647909 / 14191812
   * 120Ah
       * 2412116 / 2412117 / 7933745 / 7933746 / 7933747
* BMW HV cable
   * 61129346573
   * 61126809274
   * 761978102
* BMW HVIL bridge
   * 12527630408
* BMW Connector
   * 61139165781 (also known as 9165781).
   * Note: equivalent part from Kostal is # 9411204.
   * [Pigtail, easy: Link to Aliexpress](https://a.aliexpress.com/_EIWvMyk)
   * [Pin your own: Link to Alibaba, You can order a sample instead of multiple connectors](https://www.alibaba.com/product-detail/12-Pin-9165781-01-76616-9_1601400196256.html)
       * This includes MQS bushing contacts, no wires attached
   * Search suggestion "12 Pin 9165781-01/76616-9"
* 8x BMW Bushing contact MQS with cable
   * 61130030859 / 61130005197 (they are all the same color though, recommend to mark them)
   * Or get a free wiring harness from your scrap dealer and reuse the cables
* HV Capacitor 470µF or higher and more than 500V.
   * Consider a capacitor with screw connection to avoid the need of soldering iron
* 33Ω resistor, ¼ watt or similar is fine.
* 12V power supply (When Grid-backup is not available)
   * with UPS to avoid [welded contacts](#CAUTIONCONTACTORSWELDED))
       * E.G: Mean Well DRC-40A with a 12v 1.2ah battery

### Note on capacitor
Capacitors are high voltage, so they need to be inside an IP enclosure to prevent anyone from touching or water getting onto it and shorting it out. Most either mount the capacitor next to the battery, or next to the inverter, at either end of the HV bus. The most popular solution is to install fuses and the capacitor right at the start where HV comes out of the battery, sort of an add-on box that gets mounted on the original HV cable coming out of the battery. This is also a good place to step down the batteries thick DC cabling (35mm² in case of the i3), down to a more manageable 10mm². [To avoid the need for 35mm² crimping tools you could consider this solution](#HVCableMod)

Example of capacitor integrated at point where wire gauge is reduced, inside exclosure:

[<img src="https://github.com/user-attachments/assets/c1b0c422-e0db-43bb-825e-ee71174ca28f" width="300">](https://github.com/user-attachments/assets/c1b0c422-e0db-43bb-825e-ee71174ca28f)

### Note on Balancing :b: 
The BMW i3 battery needs periodic cell-balancing to be able to operate at full capacity. To start this balancing procedure, charge the battery to 100%, and go to the "More Battery Info" page on the Webserver. There there is a button called "Start balancing". When balancing is started via this page, the battery will power off the wakeup(WUP) pin towards the battery, stop CAN communication, and the battery can then start to balance, just as it would in a car.

Perform this balancing as often as necessary to keep cell mV delta low. Failure to balance will longterm lead to much capacity being unavailable due to voltage diff, along with wildly incorrect SOC% readings.

## Important info when used with BYD CAN inverter
> [!NOTE] 
> If you intend on using BYD-CAN with the BMW i3, the battery needs to be on a separate CAN bus. The BMW i3 is using the same CAN IDs as BYD do, so if you try to run them both on the same bus the IDs will collide and values get interpreted wrong

## Troubleshooting tips
| Problem | Suggested fix |
| :-----: | :---: |
| Contactors not closing | Check that the capacitor is seated between HV+ and HV-. Check that negative and positive are not accidentally shorted together |
| Event "Error: Battery interlock loop broken. Check that high voltage / low voltage connectors are seated"  | Check that both interlocks are OK. 1. The High Voltage needs to have the two small HVIL wires joined together near the orange connector. 2. The Low Voltage connector also needs to have pin 6 and 12 connected via a 33 Ohm resistor. If you are doing the pins yourself, make sure they are seated all the way. |

## Example completed setup
Fronius Gen24 with 2x BMW i3 batteries in [double battery mode](https://github.com/dalathegreat/Battery-Emulator/wiki/Double-Battery)

[<img src="https://github.com/user-attachments/assets/605dd84c-bf70-4ab2-a039-ff2856c0cfbd" width="300">](https://github.com/user-attachments/assets/605dd84c-bf70-4ab2-a039-ff2856c0cfbd)

i3 94Ah with Sofar inverter

[<img src="https://github.com/user-attachments/assets/e53c5d88-26cf-4075-a647-d3b5e78fda7a" width="300">](https://github.com/user-attachments/assets/e53c5d88-26cf-4075-a647-d3b5e78fda7a)
