# Options for high voltage precharge

### Why

The MEB battery, but also CCS charge ports require an external high (precharge) voltage to be applied before contactors are closed. This should not be confused witi the other Precharge terms used;

- Precharge: Precharge resistor inside battery
- **External HV source**: No precharge resistor inside battery, external hardware needed to provide high voltage to wake battery :arrow_backward:  This is the one this Wiki page handles 
- Capacitance: Capacitor needed to wake battery. Can be raw capacitor, or a DC/DC converter that has capacitance
- No precharge on inverter side: Means inverter internally has no contactors, exposing capacitance on BAT input

One of the options is the HIA4V1 board original or modified for Lilygo or Stark CMR. 

> [!CAUTION]
> There are various HIA4V1 board version avaiable that have different output polarization! Make sure to test separately (testmode described below) before connecting to the battery. There has been cases where the HIA4V1 has damaged the BMS due to overvoltage/wrong polarity. So going for other hardware is recommended

# Option A: TPS55288EVM-045 + XPPOWER (emco) G05 high voltage source
The Texas Instruments TPS55288EVM-045 Evaluation Module + XPPOWER (emco) G05 high voltage source is the latest option in external precharge. Support for this was added in firmware v10.11.0

## Wiring diagram
Parts needed:

- Texas Instruments TPS55288EVM-045 Evaluation Module
- XPPOWER (emco) G05 high voltage source
- Diode 1N4007
- Contactor Normally Closed
- Fuseholder and DC-Fuses 10x38 2A for DC use

<img alt="image" src="https://github.com/user-attachments/assets/939952d4-aa99-4db0-88f1-238af4c45f69" />  

Example installation:

<img alt="image" src="https://github.com/user-attachments/assets/829c5f4b-08c2-4e26-a7f6-eaa9e5d9522a" />


Jumper settings for the TPS55288EVM-045 board:
- JP1: ON (device enable)
- JP2: FB_INT (internal feedback selection)
- JP3: OPEN
- JP4: OPEN  


# Option B: HIA4V1
The HIA4V1 has been used successfully to precharge the MEB battery's inverter port. It can be found on aliexpress. Without modification it requires manually tuning the resistor (marked 104 in the picture below) to match the external voltage to the internal battery voltage. Given the thin traces on the board, it is probably a good idea to put a circuit breaker between the battery and this board.

![image](https://github.com/user-attachments/assets/b6f05eae-92d4-44cd-a668-91911b32e44e)

![image](https://github.com/user-attachments/assets/582fdc58-b5f0-4438-b2fb-e2dad54a54a1)

## Modified HIA4V1 directly controlled via digital output ESP32 (for Lilygo and Stark CMR)

The HIA4V1 contains a 555 based oscillator, with the output connected to a MOSFET. This MOSFET is 3v3 compatible, which means we can directly control it from an ESP32 based board like the lilygo.

To do this:
- first remove the 620 ohm smd resistor (marked 621) from the board. Easiest way to do this is put some solder on top of the resistor such that it covers both ends, melting the solder of both ends and thus releasing the resistor.  
- In place of the above resistor, on the pad closest to the MOSFET, solder a small wire.
- Connect this wire via a 330 ohm resistor to a pin on the lilygo (eg. io25) or Stark CMR (eg. io19 GPIO header).
- Solder a gnd wire to gnd pin of the connector on the underside of the board (if the low voltage gnd of the HIA4V1 is already shared with the lilygo, this step is not necessary). And connect this to the gnd of the lilygo or connect to GND of Stark CMR GPIO header

![image](https://github.com/user-attachments/assets/d93ed573-9fe0-48fe-bb53-1e0afe9de697) 
![image](https://github.com/user-attachments/assets/7299e5d0-cd37-4e06-870f-e70b5531ad86) 

By using the ledcWriteTone(PRECHARGE_PIN, freq); function you can now tune the voltage. 

Results while powering the board with 12V:
- 23kHz : 370V
- 28kHz : 390V

ledcWrite(PRECHARGE_PIN, 0); to turn the output off.

Note that these values depend on the current the HIA4V1 has to provide. 
> [!CAUTION]
It is absolutely necessary to bias HIA4V1 board with 4x 140k resistors in series across the HV output (4x to increase voltage handling capability), to prevent very high output voltage in no-load situations (which may damage connected equipment). I didn't do it and I destroyed one battery control unit like this :-(.

## Modified HIA4V1 for control via FET board (possible for Lilygo and Stark CMR)

HIA4V1 modifications:
* Remove 6 (marked) components on the primary side of the transformer
* Bridge the primary side of the coil to the power pin (A)
* Add the output bias resistors 4x 140k (or equivalent) on the HV output pins, protected with shrinksleve (B)
* Clearly mark the polarity of the HV output to avoid confusion

Frontside modifications - Components to be removed and output polarity marking:
![image](https://github.com/user-attachments/assets/a3219612-31ce-462b-873c-d2bed5a781ab)

Backside modifications - Bridge from coil to power pin (A) and HV bias resistors (B)
![image](https://github.com/user-attachments/assets/5b746ff4-c54d-413a-b2bc-4834146d8de7)

This method can also be used with the Lilygo HW if a seperate FET board is used, any MOS FET that works with 3V3 siganl will work.

ToDO add link, for now google for: 15A 400W MOS FET Trigger Switch Drive Module PWM Regulator

Wiring diagram:
<img width="1262" height="709" alt="image" src="https://github.com/user-attachments/assets/5c32be5c-9cee-48e7-8e07-537b7a7d9523" />

## Decoupling inverter from battery during precharge
During precharge the inverter will see a high voltage on its inputs pins. The inverters we have tested on will use this a trigger to startup. This will put a load on this high voltage while the contactors of the battery are not yet closed. This load will disrupt the precharging sequence and will cause the precharge to fail.
In order to prevent this we decouple the positive input from the inverter while precharging. This is done via a normaly closed (NC) high voltage contact. When the precharge sequence is active, this contact will be opened, decoupling the battery and precharge circuit from the inverter.
We use a normally closed contact, because this does not require any power during normal operation.

Type used: SEV100ADXL (1NC contact and controlled via 12V)
[Link to contactor](https://nl.aliexpress.com/item/1005004650010865.html?spm=a2g0o.detail.pcDetailTopMoreOtherSeller.1.2c18pc9apc9aDj&gps-id=pcDetailTopMoreOtherSeller&scm=1007.40050.354490.0&scm_id=1007.40050.354490.0&scm-url=1007.40050.354490.0&pvid=09f5a17f-9a78-4a39-b571-88f5f414ad8b&_t=gps-id:pcDetailTopMoreOtherSeller,scm-url:1007.40050.354490.0,pvid:09f5a17f-9a78-4a39-b571-88f5f414ad8b,tpp_buckets:668%232846%238114%231999&pdp_ext_f=%7B%22order%22%3A%2216%22%2C%22eval%22%3A%221%22%2C%22sceneId%22%3A%2230050%22%7D&pdp_npi=4%40dis%21EUR%2126.19%2126.19%21%21%2129.09%2129.09%21%402101ea7117488925298473608e3850%2112000041496624126%21rec%21NL%21164472360%21XZ&utparam-url=scene%3ApcDetailTopMoreOtherSeller%7Cquery_from%3A) or google for: Sayoon SEV100AD SEV100BD Hv Dc-relais 100A

The connection is added to the schematic above

## Overvoltage and reverse-polarity protection

<img width="1001" height="292" alt="image" src="https://github.com/user-attachments/assets/3c1fe3c9-5fb1-4d82-b5ff-3a4572d617ac" />

You can connect three 5KP150A 150V TVS diodes in series to protect against overvoltage (these will clamp the voltage at around 470-480V). Unipolar ones will also conduct like diodes in the forward direction, which will protect against reverse polarity.

If using TVS diodes, fuse protection is very important (as these diodes have a pulse rating of 400A, and usually fail short, so could draw a large momentary current from the battery).

## Software configuration (Lilygo or Stark CMR)
Below the newest SW settings, details depend on your setup, but this is basis.

![1000068581](https://github.com/user-attachments/assets/bcee2a3c-eb80-4f7e-86e2-0a53513d25e4)

For the previous/older SW version:
Make sure to enable the #define PRECHARGE_CONTROL option in the USER_SETTINGS.h file
https://github.com/dalathegreat/Battery-Emulator/blob/main/Software/USER_SETTINGS.h

Generic:
The precharge code itself is located in the folder Software/src/communication/precharge_control/precharge_control.cpp

https://github.com/dalathegreat/Battery-Emulator/blob/main/Software/src/communication/precharge_control/precharge_control.cpp

At the time of writing (release 8.13.0) both Lilygo and Stark CMR are tested and supported out of the box.

The mapping of the pins towards the physical hardware can be found in the corresponding file linked to the hardware you use, located in the directory Software/src/devboard/hal. Make sure to double check the connection is as expected.

https://github.com/dalathegreat/Battery-Emulator/tree/main/Software/src/devboard/hal

## PWM testmode (Lilygo or Stark CMR)
As of release 10.2.0 and above there is a testmode to drive the HIA4V1. This allows you generate a voltage and check polarity.

To activate the testmode configure to software:
- Ensure **NO** battery is configured
- Select the option "External precharge via HIA4V1"
- "Precharge, maximum ms before fault" defines the time this testmode will run

After startup/reboot this will enable the PWM for the duration set and allow you to check the polarity is as expected and measure the voltage.
I measured the following:
- With bias resistors installed powered with 12V **without** battery connected: 247V DC
- With bias resistors installed powered with 12V **with** battery connected: 287V DC

(The voltage indeed went up when the battery was connected, BMS powered, but not configured in BE.)


