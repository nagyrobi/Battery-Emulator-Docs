# Dacia Spring / Renault K-ZE ( CMFA-EV platform )

The Dacia Spring Electric (27.4kWh) / Renault K-ZE (26.8kWh), are both vehicles in the CMFA-EV platform. 

There are also a few Chinese EVs that also share the same CMFA-EV battery platform, all the following models are compatible with the Battery-Emulator integration:

* Dacia Spring Electric (2021–present)
* Renault City K-ZE (2019–present)
* Dongfeng Aeolus EX1 (2019–2021)
* Dongfeng Fengxing T1 (2019–2021)
* Dongfeng Fengguang E1 (2019–2024)
* Dongfeng Nano Box (2022–2024)
* Venucia e30 (2019–2023)

### Note on model year
Some 2024+ batteries seem to not respond to CAN. Investigation ongoing!

2024+ can be identified with an additional "Pressure Sensor" near the HV/LV connector

<img alt="image" src="https://github.com/user-attachments/assets/65afac89-cdec-4e20-b285-b094224a9ff8" />

### General info

These batteries have 72 cells in series, which creates an operating voltage of approximately **216 to 302VDC**. Make sure [the inverter](https://github.com/dalathegreat/Battery-Emulator/wiki#supported-inverters-list) you are planning to use is compatible with this voltage range!

![image](https://github.com/user-attachments/assets/78f6bc04-5deb-4018-a675-4b6f7e8a1144)

### Testing the battery
In order to read the informations from the BMS, you need:
- [LiLyGo T-CAN485](https://s.click.aliexpress.com/e/_oDPdyMg) with Battery Emulator installed and configured for this battery and NO inverter.
- 12V source for LilyGo and battery (it can be a Gel battery, UPS, 12V adapter etc)
- Low Voltage battery Connector with those wires connected: 12V, GND, CAN-L and CAN-H (see the [connections](https://github.com/user-attachments/assets/d26e3b46-573e-42f3-80d2-f24f4cf91722))

### Shopping list
Here is a detailed [shopping list](https://docs.google.com/spreadsheets/d/14ghFL5mUg0hlUOsraOJc9BExlsMp5ClRRITRSQVLfkA/htmlview#gid=1550632359) for this battery implementation.

## Software configuration
For this battery type, use the option called "CMFA platform, 27kWh battery" under the "Battery Protocol" section, NMC chemistry.

![setari](https://github.com/user-attachments/assets/d0e5612f-e54a-46d5-bbbf-29782649ee41)

See [Fronius Gen24 settings](https://github.com/user-attachments/assets/c10cce43-ab5c-4fc6-b7e9-beec40e694f4)


## Battery module - BMS pin diagram. 
BMS reads from each module the cells voltage + one GND and 2 temp sensors (one for each module). should be ~17Kohm range

<img alt="image" src="https://github.com/user-attachments/assets/346c3f88-9e72-490e-a249-1a20138a5aee" />

<img alt="image" src="https://github.com/user-attachments/assets/7639b88e-3ea7-43c2-bec8-10ebed1ed18f" />

## Wiring diagram, Low Voltage

The battery contains a terminating resistor. 

![image](https://github.com/user-attachments/assets/1e075b98-4b58-4b4f-8180-0612f35f6e78)

The LV connector on the battery has the following pinout:

![image](https://github.com/user-attachments/assets/9d803be3-c63c-4ffd-b9b2-0353376abeb1)

The pin numbering is engraved on the LV connector (PT06A-12-10S)

![image](https://github.com/user-attachments/assets/e4a1b27a-2bc4-4510-8042-c181663d7c10)

Try to source at least the LV connector; scrapers are happy to cut the cables and usually give them for free.
The LV connector usually goes to scrap with the car.

Get the HV connector too; it makes things look nicer. 
Search for HV cable and connector on [eBay](https://www.ebay.com/sch/i.html?_nkw=297A21306R), part#: 297A21306R

Connect the battery to the Battery-Emulator according to this diagram:

![conexiuni](https://github.com/user-attachments/assets/82696d72-7d77-44a5-bb6f-f0c8437992d6)

<img alt="schema_conectare" src="https://github.com/user-attachments/assets/d26e3b46-573e-42f3-80d2-f24f4cf91722" />

12V power info: The preacharge+contactors consume 1.5A. The BMS itself uses 0.1A

Use a relay board (NO) (5V or 12V) to apply GND to the pins (pin 2AE / 2AD / 2AC all accept a **GND signal** to be toggled on).

See  [4ch SSR on DIN rail **(DC-CN)**](https://s.click.aliexpress.com/e/_olDgyMC)

![4ch SSR on DIN GND](https://github.com/user-attachments/assets/a1bbcc43-7816-468e-9ce3-37e7703976fd)

## Wiring, High voltage
The battery has a service disconnect switch:

![image](https://github.com/user-attachments/assets/e0398e96-aebc-4601-a69f-fb7b70e43dbc)

Note that there are two versions of the service disconnect switch. The 2021 model is different to 2022+. So, if you purchase a service disconnect switch, make sure you get the correct model year!

![image](https://github.com/user-attachments/assets/c82d67e5-d2c1-4b1d-a5e8-56345b29a886)


The polarity of the High Voltage outputs can be seen here, Left is **positive**, Right is **negative**.

![Polarity](https://github.com/user-attachments/assets/fc754f5f-2c00-4e67-a97e-3e2f75aa1935)

## HV Cable preparation
Beware of the the mantle strings (cut them) = not to touch the copper conductor.
The optional [ferrule](https://www.aliexpress.com/item/1005007192861678.html) for the copper conductor is 35smm
![HV cable](https://github.com/user-attachments/assets/2fa0341e-07c2-431b-9868-0d48fdc32b1a)

![HV_box](https://github.com/user-attachments/assets/a5dd40f0-78f4-48cb-bb5b-b3971604926e)

## Configuring the software
Enable the `CMFA platform, 27 kWh battery` option in the software

Do **NOT** use PWM contactor control.

Spring keeps the HV voltage between 220V (0% SOC) and 296V (100% SOC)
You can adjust SOC min percentage and SOC max percentage from the emulator settings to keep the battery voltage somewhere in the above voltage range. Like -5 / 85 with current build (june 2025)

This battery also benefits from automated 30s daily resets, like the [Nissan LEAF platform](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Nissan-LEAF---e%E2%80%90NV200#periodic-restart-of-bms). This can be automated with BMS power output.

<img width="448" height="39" alt="image" src="https://github.com/user-attachments/assets/96774b56-d740-4682-a54b-ab6eeb0ca6a0" />

## Fuse sizing:

For fuse sizing, we use the max inverter power / 218V = max fuse size.
ex, for a 5kW inverter: 5000W / 218V = 23A => use a [25A fuse](https://s.click.aliexpress.com/e/_onOPNy8)

* [Fronius Gen24 6-12kW](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Fronius) accepts max 22A on the battery input, so you will be able to draw from this battery 4.7-6.6kW, depending on voltage (SoC)
* [Fronius Gen24 3-5kW](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Fronius) accepts max 12.5A on the battery input.

## Completed builds:
1. DIY wooden support, with space for the fuse underneath
![wooden_support](https://github.com/user-attachments/assets/5ee48a99-a986-4720-9fb2-bd1ea2b45970)

![2025-06-07-6131-s](https://github.com/user-attachments/assets/19080cc5-e3af-4e59-8cfe-8b09383e3c89)

2. Metallic box
![Dacia Spring battery metalic box](https://github.com/user-attachments/assets/d6cf2445-4f72-4afb-be99-5bd67baaed64)
![PE on the case](https://github.com/user-attachments/assets/04fe6c93-5634-4061-816c-ce04f0385733)

## Note on missing cell voltages
If you are not seeing all 72 cells in the cell monitor page, the fuses on the cell taps might be damaged.
A telltale sign is that some values get corrupted, and the charge current goes to 0. This can be repaired by opening the battery and repairing the damaged traces.

![image](https://github.com/user-attachments/assets/ee3eded2-1dcf-4497-a2fd-6d199d5997ae)

## Inside the battery:

1. One 225A main DC fuse
![Dacia Spring battery 225A fuse](https://github.com/user-attachments/assets/3d305ecc-346d-47fa-8fe7-5a1a63149b5a)

2. One main BMS + 6 slaves BMS
The Master <=> Slaves BMSs seem **not** to be software locked, so you can easily swap the main BMS if it's the same part number (printed on the box).

![Dacia Spring battery BMS](https://github.com/user-attachments/assets/0a333285-c4d3-477b-aa1f-73b69d2a7d2c)

3. Inside look.
The battery case opens very easily. It has many M7 screws; no glue is used to seal it.
![inside Dacia Spring battery](https://github.com/user-attachments/assets/dc935807-3289-4ebe-b0b0-e4a916a95c5a)
