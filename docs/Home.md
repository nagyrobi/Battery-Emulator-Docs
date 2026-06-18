> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

# Welcome to the Battery-Emulator wiki!

## How do I get started?
- Pick a [supported solar inverter](https://github.com/dalathegreat/Battery-Emulator/wiki#supported-inverters-list) (solar panels optional) 🌞 
- Pick a [supported battery](https://github.com/dalathegreat/Battery-Emulator/wiki#supported-batteries-list) 🔋 
- Order the Battery-Emulator [compatible hardware](https://github.com/dalathegreat/Battery-Emulator/wiki#where-do-i-get-the-hardware-needed) 🤖 
- Follow the [installation guidelines](https://github.com/dalathegreat/Battery-Emulator/wiki/Installation-guidelines) section for how to install and commission your battery properly 📓 

## Where do I get the hardware needed?
There are many hardware kits that can run the Battery-Emulator software. Cheap option is the "LilyGo T-2CAN" (2x CAN). For those that need more reliable and certifiable hardware, the "Stark CMR" is highly recommended. Amount of stars ⭐ signal how easy to use the hardware is for a newcomer:

|  Product |  Product Link | Notes | CAN interfaces | Newcomer friendly |
| :--------: | :---------: | :---------: | :----------: | :----------: |
| LilyGo T-CAN485 |  [Wiki page](https://github.com/dalathegreat/Battery-Emulator/wiki/Hardware:-LilyGo-T%E2%80%90CAN485)   | Cheap! CAN & Modbus! | 1 (+ 1 add-on) | ⭐
| Stark CMR Module | [Wiki page](https://github.com/dalathegreat/Battery-Emulator/wiki/Hardware:-Stark-CMR) | Professional HW, CE certified, Massive I/O | 2 (+ 1 add-on) | ⭐⭐⭐
| LilyGo T-2CAN | [Wiki page](https://github.com/dalathegreat/Battery-Emulator/wiki/Hardware:-LilyGo-T%E2%80%902CAN) | Cheap! Dual isolated CAN | 2 (+ 1 add-on) | ⭐⭐⭐
| Waveshare ESP32-S3-RS485-CAN  | [Wiki page](https://github.com/dalathegreat/Battery-Emulator/wiki/Hardware:-Waveshare-ESP32%E2%80%90S3%E2%80%90RS485%E2%80%90CAN) | Cheap! CAN & Modbus! | 1 (+ 1 add-on) | ⭐⭐
| ESP32 Devkit V1 | [Wiki page](https://github.com/dalathegreat/Battery-Emulator/wiki/Hardware:-ESP32-DevKit-V1) | Build your own! For expert tinkerers | | ⭐

> [!NOTE]  
> There is no way to purchase a pre-programmed device. This is a hobbyist open source project. You will be responsible for loading the software and setting it up correctly for your components. There is however a [support Discord group](https://www.patreon.com/dala) available.

## Supported inverters list
The following solar inverters have support. Remember to double check that the voltage range of your inverter matches the battery you intend to use. Click the link in the "Vendor" tab to get a more detailed page about how to set up a specific inverter with the Battery-Emulator. Also pay attention to the star rating to see how easy the inverter is to use.

- ⭐  Compatible, but requires significant effort to setup and operate. Not recommended for beginners.
- ⭐⭐  Works well with some limitations.
- ⭐⭐⭐ Very well supported inverter. Multiple success stories. Beginner friendly.
- 🔌 The inverter can also function entirely offgrid

|  Vendor |  Product Name | phases | Supported | Notes | Battery voltage |
| :--------: | :---------: | :---------: | :---------: | :----------: | :---------: |
| [Afore](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Afore)  | [Afore AF17K-THA 230V](https://www.aforenergy.com/product/three-phase-hybrid-storage-inverter-3-17-kw-delta-voltage-series/) | 3 | ( ⭐⭐ )  | Good option for Norway IT | 150–800V
| Canadian Solar | CSI-TE & CSI-HE & CSI-BHE Series | | ⚠️ | Untested, works with HVS / HVM
| [Bluesun](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Bluesun)  | [BSE8KH3/10KH3/12KH3/15KH3](https://ensun.sk/storage/bse8_10_12_15kh3_en_v220413.pdf) | 3 | ( ⭐⭐ )  |  | 125–600V
| Canadian Solar | CSI-TE & CSI-HE & CSI-BHE Series | | ⚠️ | Untested, works with HVS / HVM
| [Deye](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Deye) | [SUN-(5-20)K-SG01HP3-EU-AM2](https://www.deyeinverter.com/product/hybrid-inverter-1/sun5-6-8-10-12-15-20ksg01hp3euam2-520kw-three-phase-2-mppt-hybrid-inverter-high-voltage-battery.html) | 3 |( ⭐⭐⭐ ) 🔌|| Requires precharge
| [Deye](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Deye) | [SUN-(29.9-50)K-SG01HP3-EU-BM3](https://www.deyeinverter.com/product/hybrid-inverter-1/sun29-9-30-35-40-50ksg01hp3eubm3-4-29-950kw-three-phase-2-3-4-mppt-hybrid-inverter-hv-battery-supported-339.html) | 3 | ( ⭐⭐⭐ ) 🔌 |Both BYD_CAN and PYLON_CAN work | Requires precharge
| [Sunsynk](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Deye) | Deye rebrand | 3 | ( ⭐⭐⭐ ) |Both BYD_CAN and PYLON_CAN work | Requires precharge
| [Felicity](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Felicity) | [T-REX-50KHP3G01](https://www.felicityess.com/product/hybrid-inverter-t-rex-50khp3g01/) | 3 | ( ⭐ ) | | 160-800V 
| [Ferroamp](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Ferroamp) | [EnergyHub](https://ferroamp.com/en/products/energyhub/) | 3 |( ⭐⭐ )||174-432V
| [FoxESS](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-FoxESS-H1-H3-AC1-KH) | [FoxESS H1/AC1](https://www.fox-ess.com/1-phase/) | 1 | ( ⭐⭐ ) | Cannot co-exist on EV battery CAN channel. Needs its own CAN channel!| 80-480V
| [FoxESS](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-FoxESS-H1-H3-AC1-KH) | [FoxESS H3/AC3](https://www.fox-ess.com/3-phase/) | 3 | ( ⭐⭐ ) | Cannot co-exist on EV battery CAN channel. Needs its own CAN channel!| 180-600V
| [FoxESS](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-FoxESS-H1-H3-AC1-KH) | [FoxESS H3 Smart](https://www.fox-ess.com/3-phase/) | 3 | ( ⭐⭐ ) | Cannot co-exist on EV battery CAN channel. Needs its own CAN channel!| 100-800V
| [FoxESS](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-FoxESS-H1-H3-AC1-KH) | [FoxESS H3 Pro](https://www.fox-ess.com/3-phase/) | 3 | ( ⭐⭐ ) | Cannot co-exist on EV battery CAN channel. Needs its own CAN channel!| 150-800V
| [Fronius](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Fronius) | [Primo Gen24 Plus](https://www.fronius.com/en-gb/uk/solar-energy/installers-partners/products-solutions/residential-energy-solutions/gen24plus-inverter-with-flexible-backup-power) | 1 |( ⭐⭐⭐ ) 🔌||150-455V (Precharge inside inverter)
| [Fronius](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Fronius) | [Symo Gen24 Plus](https://www.fronius.com/en-gb/uk/solar-energy/installers-partners/products-solutions/residential-energy-solutions/gen24plus-inverter-with-flexible-backup-power) | 3 |( ⭐⭐⭐ ) 🔌||160-531V (Precharge inside inverter)
| [Fronius](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Fronius) | Symo Hybrid 3.0/4.0/5.0-3-S |  |( ⭐⭐⭐ )||150-455V
| [Fronius](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Fronius) | Verto Plus 15-33kW |  |( ⭐⭐⭐ ) 🔌 | Same as Gen24|xxx-700V
| [GE](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Goodwe) | GEH5.0/8.6/10-1U-10 | 1 |( ⭐⭐ )| GEH10 Tested. Made by GoodWe |
| [GoodWe](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Goodwe) | [ET hybrid solar](https://en.goodwe.com/et-plus-series-three-phase-hybrid-solar-inverter) | 3 |( ⭐⭐⭐ ) 🔌||
| [GoodWe](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Goodwe) | [BT AC battery inverter](https://en.goodwe.com/bt-series-three-phase-ac-coupled-retrofit-solar-inverter) | 3 |( ⭐⭐⭐ )||
| [GoodWe](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Goodwe) | [EH hybrid solar](https://en.goodwe.com/eh-series-single-phase-hybrid-solar-inverter) | 1 |( ⭐⭐⭐ )||
| [GoodWe](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Goodwe) | [BH AC battery inverter](https://en.goodwe.com/bh-series-single-phase-ac-coupled-retrofit-solar-inverter) | 1 |( ⭐⭐⭐ )||
| [GoodWe](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Goodwe) | [EHB hybrid inverter (South Africa only)](https://en.goodwe.com/ehb-series-single-phase-hybrid-solar-inverter) | 1 |( ⭐⭐⭐ )||
| [GoodWe](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Goodwe) | [A-ES hybrid inverter (America Split Phase)](https://en.goodwe.com/a-es-series-split-phase-hybrid-solar-inverter-for-north-america) | 1 |( ⭐⭐⭐ )||
| [Growatt HV](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Growatt-HV) | Growatt SPH | | ( ⭐⭐ ) | 
| [Growatt WIT](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Growatt-HV) | WIT 50-100K-HU/AU | | ( ⭐⭐ ) |
| [Growatt LV](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Growatt-LV) | N/A | | ⚠️ :zap: | Testing ongoing!
| [Hoymiles](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Hoymiles) | HYT/HAT-HV-series, HYT-10.0HV-EUG1 tested | 3 | ( ⭐⭐ )🔌 | 
| [Huawei](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Huawei-SUN2000) | SUN2000 | | ❌ Requires DC/DC converter, NOT supported | [Low Voltage alternative](https://sunvault.ro/shop/acumulatori-litiu/precomanda-solutie-stocare-16-kwh-pentru-invertoarele-huawei/) | 48V |
| [Ingeteam](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Ingeteam) | [STORAGE 1Play TL M (3-6 kW)](https://www.ingeteam.com/us/en-us/sectors/energy-storage/p15_57_42/storage-1play-tlm.aspx) | 1 |( ⭐⭐ )||
| KACO | [Blueplanet hybrid 6.0- 10.0 TL3](https://kaco-newenergy.com/products/blueplanet-hybrid-100-tl3) | 3 | ⚠️ | Untested, works with HVS / HVM
| [Maple Leaf Northern Fox](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-FoxESS-H1-H3-AC1-KH) | [H1-H3-AC1-KH](https://solarpowerstore.ca/products/fox-h1-ac1-split-phase-hybrid-inverter?variant=45975399399639) | 1/3 | ( ⭐⭐ ) | Rebranded FoxESS inverter | 150-850V
| [Kostal Plenticore](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Kostal) | N/A | | ( ⭐ ) | RS485 | 120-650V
| [SAJ](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-SAJ) | [H2 Hybrid](https://global.saj-electric.com/solarenergy-detail/63) | 3 | ( ⭐ )
| [Schneider](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Schneider) | XW PRO | | ⚠️ ⚡  | Testing ongoing! | 48V
| [SMA Sunny Boy Storage](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-SMA) | [2.5 / 3.7 / 5.0 / 6.0](https://www.sma.de/en/products/battery-inverters/sunny-boy-storage-37-50-60) | 1 | ( ⭐⭐ ) | Cannot co-exist on EV battery CAN channel. Needs its own CAN channel! | 100-550V |
| [SMA Sunny Island](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-SMA) | 4.4M / 6.0H / 8.0H | | ⚠️ ⚡  | Testing ongoing! | 48V
| [SMA Sunny Boy Smart Energy](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-SMA) | 3.8 / 4.8 / 5.8 / 7.7 / 9.6 / 11.5 -US-50 Hybrid Inverter | 1 | ( ⭐⭐⭐ ) | Cannot co-exist on EV battery CAN channel. Needs its own CAN channel! | 90V-500V | 
| [SMA Sunny Tripower SE](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-SMA) | [Smart 5.0-10.0 SE](https://www.sma.de/en/products/hybrid-inverters/sunny-tripower-smart-energy) | 3  | ( ⭐⭐⭐ ) | Cannot co-exist on EV battery CAN channel. Needs its own CAN channel! | 150-600V | 
| [Sofar](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Sofar) | [5K...20KTL-3PH](https://sofarsolar.eu/products/hyd-5k20ktl-3ph/) | 3 | ( ⭐⭐ )
| [SolaX](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Solax) |[X1 Hybrid](https://uk.solaxpower.com/x1-hybrid-g4/) | 1 | ( ⭐⭐ )| Cannot co-exist on EV battery CAN channel. Needs its own CAN channel! | 80-480V |
| [SolaX](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Solax) |[X3 Hybrid](https://uk.solaxpower.com/products/x3-hybrid-g4/) | 3 |( ⭐⭐ )| Cannot co-exist on EV battery CAN channel. Needs its own CAN channel!
| [SolaX](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Solax) |[X3-Ultra Hybrid](https://www.solaxpower.com/our-products/x3-ultra.html) | 3 |( ⭐⭐ )| Cannot co-exist on EV battery CAN channel. Needs its own CAN channel!
| [Solxpow](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Solxpow) | Solxpow X3 12-20kW | 3 | ( ⭐⭐ ) |  |135-750V
| [Solinteg](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Solxpow) | MHT 10-20kW | 3 | Untested, should work |  |135-750V
| [Solis](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Solis) | Various models, please see page for details | 1 and 3 | ( ⭐⭐⭐ ) | Protocol seems to work for most models, however voltage range differs! |120-800V
| [Sol-Ark](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Sol%E2%80%90Ark-LV) | LV 48V | ? | ⚠️ Testers wanted  | | 48V | 
| [Sungrow](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Sungrow) | [SH15/20/25T](https://www.sungrowpower.com/en/products/residential-energy-storage-system/sht-15-20-25) | 3 |( :star::star: )| BYD protocol|100-700V
| [Sungrow](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Sungrow) | [SH5.0/6.0/8.0/10RT](https://en.sungrowpower.com/productDetail/905/mv-power-converter-hybrid-inverter-sh5-0-6-0-8-0-10rt) | 3 |( ⭐⭐ )| Some AU models not supported|150-600V
| [Sungrow](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Sungrow) | [SH3.0/3.6/4.0/5.0/6.0RS](https://en.sungrowpower.com/productDetail/2444/mv-power-converter-hybrid-inverter-sh3-0-3-6-4-0-5-0-6-0rs) | 1 |( ⭐⭐ )| BYD protocol | 80-460V
| [Sungrow](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Sungrow) | [SH8.0/10.0RS](https://en.sungrowpower.com/productDetail/2444/mv-power-converter-hybrid-inverter-sh3-0-3-6-4-0-5-0-6-0rs) | 1 |( ⭐⭐ )| Sungrow SBR Protocol only | 80-460V
| VIESSMANN | Hybrid Inverter 5.0/6.5/8.0/10.0 A-3 | | ⚠️ | Untested, works with HVS / HVM
| [Vevor](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Vevor) | Multiple models | | ( ⭐ )  | RS485 | 48V
| [ZCS Azzurro](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-ZCS-Azzurro) | TODO | 1/3 |( ⭐ )|
| [Yingfa](https://github.com/dalathegreat/Battery-Emulator/wiki/Inverter:-Yingfa) | Hybrid inverter 5kw | 1 |( ⭐⭐ ) | Needs external precharge | 85-400V


## Supported batteries list

Be sure to checkout the [installation guidelines](https://github.com/dalathegreat/Battery-Emulator/wiki/Installation-guidelines) section for how to install your battery. Amount of stars ⭐ signal how mature and stable the integration is.

- ⭐ Works, but many values estimated or functionality missing. Expect manual tweaking to keep battery operational
- ⭐⭐ Integration has minor known issues or missing features. Manual interventions sometimes required.
- ⭐⭐⭐ Very well supported battery. Longterm stability confirmed without user interaction. Many success stories from users.

If the battery has a 🅱️ symbol, cell balancing has been confirmed working (Important for longterm operation)

If the battery has a 2️⃣ or 3️⃣ symbol, double- or triple battery is supported


|  (Car) Manufacturer |  Product Name | # kWh | Supported | Voltage range | Notes |
| :--------: | :---------: | :---------: | :---------: |:---------: | :---------: |
| BMW | [BMW i3 (all sizes)](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-BMW-i3) | 18/22/40 | ✅ ( ⭐⭐⭐ ) 🅱️ 2️⃣  | 270-400V | Balancing only when powered off |
| BMW |  [iX & i4-7 Platform](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-BMW-iX,-i4%E2%80%90i7-(Gen5-platform)) |68 / 80.7| ✅ ( ⭐⭐ ) 🅱️  | 270-464V | Manual contactor control
| BMW |  [PHEV (330e/530e etc)](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-BMW-PHEV-(Gen-3&4-platform)) | 12/24 | ⚠️ Testing ongoing (2021+ Gen4 CAN logs wanted!) | 270-400V | Manual contactor control
| BYD | [BYD Atto 3](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-BYD-vehicle-(Atto-3-%E2%80%90-Seal-%E2%80%90-Tang-%E2%80%90-Dolphin-%E2%80%90-Song-%E2%80%90-and-more!)) | 50/60 | ✅ ( ⭐⭐ ) 2️⃣ | 380-440V
| BYD | [BYD Yuan Plus](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-BYD-vehicle-(Atto-3-%E2%80%90-Seal-%E2%80%90-Tang-%E2%80%90-Dolphin-%E2%80%90-Song-%E2%80%90-and-more!)) | 50/60 | ✅ ( ⭐⭐ ) 2️⃣ | 380-440V
| BYD | [BYD Dolphin mini/E3](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-BYD-vehicle-(Atto-3-%E2%80%90-Seal-%E2%80%90-Tang-%E2%80%90-Dolphin-%E2%80%90-Song-%E2%80%90-and-more!)) | Multiple | ✅ ( ⭐⭐ ) 2️⃣ | 307/332V
| BYD | [BYD Seal](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-BYD-vehicle-(Atto-3-%E2%80%90-Seal-%E2%80%90-Tang-%E2%80%90-Dolphin-%E2%80%90-Song-%E2%80%90-and-more!)) | Multiple | ✅ ( ⭐⭐ ) 2️⃣ | 380-440V
| BYD | [BYD Song](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-BYD-vehicle-(Atto-3-%E2%80%90-Seal-%E2%80%90-Tang-%E2%80%90-Dolphin-%E2%80%90-Song-%E2%80%90-and-more!)) | Multiple | ✅ ( ⭐⭐ ) 2️⃣ | 380-440V
| Chevrolet | [Bolt](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Ampera%E2%80%90e-64-kWh) | 60/66 | ✅ ( ⭐ ) | 330-403 |
| Citroen | [Basalt (Stellantis CMP Smart Car)](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Stellantis-CMP-Smart-Car-platform) | 44 | ✅ ( ⭐⭐ ) 🅱️   | 300-350V
| Citroen | [C-Zero](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-i%E2%80%90Miev-CZero-Ion) | 16 | ✅ ( ⭐ ) | 310-370V
| Citroen | [ë-C3 / Aircross (Stellantis CMP Smart Car)](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Stellantis-CMP-Smart-Car-platform) | 29/44 | ✅ ( ⭐⭐ ) 🅱️   | 300-350V
| Citroen | [ë-C4 2020+ (Stellantis eCMP)](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Stellantis-eCMP-(Citroen,-DS,-Opel,-Peugeot)) | 50 | ✅ ( ⭐⭐ ) 🅱️   | 320-450V
| Citroen | [Spacetourer (Stellantis eCMP)](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Stellantis-eCMP-(Citroen,-DS,-Opel,-Peugeot)) | 50/75 | ✅ ( ⭐⭐ ) 🅱️  | 320-450V
| Dacia | [Spring](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Dacia-Spring-%E2%80%90-Renault-K%E2%80%90ZE) | 27 | ✅ ( ⭐⭐ ) 2️⃣ | 216-302V
| Fiat | [Grande Panda (Stellantis CMP Smart Car)](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Stellantis-CMP-Smart-Car-platform) | 44 | ✅ ( ⭐⭐ ) 🅱️   | 300-350V
| Ford | [Mustang Mach-E](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Ford-Mach%E2%80%90E) | 66/88 | ✅ ( ⭐⭐ ) | 300-410V
| Ford | [E-Transit](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Ford-Mach%E2%80%90E) | 66 | ✅ ( ⭐⭐ ) | 300-410V
| Geely | [Geometry C](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Geely-Geometry-C) | 53/70 | ⚠️ Testing ongoing | 270-417V*
| Hyundai |  [E‐GMP platform](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Hyundai-E%E2%80%90GMP-platform-(58.2-%E2%80%90-77.4-kWh)) |58.2/72.6/77.4/84| ⚠️ (Integration ongoing!) | 430-806V | SoC not updating, contactors not closing 
| Hyundai | [Kona 39/64kWh](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Kia-Niro---Hyundai-Kona-64-kWh) | 39/64 | ✅( ⭐⭐⭐ )2️⃣🅱️| 230-410V*
| Hyundai | [Kona Hybrid](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Kia-Niro-Hybrid) | 2 | ⚠️ Testing ongoing | 
| Hyundai | [Santa Fe PHEV](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Hyundai-Santa-Fe-PHEV) | 14 | ✅ ( ⭐⭐ )  | 290-400V
| Jaguar | [I-PACE](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Jaguar-I%E2%80%90PACE) | 90 | ⚠️ Testing ongoing | 
| Kia | [e-Niro 39/64kWh](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Kia-Niro---Hyundai-Kona-64-kWh) | 39/64 | ✅( ⭐⭐⭐ )2️⃣🅱️| 230-410V*
| Kia | [Niro HEV / Ceed PHEV](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Kia-Niro-Hybrid) | 2 / 8.9 | ✅ ( ⭐ ) |
| Kia | [XCeed PHEV](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:%E2%80%90Kia%E2%80%90Xceed%E2%80%90PHEV) | 8.9 | ⚠️ (Integration ongoing!) | 240V – 413V
| Kia | [EV6](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Kia-EV6) | 58.2/72.6/77.4 | ⚠️ Testing ongoing | 430-806V | SoC not updating, contactors not closing 
| Land Rover | [Land Rover](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Land-Rover) | | ⚠️ Untested base added |
| Mini | [Cooper electric](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-BMW-i3) | 32 | ✅ ( ⭐⭐⭐ )  | 270-400V
| Mitsubishi | [i-Miev](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-i%E2%80%90Miev-CZero-Ion) | 16 | ✅ ( :star: )  | 310-370V
| Mitsubishi | [eK X EV](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Nissan-Sakura---Mitsubishi-eK-X-EV) | 20 | ✅ ( ⭐ )  | 300-400V
| MG | [HS PHEV](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-MG-HS-PHEV) | 16 | ✅ ( ⭐ )  | 310-378V
| MG | [MG4](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-MG4) | 51/64/77 | ⚠️ Testing ongoing   | 260-470 V
| MG | [MG5 - Marvel R](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-MG5-%E2%80%90-Marvel-R) | 50/52/61/70 | ✅ ( ⭐⭐ ) 🅱️  | 268-438 V
| Nissan | [Ariya](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Nissan-Ariya) | 87/63 | ⚠️ (CAN logs wanted!) | 
| Nissan | [LEAF](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Nissan-LEAF---e%E2%80%90NV200) | 24/30/40/62 | ✅ ( ⭐⭐⭐ ) 🅱️3️⃣  | 300-400V
| Nissan | [e-NV200](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Nissan-LEAF---e%E2%80%90NV200) | 24/40 | ✅ ( ⭐⭐⭐ ) 🅱️3️⃣  | 300-400V
| Nissan | [Sakura](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Nissan-Sakura---Mitsubishi-eK-X-EV) | 20 | ✅ ( ⭐ )  | 300-400V
| Opel | [Frontera (Stellantis CMP Smart Car)](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Stellantis-CMP-Smart-Car-platform) | 44 | ✅ ( ⭐⭐ ) 🅱️   | 300-350V
| Opel | [Ampera-e](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Ampera%E2%80%90e-64-kWh) | 60/66 | ✅ ( ⭐  ) | 330-403 |
| Peugeot | [Ion](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-i%E2%80%90Miev-CZero-Ion) | 16 | ✅ ( ⭐  )  | 310-370V
| Polestar | [2](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Volvo-XC40---Polestar-2) | 78 | ✅ ( ⭐ ⭐  )  | 290-450V |
| Renault | [Kangoo](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Renault-Kangoo) | 22/33 | ✅ ( ⭐⭐ )  | 300-400V
| Renault | [Fluence ZE](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Renault-Fluence-ZE) | 22/36 | ✅ ( ⭐ )  | 300-400V
| Renault | [K‐ZE](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Dacia-Spring-%E2%80%90-Renault-K%E2%80%90ZE) | 27 | ✅ ( ⭐⭐ ) | 216-302V
| Renault | [Twizy](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Renault-Twizy) | 6.1 | ⚠️ Testing ongoing | 48V
| Renault | [Zoe Gen1](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Renault-Zoe-Gen1) | 22/41 | ✅ ( ⭐⭐⭐ ) 2️⃣ |  300-400V
| Renault | [Zoe Gen2](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Renault-Zoe-Gen2) | 52 | ✅ ( ⭐⭐⭐ ) 2️⃣🅱️ |  300-400V
| Rivian | [R1T](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Rivian-R1T) | 135 | ✅ ( ⭐⭐ )  |  300-400V
| Tesla | [Model 3 & Y (all sizes)](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Tesla-Model-S-3-X-Y) | All | ✅ ( ⭐ )  | 280-400V | Note, balancing only when contactors are opened!
| Tesla |  [Model S & X (2021+)](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Tesla-Model-S-X) | All | ✅ ( ⭐ )  | 310-460V | Note, balancing only when contactors are opened!
| Tesla | [Model S & X (2012-2020)](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Tesla-Model-S-X-2012%E2%80%902020) | All | ✅ ( ⭐ )  | 280-400V |
| Toyota | [Proace (Stellantis eCMP)](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Stellantis-eCMP-(Citroen,-DS,-Opel,-Peugeot)) | 50/75 | ✅ ( ⭐⭐ ) 🅱️  | 320-450V
| Volkswagen, Audi, Škoda, Cupra and Ford | [MEB Platform](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-MEB) | 48/55/61/82 | ✅ ( ⭐⭐ ) 🅱️  | 252-450V* | Requires [precharge circuit](https://github.com/dalathegreat/Battery-Emulator/wiki/High-Voltage-source)
| Volvo | [EX30](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Volvo-EX30) | 55/69 | ✅ ( ⭐ ) | 300-460V | 
| Volvo | [XC40/C40](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Volvo-XC40---Polestar-2) | 69/78/82 | ✅ ( ⭐⭐ )  | 290-450V | Requires DC/DC load, can lock itself permanently
| Volvo | [SPA PHEV](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Volvo-SPA-S60-90-V60-90-XC60-90-Hybrid-batteries) | 19 | ✅ ( ⭐ )  | 290-450V | Requires DC/DC load, can lock itself permanently

### Other batteries
* DIY HV battery:
   * [RJXZS BMS](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-RJXZS-BMS) ✅
   * [Orion BMS](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Orion-BMS) ✅
   * [Simp BMS](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-SimpBMS) ✅
   * [DALY BMS](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Daly-SmartBMS) ✅
   * [EMUS G1 BMS](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-EMUS-G1-BMS) ✅
   * [Ennoid BMS](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Ennoid-BMS) ⚠️ (Not tested)
* DIY LV battery:
   * [DALY BMS](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Daly-SmartBMS) ✅
* [FoxESS HV2600 batteries](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-FoxESS-HV2600) ✅
* [Pylon HV batteries (Dyness Tower)](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Pylon-HV) ✅
* [CHAdeMO vehicles](https://github.com/dalathegreat/Battery-Emulator/wiki/Chademo-vehicle) ⚠️ (Experimental support for emergencies)

## Supported chargers list (optional)
Emergency charging batteries via a generator, supported via the following standalone chargers:
* [Chevrolet Volt Gen1](https://github.com/dalathegreat/Battery-Emulator/wiki/Charger:-Chevrolet-Volt-Gen1)
* [Nissan LEAF 2013-2024 PDM](https://github.com/dalathegreat/Battery-Emulator/wiki/Charger:-Nissan-LEAF-PDM)

## Shunts
* [BMW S-BOX](https://github.com/dalathegreat/Battery-Emulator/wiki/Shunt:BMW-SBOX)

## How do I configure the software for my battery/inverter?

<img alt="image" src="https://github.com/user-attachments/assets/41ca33b3-5eb7-4847-9de5-3a5b2b74147c" />

All the changes to the software are done on the _Change Settings_ page, which can be accessed thru the Webserver. At the top of this webpage, you can select which battery, inverter protocol and what interface they are connected to. If you are unsure which protocol you need, check the specific page for the battery/inverter you are using linked here in the Wiki

## How do I know what battery/inverter interface to use
This depends which hardware you are using for the Battery-Emulator. For instance Stark CMR uses "Native CAN" for CAN1, and "Native CAN FD" for CAN2. See the Wiki page for the hardware you are using for more info

## Status LED
The board has a built in LED that is used to signal current status. With this feature, it is easy to at a glance catch what info the board is getting. It will show the current colors:
* Pulses 🟢 if all is well and BMS is active
* Pulses 🟡 if battery has entered a warning state
* Solid 🔴 if battery goes into a fault state

By visiting the "Events" page in the Webserver, you can see which specific warnings/faults are active

## CAN wiring troubleshooting

This section has been expanded [and moved here](https://github.com/dalathegreat/Battery-Emulator/wiki/CAN-wiring-practices-and-troubleshooting) 

Note! CAN networks are vulnerable to lightning strikes. [See the dedicated wiki page for this for more info](https://github.com/dalathegreat/Battery-Emulator/wiki/Lightning-strike) :cloud_with_lightning: 

## What about safety? ⚠️ ℹ️
Reusing old often crashed EV packs always comes with risks. The system performs a few safety functions for safer charging and discharging. Apart from this, the data sent to the Inverter is also processed on the inverter side, and depending on which inverter is used a few additional safety checks are performed there. Here is a list of all safety functionalities that are in the system. Note that almost all safety features rely on communication data, so a physical error (damaged cell casings, ruptured/leaking cells, corrosion etc.) wont be detectable via software. For this you need fuses, and periodic visual inspections. 

> [!TIP]
> Be sure to checkout the [installation guidelines](https://github.com/dalathegreat/Battery-Emulator/wiki/Installation-guidelines) section for how to install your battery

> [!CAUTION]
> ***At the end of the day, you alone are responsible for the system.***

Safety features run on (most) inverter(s):
- Battery sends max total voltage allowed for charging. Incase this value is reached, inverter stops charging. (For instance 404V)
- Battery sends min total voltage allowed for discharging. Incase this value is reached, inverter stops discharge (For instance 300V)
- Battery sends max cell temperature. Incase this value goes too high, inverter stops charge/discharge (For instance 40*C)
- Battery sends min cell temperature. Incase this value goes too low, inverter stops charge/discharge (For instance -15*C)
- Battery sends max allowed charge in Watts. Incase this goes to 0W, no further charging is possible. (This can happen when battery is full)
- Battery sends max allowed discharge in Watts. Incase this goes to 0W, no further discharge is possible. (This can happen when battery is completely empty)
- Battery sends state of health %. Incase this value drops too low, the inverter will alert the user that it is time to recycle the battery.
- Inverter analyzes insulation resistance of the battery connection. Incase a leakage to ground is detected, the system stops.

Safety features run on Battery-Emulator side:
- If the code enters FAULT state, inverter gets notified, all charging/discharging stops, and contactors are opened ([if they are controlled via GPIO pins](https://github.com/dalathegreat/Battery-Emulator/wiki/Contactor-Control-via-GPIO-pins)).
- If CAN communication is lost between emulator and battery for more than 60s, the code enters FAULT state.
- Total pack voltage is sampled, if it goes too high it sets allowed charge power to 0. If it continues to rise, we enter FAULT mode
- Total pack voltage is sampled, if it goes too low it sets allowed discharge power to 0. If it continues to fall, we enter FAULT mode
- Minimum cell voltage is sampled, and if one cell goes too low the code enters FAULT state. (For instance <2900mV)
- Maximum cell voltage is sampled, and if one cell goes too high the code enters FAULT state. (For instance >4250mV) 
- Battery state of health % is sampled, if it is below 25% the code stops and informs the user that it is time to recycle the battery.
- BMS fault codes are sampled, if any serious code is set, the code enters FAULT state (For instance LB_Failsafe_Status on Nissan LEAF packs)
- High voltage wiring is unhooked during operation. This will trigger interlock messages, and the code enters FAULT state
- Incase of a high voltage leak to battery casing (Protective earth), the code enters FAULT state (For instance LB_Failsafe_Status on Nissan LEAF packs)

> [!IMPORTANT]  
> Do note that all actual limits are battery/inverter specific, the values here are only used for example purposes. The amount of safeties will vary depending on your choice of battery.

> [!TIP]  
> You can also add an [equipment stop button](https://github.com/dalathegreat/Battery-Emulator/wiki/Equipment-Stop) to the Battery-Emulator, to increase the amount of safety.

## Connectivity
The board has wifi, and supports running a [Webserver that you can connect to for real time values](https://github.com/dalathegreat/Battery-Emulator/wiki/Webserver-guide), [Over The Air updates](https://github.com/dalathegreat/Battery-Emulator/wiki/OTA-Update) (OTA), cellmonitoring, changing settings and more. See the [Webserver](https://github.com/dalathegreat/Battery-Emulator/wiki/Webserver-guide) page for more info on how to use the system

For those into home automation, the code also supports [MQTT](https://github.com/dalathegreat/Battery-Emulator/wiki/MQTT) 