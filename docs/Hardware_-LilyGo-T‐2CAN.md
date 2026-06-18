## Hardware basics
The LilyGo T-2CAN is a dual CAN board, excellent for integrations that require separate CAN controllers. It is very easy to use this board on multi-CAN systems compared to the LilyGo T‐CAN485. The CAN interfaces are both galvanically isolated, making it safe to use with inverters such as Solax.

> [!NOTE]
> This board does not support Modbus/RS485 or CAN FD natively, although both can be [added on](#add-ons).

<img alt="image" src="https://github.com/user-attachments/assets/5237ad57-bf97-4e99-8645-f658b34a1813" />

## Purchase link
The hardware can be bought via sites like AliExpress, or the official [LilyGo store](https://lilygo.cc/products/t-2can)

## Hardware info
The hardware has more details on LilyGo's Github page
https://github.com/Xinyuan-LilyGO/T-2Can

> [!NOTE]
> This has an included Antenna that needs to be mounted for good Wifi performance. Failure to install this will lead to connectivity issues

<img alt="image" src="https://github.com/user-attachments/assets/078b4c52-9de2-4290-9cba-8106e7984277" />

## Installing the software
Follow the [quickstart guide](https://github.com/dalathegreat/Battery-Emulator?tab=readme-ov-file#how-to-install-the-software-) to install the Battery-Emulator software onto the board for the initial setup

## Over the air (OTA) software updates
When updating this board [OTA](https://github.com/dalathegreat/Battery-Emulator/wiki/OTA-Update), be sure to select the software marked for this board. The files will be marked like this, signaling that this is **T-2CAN** hardware

`BE_vX.Y.Z_LilygoT-2CAN.ota.bin`

## Interfaces
The board comes with 2 CAN channels. One is labelled CAN-A , and the other one is CAN-B

<img alt="image" src="https://github.com/user-attachments/assets/00c63c75-1a98-475b-9464-976c271ffacc" />

The interfaces correspond to the following options in the Battery-Emulator software

- CAN-A -> **CAN MCP 2515 Add-on**
   - CANLA (CAN-LOW)
   - CANHA (CAN-HIGH)
- CAN-B -> **Native CAN**
   - CANLB (CAN-LOW)
   - CANHB (CAN-HIGH)

Example configuration, Nissan LEAF battery connected to CAN-B , and a Deye inverter connected to CAN-A

<img alt="image" src="https://github.com/user-attachments/assets/9158a529-4cc7-480c-bcbd-2570feb9b425" />

## <a name="add-ons"></a>Add-ons

The T-2CAN has two SH-1.0mm connectors with two GPIOs each, and unpopulated solder pads for power and 21 more GPIOs. This allows you to add some modules without soldering, and even more by making connections to the solder pads underneath.

> [!NOTE]
> The onboard 3.3V regulator (RT9080) is only rated for 600mA output, which does not leave much spare for add-ons (the ESP32S3 requires 500mA minimum). The 5V rail has much more capacity (3A), so if you need significant amounts of current at 3.3V you will need an additional regulator or external power.

### Modbus/RS485 (solderless)

You can connect a 4-pin auto-direction 3.3V-supply TTL-RS485 module to the T-2CAN via the first SH-1.0mm connector. LilyGo sell a [pre-made SH-1.0mm to Dupont cable](https://lilygo.cc/products/dupont-cable). The modules are readily available on Aliexpress:

<img width="270" src="https://github.com/user-attachments/assets/acc52eea-165e-44f4-a88c-bf2dc9f02262" /> 
<img width="270" src="https://github.com/user-attachments/assets/079effb4-5350-4c1c-b9c3-ba6c6e7f9472" />
<img width="270" src="https://github.com/user-attachments/assets/a8ee6ae9-f6cf-4a76-bd77-dbe5ba221a6c" />

The connections should be made like this (the colors match the LilyGo cable):

<img alt="image" src="https://github.com/user-attachments/assets/ca1f9619-5ea8-40ca-811f-482ca10adf04" />

These modules are inconsistent with their TX/RX labelling. Usually the TX pin on the module is an input, which should be connected to TX on the T-2CAN (which is an output). However on some, the TX pin on the module is an output, so should instead go to RX on the T-2CAN (and vice versa for the other pin). Choosing modules with onboard LEDs helps with debugging.
<br>If you do run into no-communication issues while everything else seems correct, switching around RX and TX will not damage anything, give it a try!

RS485 needs a 120 ohm termination resistor at each end of the bus, for best performance. This may need manually enabling on your RS485 module. For example, the blue modules have empty 'R13' pads which need a solder blob bridging them to enable the 120 ohm termination:

<img width="300" src="https://github.com/user-attachments/assets/844864f9-7cad-4d89-866b-1a3914c19304" />

The TX/RX pins are also used by the bootloader when the ESP32 starts up, which sends a brief chunk of debugging information onto the bus at 115200. This will hopefully be ignored by attached RS485 devices - if there are problems, it may be possible to burn an efuse on the ESP32S3 to disable this.

### Configurable port

The second 'QWIIC' connector (the one with GND/3V3/IO01/IO02 on the image above) can be configured for several different functions, via the settings page:

<img alt="image" src="https://github.com/user-attachments/assets/d6bbc327-0f7e-4901-95bf-e8b14309d329" />

#### WUP1 / WUP2

These signals are used as wake-up signals for some batteries.

|  T-2CAN pin |  Signal |
| :--------: | :---------: |
| IO01 | WUP1 |
| IO02 | WUP2 |

#### E-Stop / BMS Power

|  T-2CAN pin |  Signal | Function |
| :--------: | :---------: | :---------: | 
| IO01 | E-Stop | An input which performs an equipment-stop (sets the inverter current to zero). See [Equipment Stop](https://github.com/dalathegreat/Battery-Emulator/wiki/Equipment-Stop) for more information. |
| IO02 | BMS Power | An active-high output to drive a contactor/relay to power the BMS. Allows BE to power cycle it periodically. |

#### I2C Display

|  T-2CAN pin |  Signal |
| :--------: | :---------: |
| IO01 | SDA |
| IO02 | SCL |

See below for more information.

### Expansion header

The underside of the board has pads for a 26-pin 2.54mm-pitch pin header.
<img alt="image" src="https://github.com/user-attachments/assets/90373534-41ed-4cfe-8650-01e41db68482" />

Note that the configurable port setting overrides these pin assignments - for example, if you choose WUP1/WUP2 for the configurable port, these pins will be on the top QWIIC connector, and not the underside expansion header.

You can either solder directly to the pads, or attach a 2x13P header and use Duponts.

<img alt="image" src="https://github.com/user-attachments/assets/408438e3-619c-4f66-a81d-7837b74e491a" /> <img alt="image" src="https://github.com/user-attachments/assets/c4980eea-2914-458e-8fb6-0e3ef3fc1c68" /> <img alt="image" src="https://github.com/user-attachments/assets/ed94ef69-827d-44ac-adfb-21ad8d0c3d2f" />

#### MCP2518 CAN FD module

<img alt="MCP2518 module" src="https://github.com/user-attachments/assets/cf47b052-fde3-4c14-85a5-cc3d532e9ccb" />

A MCP2518 CAN FD module can be connected to the green pins on the diagram above. This can be attached with a 2x5 Dupont connector to the top section of the pin headers (you can make up your own cable with a 2x6 Dupont at the other end for the module). This provides a third non-isolated interface capable of CAN FD (required by some batteries), in addition to the existing two isolated ones.

Make sure you check the crystal frequency of your board and update the 'CAN-FD-addon crystal (Mhz)' value, eg, this one is 20 MHz:

<img width="300" alt="MCP2518 board showing 20 MHz crystal" src="https://github.com/user-attachments/assets/a4d85c09-6cde-47db-8d6c-5a7e7bd726a6" />

#### LED

You can attach a WS2812B LED to the board, connecting to IO35, 5V and GND. It may be easiest to solder this directly to the board using thin jumper wires. It is preferable to use the 5V rather than 3.3V supply as it has more spare capacity.

#### Contactors

The contactor outputs provide a 3.3V logic signal, which is insufficient to drive a contactor directly. You can drive relays via a transistor or optoisolator buffer, or use solid state relays (SSRs) which turn on fully at 3V (the voltage may sag below 3.3V).

### Screen support
The T-2CAN board can have a 128x64 SSD1306/SSD1309 I2C OLED display attached to it, that will display battery status, events and WiFi info.

Currently only on Lilygo T-2CAN, using the second QWIIC connector (SDA=GPIO1, SCL=GPIO2).

<img alt="image" src="https://github.com/user-attachments/assets/49c7223d-9697-4668-aa17-b516a4b594ed" />

#### Screen parts needed 
The screens are available in several sizes. Some are monochrome, others are two-tone (each pixel can only show one color, but different regions are different colors). The same 4-pin SH-1.0mm 'QWIIC' cable from LilyGo can be used, also available from AliExpress. Pay close attention to the pin connections, since the color code is not consistent between cables.

|  Product |  Purchase Link |
| :--------: | :---------: |
| Display |  [0.96 inch](https://a.aliexpress.com/_EInVnDS)   |
| Display |  [1.54 inch](https://www.aliexpress.com/item/1005009313931934.html) |
| Display |  [2.42 inch](https://a.aliexpress.com/_EIVtGmY)   |
| QWIIC 4pin |  [Aliexpress](https://a.aliexpress.com/_EugpEKC)   |

![image](https://github.com/user-attachments/assets/81e2a575-3e7e-4eb6-8b55-5f4b32c618cf)

#### DIN Rail Holder

STL for 3D printing 
[Open Frame DIN] (https://www.thingiverse.com/thing:7278747)
