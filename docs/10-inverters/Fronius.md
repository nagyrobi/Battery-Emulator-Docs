> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

# Fronius inverter wiki page

## Types of compatible Fronius inverters
The code works with the following Fronius inverters
* Fronius Primo Gen24 Plus (all sizes) ✅
* Fronius Symo Gen24 Plus (all sizes) ✅
* Fronius Symo Gen24 Plus SC 12.0 ✅
* Fronius Symo Hybrid 3.0/4.0/5.0-3-S ✅ (ℹ️ Legacy product, no longer available for purchase)

On this date (23.04.2025) Fronius deployed a new software version that momentarily broke compatibility with Battery-Emulator 

- Installed: ROW 1.35.8-1 (Working ✅ ) Works with BE versions below 8.11.0
- New: ROW 1.36.5-1 (Breaks ❌ ) Works with BE versions above 8.12.0
- Newest firmware (1.40.8-1) works with the latest BE version. (info updated on 18.04.2026)

It is recommended to always use latest software version of both the Fronius inverter and the Battery-Emulator

## Hardware limitation :zap:
* Fronius GEN24 **Symo** inverters (3-5kW) are capped to max 12.5A on the battery port.
* Fronius GEN24 **Symo** inverters (6-10kW) are capped to max 22A on the battery port.
* All Fronius GEN24 **Primo** inverters are capped to max 22A on the battery port.

This means that if you have a high voltage battery (450V), you will see much higher charge/discharge speed (22A x 450V = 10kW), compared to a low voltage battery (22A x 250V = 5,5kW)

Keep this amperage limit of 22A in mind when designing a battery system for the Fronius!

## Software limitation :floppy_disk: 
ℹ️ There is a NON-Plus variant of Gen24 available, for this you have to purchase a battery license before you can add a battery to the configuration. Contact your installer incase "Battery Operation" feature is missing!

![Bildschirmfoto_vom_2024-04-07_10-01-43](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/b4513dd5-bacf-434a-bf83-fd66420bc0bd)

This can also be seen from the label on the inverter, incase "Plus" is missing:

![bild](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/df592149-0e24-46a9-af1c-760578fe7a6a)

## Setting up the Fronius inverter for DIY battery
You will need a technician login to the inverter to make changes to the setup. Contact your solar installer incase you don't have the technician login!

ℹ️ You will also need a Fronius Smartmeter so that the system can measure consumption and generation. The Fronius Smart Meter models "63A-3", "TS 65A-3" and "IP"(wireless) are all compatible.

To use a Fronius inverter with a used EV battery, it needs to be setup for battery operation. This is done via the "Solar.start" app, under components, add battery, and select "BYD Premium HVS/M".

![FroniusSetup](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/a706896f-c3ec-43aa-b5c3-be199bdcea9f)

#### Notes on Scaled SOC + Fronius min/max SOC
In the Fronius settings it is possible to define "SOC minimum" and "SOC maximum". If you are using the [Rescale SOC](https://github.com/dalathegreat/Battery-Emulator/wiki/Webserver-guide#rescale-soc) functionality in the Battery-Emulator, you will be applying double-rescaling of the usable capacity. It is recommended to only have one of the systems restrict the SOC window, which simplifies any troubleshooting.

Example of double-rescaling:

![image](https://github.com/user-attachments/assets/43983611-d022-42c3-b1b5-daa35792a763)

![scaled](https://github.com/user-attachments/assets/8bc93e51-742a-4342-ae87-820147ad44cd)

After adding the battery, connect the LilyGo Modbus terminals to the inverter according to this diagram (LEAF battery example)

![FroniusWiring](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/2f461d3b-d8ee-4bd9-84b4-c99cc4fdbe0b)

When the low voltage communication is handled, also connect the high voltage side (LEAF battery example)

![HighVoltageWiringFronius](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/16d30e40-314d-45cf-88c3-c7ca73bee1c4)

## Which protocol to use
For this inverter type, use the option called "BYD 11kWh HVM battery over Modbus" under the "Inverter Protocol" setting. Also select the Inverter interface as "Modbus"

<img width="490" height="65" alt="image" src="https://github.com/user-attachments/assets/bc1d1e6b-3fec-437e-b2c5-74f48c652b49" />

## Starting and stopping the system
When turning the system on, follow this startup procedure. Work quick, to avoid the inverter getting stuck in battery not detected mode.
### Startup
1. First start the Fronius inverter via AC switch
2. Turn on the Solar DC switch
3. Turn on the Battery DC switch
4. Start the LEAF battery BMS* with 12V
5. Start the LilyGo hardware with 5V
6. Then either handle precharge/contactor closing manually or let the LilyGo hardware handle it automatically

### Shutdown
1. Turn off the LEAF BMS*, cut the 12V supply to it. Wait 60seconds
2. LilyGo status LED will turn red. The Fronius inverter will within 30seconds stop using the battery.
3. After 30s has passed, turn off the contactors incase the LilyGo isn't setup to automatically handle them
5. Turn off the Fronius inverter via AC switch
4. Turn off the Battery DC switch
2. Turn off the Solar DC switch

NOTE: LEAF BMS is controlled via terminal #2 IGN in all Leaf batteries

## Day to day monitoring
The performance of the system can be tracked with the app "Solar.web". This app gives direct info on current output/input, SOC% level of battery, graphs, and more. No settings can be changed via this app, but it is a great tool for visualizations and quick status checks.
img

![SolarApp](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/3d310bf9-1da5-40b1-aa08-de30d7fc8af0)

## Troubleshooting

### Battery not detected
If you see "Battery not detected" in the Fronius apps;

![bild](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/ac6efc54-96fc-4345-948b-7cf0565bae5d)

Then verify the following
- Make sure High Voltage is present on inverter battery input. Also make sure the DC isolator switch on the front of the Fronius is turned ON

![image](https://github.com/user-attachments/assets/1b670acc-237d-4edc-b206-bbc0dd0294da)

- Next step is checking that Modbus connection is active. Check the Events page, if you see this MODBUS_INVERTER_MISSING event;

![image](https://github.com/user-attachments/assets/ca1404c1-fd57-4d1d-b435-1e9ad640b8bf)

It means that the Modbus connection is down. Verify that:
- Polarity of M0+ and MO- is correct
- Protective earth is attached in both Inverter and Battery side
- Check that wires are seated correctly in the Fronius connector. It is very easy to miss that the wires are not pushed in all the way.
- Try a different powersupply for the LilyGo. Powering it via USB from a computer can cause noise on the signal output. Powerbank or phone charger might have cleaner voltage output. If you see strange modbus errors, your powersupply might be noisy
- Make sure the Battery-Emulator has a good modbus connection to the Inverter
   - Use shielded wires for a stable connection
   - If you see "ModbusServerRTU.cpp  [ 252] serve: RTU receive: E5 - Packet length error" in the USB output on the LilyGo, it is an indication that wiring is not perfect and occasionally get corrupted

### Invalid battery size detected

<img width="445" height="233" alt="image" src="https://github.com/user-attachments/assets/0d342dcd-7829-4959-a0fd-3c7d8d6ed205" />

If you see this error, it might be because the battery you are using is having higher allowed maxvoltage than the Fronius inverter accepts. A good example is using BYD Atto3 battery (Max 460V), with a Fronius Primo single phase inverter, that only can take 450.

A quick solution is to enable the "450V maxvoltage cap" setting. This fakes it so that all batteries appear as 450V max. 

<img width="675" height="172" alt="image" src="https://github.com/user-attachments/assets/913139cf-527c-48c8-a1fc-e601f9d96d0b" />

NOTE: This setting should not be used with Fronius Symo 3-phase inverters. These inverters are OK with battery voltages up to 700VDC

## Advanced control of energy (Spot price, scheduled charge/discharge etc.)
Once you have your battery connected to the Fronius, it is possible to add additional hardware into the mix for advanced control of how energy should flow in the system. This is useful for those with spot-price electricity, or a nightly tarriff. Below are some examples you can utilize to control the Fronius Gen24 directly via Modbus TCP

- You can setup forced nightly charging via the webinterface of the Gen24 (Requires connecting to the inverter directly, not available via SolarWeb). This is very useful if you have a cheap night-tarriff, and want to charge the battery during the night and use the energy during the day
- [Arska-node](https://github.com/Netgalleria/arska-node/)
   - Reads Nordpool electricity prices (from EntsoE/Elering) and renewal energy forecast (solar from Open Meteo, solar/wind from Finnish FMI) as well as local real-time net power consumption/sales (smart meter HAN P1-port -tested in FI, Shelly 3EM). Based on this and time based data (+ optional ds18b20) the system updates variables (https://github.com/Netgalleria/arska-node/wiki/Channels#variables ) which are used in channel rules deciding whether the channel should be up/down/charging/discharging. Arska has also basic load management functionality (limits loads if consumption exceeds given limits). So far users have used the system mainly to optimise self-consumption of PV production and for scheduling flexible consumption at the cheapest time and selling any surplus when the price is high. Arska has controlled (through GPIO, Shelly and Tasmota relays) water boilers and (underfloor) heating. According to the developer, version 1.3 controls the Fronius GEN24 charging/discharging parameters (nWRte, OutWRte, StorCtl_Mod) based on given channel rules.
- Info on how to control Fronius Modbus via Homeassistant (German) https://www.libe.net/byd-modbus
- [SBAM](https://github.com/atbore-phx/sbam/tree/main) Charge Fronius battery using SolCast weather forecast.
- https://github.com/callifo/fronius_modbus This integration gives you full control over the charge and discharge of the battery.

## Off-Grid/Backup Configuration

Symo/Primo Gen24 inverters that are smaller than 6.0kW do **not** support running full backup mode (Primo 5.0 has support). On these smaller inverters, your best bet is to utilize the PVpoint outlet to get power out of the battery.

A Smart-Meter is mandatory for battery use when connected to a grid (or generator). However, the Gen24 can run 'Off-Grid' if configured specifically for this scenario and limitations. 

To run the Gen24 as off-grid but have the facility to connect to a grid (or generator) at some point, then you must follow the Full-Backup configuration design as documented by Fronius. 

If you intend to never connect the inverter to a grid source (or generator), then you can hard wire the Gen24 in Full-Backup mode. In both cases, the smart-meter **MUST NOT** be detected when running in Full-Backup mode. 

If needed for a future grid or generator power source, the smart-meter can be wired in as per normal ready for a grid or generator input, but the modbus comms (D+) must be disconnected to ensure it does not send data to the Gen24. If the gen24 detects a smart-meter when running in full-backup, then it assumes the grid is connected and will shutdown the inverter and not work reliably.  

To hard wire the Gen24 as off-grid and 'cold-start' using the EV battery pack or PV, I/O pins 6 and 7 need to be jumpered to the V+ on the I/O connector as shown below. 

<img width="403" height="478" alt="Screenshot 2026-05-12 at 12 19 41 pm" src="https://github.com/user-attachments/assets/86ee800b-46ea-4e33-9fb6-e0f9e7d24652" />

Additionally, Full-Backup mode needs to be configured on the Gen24 as follows:

<img width="610" height="671" alt="image" src="https://github.com/user-attachments/assets/cfef8792-eaa9-4aa9-92ae-eafeb6e2371b" />

Pin0 is not used when hard wiring for full backup. Pin0 is used for automatic switchover to full-backup int the event of a grid failure. Follow the Fronius documentation for this type of configuration. 

The Fronius Gen24 configuration documents describe both automatic and manual switch-over to backup mode using combination of switches and/or relays. Both options can be commercially sourced if you need a non hard-wired off-grid only solution. 

**IMPORTANT NOTE: **
When running as Full-Backup, the inverter will run at 53Hz. As such, no other inverter can be active on the AC circuit. Only the Gen24 can be providing power! If you need additional power, then a true Micro-Grid should be used (AC coupling with Victron etc)  

Also note, that Fronius officially supports 2000h off-grid (full-backup) per year on the Gen24. Exceeding this hour count voids the warranty. There is no reports of any inverters shutting down after 2000hours. 

To get around this off-grid limitation, you can create a microgrid using a 48V battery that powers a small inverter, and then connect the Gen24 to this microgrid. Then it will be essentially offgrid, without incrementing the 2000h max counter. Example of this running successfully has been done with a Victron a 7.2kwh(LV) , and FroniusGen24 with a 75Kwh battery(HV). A smart meter is required if running as a true micro-grid.

All normal earthing requirements when running in 'off-grid' must still be followed. Consult a qualified electrician for advice. 

## Notes on capacity reporting
Fronius supports 22kWh BYD batteries, and the protocol has a max value of 65535 Wh (unsigned 16bit value) for reporting Wh towards inverter. That means we have to restrict reported Wh number towards the inverter, for example when using a 75kWh Tesla battery, it will show up as max 60kWh in Fronius portal.

Note that this is just a REPORTED value, you can still use the full capacity of the battery. For instance, a 400kWh battery will show up as  60kWh, but the inverter will still use the full 400kWh

