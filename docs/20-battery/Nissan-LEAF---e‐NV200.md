> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

# Nissan LEAF / e-NV200 battery

## Physical size
The Leaf battery packs (24/30/40kWh) are all the same physical size. The 62kWh battery however is 40mm taller.

- 24kWh (2011–2012, ZE0) = 277kg (601lb) 1547.0 (L) × 1188.0 (W) × 264.0 (H) mm
- 30kWh (2013–2017, AZE0) = 294kg (648lb) 1547.0 (L) × 1188.0 (W) × 264.0 (H) mm
- 40kWh (2018–2025, ZE1) = 303kg (668lb) 1547.0 (L) × 1188.0 (W) × 264.0 (H) mm
- 62kWh (2018–2025, ZE1) = 410kg (903lb) 1547.0 (L) × 1188.0 (W) × 304.0 (H) mm

![Leaf 24/30/40kWh pack](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/4af680d7-5736-4249-9fd1-ff47b38b3532)

The e-NV200 battery pack is 1578 (L) x 1102 (W) x 266 (H) mm and is packaged differently from the Leaf packs (active cooling and service disconnect at front instead of middle).

![e-NV200 24/40kWh battery pack](https://www-asia.nissan-cdn.net/content/dam/Nissan/hong-kong/vehicles/e-nv200/product_code/product_version/performance/nv200-performance-battery.jpg.ximg.l_12_m.smart.jpg)

## Software configuration
For this battery type, use the option called "Nissan LEAF battery" under the "Battery Protocol" setting

<img width="598" height="146" alt="image" src="https://github.com/user-attachments/assets/d02bde1e-80c5-464e-9b81-31459391bd9c" />

* If you are using the 2011-2012 24kWh battery, you can enable "Interlock required" in the software for extra safety. Then the software checks that high voltage connectors are plugged in before you can start.
   * If you use "Interlock required" on a 2013-2023 battery, both HV plugs need to be seated (80kW and 6kW heater).
   * For 2013-2023 it is recommended to **not** use "Interlock required" due to the inconvenience of having to source both HV connectors, and instead just block off the unused HV port with a cover.

## Wiring diagram
The following pictures show an example of hooking up a LEAF battery to a Fronius Gen24 inverter. The same diagram can be useful for planning other inverter combinations.

> [!NOTE]  
> The LEAF battery requires a 12V supply capable of delivering 1.5A

Remember to seat the service disconnect switch. Without this fitted, the battery will not output any voltage when contactors are closed.
This is how the SDS is used [(Youtube)](https://www.youtube.com/watch?v=tbo2Qzdj-Rg).

**Here's how to connect the high voltage lines**

The Positive (+) wire is close to the data port and the Negative (-) wire is close to the PTC High Voltage port (Aux Max 6kW).

![HighVoltageWiring](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/8ddeceeb-efa5-4f05-94d5-2c4f0360511b)

![423205153-0e4498c8-f8b8-41d3-bd6f-fa9e5d0640d2](https://github.com/user-attachments/assets/53fc2333-4e02-4cda-bf2a-4128baa3be46)
![Zoe_harness](https://github.com/user-attachments/assets/e6d800e2-0fc6-4d7a-ab19-7a00199740ca)
![Cut_here](https://github.com/user-attachments/assets/e0c1bea2-873d-4cc3-8c4f-d38d8f4af8e3)

An even more detailed connection diagram, with automatic contactor control via solid state relays for maximum safety. This diagram features the optional BMS reset relay (if you are not using this, just supply BAT+IGN with constant 12V), and an optional equipment stop circuit.

Nissan's own documentation uses pin numbering on the 36 pin low voltage connector which does not match the moulded connector. The connections on this diagram have been renumbered so that they do.
![lilygo draw](https://github.com/user-attachments/assets/a6b0aa26-10ad-4327-8bbe-f833a005d74c)

## Precharge/Contactor closing
Almost all EV batteries contain contactors and precharge relays. Contactors act like big relays, and are used to control electrical circuits where currents are high. They are designed to be able to break the flow of current in a safe manner without electrical arcing. There are two contactors, one for positive and one for negative. To avoid electrical arcing when turning on the battery, the initial inrush of current is led thru a precharge resistor, to allow for slow charging of the capacitors inside the inverter. If the inverter has been turned off for a long time, the capacitors inside will act almost as a dead-short, 0 ohm resistance. If you skip using the precharge, then your contactors will spark every time you close them, wearing them out prematurely. Now that we know what the contactors/precharge does, we can look at ways to control it.

The Nissan LEAF battery can be used in two ways. Manual and automatic startup/shutdown of the contactors/precharge circuit.
### Manual control 🖐️
Using switches and manual timing, it is possible to turn on the precharge(A), negative contactor(B), and positive contactor(C). This is the simplest method, but it increases the risk of failures. Accidentally flipping the wrong switch at the wrong time may damage the battery and/or inverter. Also if you leave your battery un-attended, the battery has no way of disconnecting from the inverter incase it senses a fault. **Due to all this, the manual control method is NOT recommended.**

### Automatic control 🤖
The LilyGo hardware can act on its own, and turn on/off the contactors/precharge resistor when the battery says it is OK and turn off when not OK to proceed. This is done via the 3.3V digital output header that is located on the board. To use these, you need to solder a 2x6 row connector onto the board. After the row connector is fitted, you can connect a flat ribbon cable between the pins, and the relays.

To enable the feature in the software, Enable the "Contactor control via GPIO" option on the Settings page

<img width="505" height="42" alt="image" src="https://github.com/user-attachments/assets/86b3a437-d56b-4107-a369-fb6a04d0e7da" />

To keep things simple, it is recommended to use Solid State Relays (SSR). These can be activated with 3Volt, and control large DC currents. Follow this schematic to complete the circuit:
- (LilyGo) Precharge pin 25 - Precharge SSR + input
- (LilyGo) Positive Contactor pin 32 - Positive SSR + input
- (LilyGo) Negative Contactor pin 33 - Negative SSR + input
- (LilyGo) GND - All 3x SSR - input

OPTIONAL: If you use SSR relays with the LilyGo, you can also enable PWM mode for reduced power consumption. Here are parameters confirmed working with the LEAF contactors+PWM

<img width="624" height="118" alt="image" src="https://github.com/user-attachments/assets/6616719e-95b1-4f0d-889e-2a6b8b676574" />

![bild](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/06e007b4-bc97-4f21-a37f-34016b46ac4a)

Before the contactors turn on, both Inverter and Battery needs to give OK ✅ signal. This can be verified via the Webinterface:

![bild](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/6763e8c1-f03f-4570-b89e-25cdcaf0a736)

## Periodic restart of BMS
The Nissan LEAF BMS is not able to operate 24/7 under all conditions. Over time the SOC% will become less and less accurate, and in some conditions even the GIDS (Wh remaining) becomes confused (see [Issue 86](https://github.com/dalathegreat/Battery-Emulator/issues/86)).

See the [Periodic Reset page](https://github.com/dalathegreat/Battery-Emulator/wiki/Periodic-BMS-reset) for details.

> [!NOTE]  
> The LEAF battery is fully charged at 92-96% SOC. Use the Scaled SOC function to get a nicer looking 100% curve!

## Part numbers for Nissan LEAF batteries
In case your battery is missing some wires/disconnect switches, here are the OEM part numbers and purchase links. Do note that it might be cheaper to source from your local scrapyard!
|  Product |  Purchase Link |
| :--------: | :---------: |
| Service disconnect switch (2011-2012) 2971C13NA0B |  [Ebay](https://www.ebay.com/sch/i.html?_nkw=fuse+2971C13NA0B)   |
| Service disconnect switch (2013-2023) 297C1-3NF0A |  [Ebay](https://www.ebay.com/sch/i.html?_nkw=nissan+297C1-3NF0A&_sacat=0)   |
| 22pin battery connector (2011-2012) Yazaki 7283-8750-30 |  [AliExpress](https://www.aliexpress.com/item/1005003344398420.html)   |
| 36pin battery connector (2013-2023) Yazaki 7287-1065-30 |  [AliExpress](https://www.aliexpress.com/item/1005004180391674.html)   |
| Precrimped 22/36 connectors | [AliExpress](https://www.aliexpress.com/item/1005005815234149.html)   |
| High voltage connector 80kW 297A6-5SH1A |  [Ebay](https://www.ebay.com/sch/i.html?_from=R40&_nkw=297A65SH1A&_sacat=0)   |
| High voltage connector 80kW 297A22581R ZOE, also works |  [Ebay](https://www.ebay.com/sch/i.html?_from=R40&_nkw=297A22581R&_sacat=0)   |
| High voltage 80kW cable 297A21061R from ZOE41 ( Both end have the good connector ) |  [Ebay](https://www.ebay.com/sch/i.html?_from=R40&_nkw=297A21061R&_sacat=0)   |
| High voltage connector 6kW PTC 297A6 3NA0A |  ~~[Ebay](https://www.ebay.com/sch/i.html?_from=R40&_nkw=297A6-3NA0A&_sacat=0)~~ Is not the right cable. |
| 4ch SSR on DIN rail. Make sure you get DC-CN. | [AliExpress](https://aliexpress.com/item/1005007825084745.html) |

A [Google spreadsheets](https://docs.google.com/spreadsheets/d/14ghFL5mUg0hlUOsraOJc9BExlsMp5ClRRITRSQVLfkA/edit?gid=0#gid=0) with parts and links for a Nissan 40kwh battery and 8kW inverter (30A fuses)

297A6 3NA0A does not fit, you have to cut a part of it to use the pins!

![image](https://github.com/user-attachments/assets/af589952-4466-4759-9177-cefb77387869)

**Incompatible cables**

When searching on eBay you may come across other cables in the Zoe which use a connector that is similar in appearance but is much smaller:

* Zoe HV Wiring Harness - PN: 240419193R
* Zoe HV Engine Bay Wiring Harness - PN: 240413370R

These will *not* connect to the Leaf battery terminals.

## Communication cable AWG limits
22 or 36pin connector are designed with an hole of 2.1mm that would contain cable and included elastic ring for water proof seal.
AWG 22 (0.5mm2) is enough cable section for the supported Amps.

Normal cable of the above section (22AWG) have a protective rubber that is too thick (usually 2.1mm external diameter) => need to choose automotive cable that follow reduced rubber cable standard (FLRY-A or B ISO 6722) that shouldn't be thicker than 1.5/1.7mm (better 1.5) outside diameter.

Need at least 0.7/1mm thick cable (e.g. cable + wrap) to allow correct water proof e.g. no ethernet cable can be used because too thin.
Eth cable can go from 23 to 26 AWG. Normal AWG is usually 24 AWG that is rated for max 0.5A.
HV battery contactors drain continuously 0.4A each one at 12v.

BTW it's not strictly necessary to be automotive grade cable if it has enough copper diameter (0.5mm2) and outer diameter of ~1.5mm.

NOTE: If you pin your connector yourself - ensure the pins go all the way to the bottom and the pin is seated properly. If they are inserted incorrectly (not far enough, "wonky") then you won't have proper communication.

Here is a nice example of a completed 22-pin 2011-2012 cable:
![image](https://github.com/user-attachments/assets/6ae35e01-8bac-4076-9a62-c18456d088f8)
![Yazaki_22pin](https://github.com/user-attachments/assets/77f78d23-da23-421b-9d1c-8dab7aabe80b)

Crimping B24 (36pin) connector in progress.

![image](https://github.com/user-attachments/assets/80004a04-3e6d-4088-86ed-be791178658d)

### NOTE about cable code
* FLRY-A Automotive low voltage cable (FL) with reduced thickness of insulation (R)
made of PVC (Y), with regularly stranded conductor (A)
* FLRY-B Automotive low voltage cable (FL) with reduced thickness of insulation (R)
made of PVC (Y), with irregularly stranded conductor (B)

## Alternative HV connectors 
Original part number for Nissan Leaf battery is 297A65SH1A but a cheaper alternative for battery HV connector can be found in scrapyards. Renault Zoe or Kangoo Batteries use the same connector as Leaf battery. 297A22581R is the part numbers for both car.

The connector used is an [Aptiv HV RCS 800](https://www.ttieurope.com/content/dam/tti-europe/manufacturers/aptiv/doc/aptiv-hv-rcs-800-automotive-connectors-datasheet-specifications.pdf).

If you are mounting the battery indoors, you can also 3d-print a high voltage plug. This is generally not recommended, due to no IP rating, and no voltage rating. So try to source a real HV connector if possible! That said, this is a link to Pelle_C's excellent 3d-printable connector: https://gitlab.com/pelle8/3d
![rcs800_32A](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/ad4aa2ec-505e-4b99-9a8c-1b554799e899)
![rcs800_leaf](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/c785254e-8a63-418d-879c-1f970facad1a)

The 2013-2023 batteries have an external high voltage heater port. The socket can be covered with [3D printed backoff](https://www.printables.com/model/1756569-nissan-leaf-battery-ptc-connector-cover). It can also be plugged with silicone, but beware that certain types of silicone are conductive while uncured. Allow the silicone to cure for 24 hours before engaging contactors in such a case.

![bild](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/a5390194-4a78-43bf-b764-cb2c6341721a)

## Alternative Service Disconnect Switch

Here is an [3D printed SDS for 2013-2023 batteries](https://www.printables.com/model/1337831-nissan-leaf-ze1-service-disconnect-plug)

![3d_ZE1_SDS](https://github.com/user-attachments/assets/86266cc2-61f1-42b6-b13b-f9d8cf6ccf30)

## Examples of wiring installs
Here are some examples on how to wire up the high voltage output from the battery, into a fusebox or DC junction box.

![image](https://github.com/user-attachments/assets/451a9c00-62e8-45ad-b5bc-7c5281ada27c)
![image](https://github.com/user-attachments/assets/f03a88c6-2a57-446f-9f32-492c84ebf088)
![cabluri2](https://github.com/user-attachments/assets/7d286bc5-c761-4b4d-8dbc-4f203d09ce64)

Phoenix 3049408 DIN rail connectors
![Phoenix 3049408 DIN rail connectors](https://github.com/user-attachments/assets/98e73c65-84ae-4523-bb3e-45b3da39d500)


## Notes on 30kWh battery
The 2016-2017 30kWh LEAF battery had a software bug in the BMS that caused the amount of kWh reported by the battery to be incorrect, and the state of health % to drop too fast. If you have one of these batteries, and it shows below 50% SOH, your battery might be affected. The Battery-Emulator can perform a degradation reset, and bring the SOH% back to 100%. This can be accessed from the Webserver, via the "More battery info" page. By pressing the "Reset degradation data", the clear is performed. 

Performing this clear can restore a few kWh of usable energy back. 

> [!IMPORTANT]
> The degradation reset only works on 2011-2017 batteries. Performing it on 2018-2025 40/62kWh packs will have a negative effect, since it will restore the battery data too low. So only perform this reset on 24/30kWh packs!

![image](https://github.com/user-attachments/assets/2fbe8098-7aae-4242-aa07-dd39160f8f02)

### Performing the reset in detail
To perform a proper SOH% reset, [that sticks between reboots](https://github.com/dalathegreat/Battery-Emulator/issues/900#issuecomment-3482162856), perform the following steps:

- Open contactors
- Reset battery degradation via the More Battery Info page
- Disconnect CAN cables (also disconnect them from the inverter if they share a single CAN bus)
- Keep BMS power ON, but remove CHG/IGN 12V signal
- Wait 3 minutes
- Reconnect CAN cables
- Restore 12V to GHG/IGN
- Close contactors
- Reboot Battery Emulator

After these steps, the SOH reset to 100% becomes persistent. 

### Set your own, real limits
Note that after you reset the SOH to 100%, the BMS will let charging and discharging the cells likely beyond the limits which are safe to use on long term, in respect to the longevity of the cells. In stationary usage the battery charges and discharges much slower, and in a different pattern than when it used to do in a car, so a SOH recalibration in the BMS will take very long to happen, to match reality. 

It's recommended to [set up MQTT](https://github.com/dalathegreat/Battery-Emulator/wiki/MQTT#home-assistant-discovery) and a [home automation system](https://github.com/dalathegreat/Battery-Emulator/wiki/Home-Assistant#chart-examples) which lets you track cell voltages and delta on longer term, to be able to investigate the behaviour. 

To see some results, follow these steps after you do the reset (in normal ambient conditions, avoid extreme cold or hot periods):

- Disable "Rescale SOC" if you have it set
- Let the battery to charge to empty
- Let the battery to charge to full
- Let the battery to charge to empty again
- Watch how the values of **Cell Voltage Delta** and **SOC (real)** change over time as approaching full and empty

For example:
<img width="2200" height="1000" alt="image" src="https://github.com/user-attachments/assets/686908a4-8d0b-498e-9d50-c35f7d56e7d2" />

Try to find the widest time area of **Cell Voltage Delta** where the value changes least - that's the most comfortable and safe "zone" for the cells to operate. Look at the **SOH** graph in the same time period - that should give you the min and the max percentage of SOC rescaling yo can set in Battery Emulator, to prevent the battery to go in the high cell voltage delta zone. Take into account the absolute minimum SOC value your inverter is willing to go until (eg. Fronius allows discharging to 5% only, doesn't go below), you can reduce min SOC rescale about by that amount. 

In the example above, the top graph establishes the red lines, which show the comfortable area of **Cell Voltage Delta**. The green lines show where the SOC curve intersects the red lines: this gives the min and the max SOC rescale values for a safe zone. (note the slight hysteresis between charge and discharge, take the highest value for safety).

This way you'll still be in the safe zone with your battery, but you'll likely be able to use bigger capacity than the original SOH allowed in the BMS before reset. Re-evaluate this graph periodically, every 3 months (no need to do full charge-discharge so often, just keep your eye on **Cell Voltage Delta** and **SOC (real)** relation on normal usage)
