> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

## GM BEV2 platform
The 60-66kWh battery in the GM BEV2 platform can be found in the following vehicles

* Opel Ampera-E (2017-2021)
* Chevrolet Bolt (2016-2023)

2020+ models are 64 KWh, earlier are 57 kWh

## Software configuration
For this battery type, use the option called "Chevrolet Bolt EV/Opel Ampera-E" under the "Battery Protocol" setting

<img width="656" height="156" alt="image" src="https://github.com/user-attachments/assets/bdabd2b7-e76f-47d6-abb3-c2205705b160" />

Also remember to configure the allowed charging power, since we do not read this value via CAN.

The battery uses 12V controlled contactors, so use `Contactor Control via GPIO` if you want Battery-Emulator to also control the contactors via GPIO

<img width="514" height="49" alt="image" src="https://github.com/user-attachments/assets/7cc2dd92-0c19-4dfe-97f8-e5a74b855dee" />

One user reported using 3x this type of relay with the LilyGo hardware 
https://nl.aliexpress.com/item/1005005622431177.html?spm=a2g0o.order_list.order_list_main.126.d91579d2FEesY4&gatewayAdapt=glo2nld

## Wiring diagrams
Connect the low voltage wiring like this:

X358 connector (black) 

- Pin 1 & 2 should be grounded (if pin 1 has 5V we lock the battery?)
- Pin 3 12v+ positive contactor
- Pin 4 12v+ negative contactor
- Pin 5 12v+ positive contactor for aux connector 
- Pin 7 12v+ precharge
- Pin 8 is coolant temp, pin 9 is ref. to pin 8.
- Pin 10 to ground

X357 connector (gray)
- Pin 1 fused with 10A (+12V)
- Pin 2 we wake up the sensors (+12V)
- Pin 3 & 4 relevant CAN H/L (?)
- Pin 5 & 6 relevant CAN H/L (?)
- Pin 7 & 8 relevant CAN H/L (?)
- Pin 9 is blocking ignition if we feed 5V
- Pin 10 wake up BMS (+12V)
- Pin 11 wake up communication (+12V)
- Pin 12 to ground

**All three CAN's should be connected to BE device.**


The following diagrams will help you to connect to the CAN buses on the Opel/Chevy batteries:

> [!NOTE]
> These batteries have multiple CAN buses that needs to be jumpered together to form one large CAN bus
    the resistor on the Lilygo must be removed
Example, development environment with contactor control via GPIO, and all 3x CAN buses connected together as one single bus

![image](https://github.com/user-attachments/assets/6a2346d0-837f-47a7-946c-3925cf8d6d8f)


![AC laden](https://github.com/user-attachments/assets/395ed2a6-af3b-4a25-9b1a-461fb95f7f39)
![DC laden](https://github.com/user-attachments/assets/749eeaf7-9cc6-4c11-a8b3-3ec8cc63684c)
![temperatuur regeling accu verwarming ](https://github.com/user-attachments/assets/57b218a0-c6cc-4ee2-a134-856f44f86c7c)

![aansluitingen hoogspannings kabels ](https://github.com/user-attachments/assets/c1a3dd81-34a6-44f7-af43-0535cb41e57c)
![activeren seriele gegevens ](https://github.com/user-attachments/assets/c4ac15b5-5e65-49e3-a087-2d056ca1be83)
![activeren seriele gegevens canbus ](https://github.com/user-attachments/assets/fa36a9c4-f6fe-4292-99b5-b2324622052e)
![blokkeerlus](https://github.com/user-attachments/assets/cf5db385-6801-4288-87d9-aaf27944f5a2)
![bus hoogspanning beheer](https://github.com/user-attachments/assets/adb23647-387c-4f7d-90d9-3a4972b9663b)
![canbus in schakelen ](https://github.com/user-attachments/assets/f1c24a8f-b8ec-40fa-b148-a41e4818ca91)
![contactors hoogsanning regeling ](https://github.com/user-attachments/assets/1f4fdff4-e223-4172-96c9-23f7283a75d5)
![contactors hoogspanning regeling](https://github.com/user-attachments/assets/93c92af1-0c91-477b-8bbe-c4b401ed51a0)
![contactors negatief](https://github.com/user-attachments/assets/caf9f3ea-3d71-49b4-9ebc-823a6c22e853)
![contactors positief](https://github.com/user-attachments/assets/350ee2fe-27a8-4603-bd9b-ab3f4a12c50b)
![Hi speed 1](https://github.com/user-attachments/assets/3928259c-f96c-4ee4-8aa4-0918a6836008)
![Hi speed 2](https://github.com/user-attachments/assets/a967d61d-b220-45e6-ae84-a97c8668127d)
![voeding en massa HV accu 2 ](https://github.com/user-attachments/assets/52bcf277-b7e6-4239-b833-40336a79cefc)
![voeding en massa HV accu](https://github.com/user-attachments/assets/8a9c1f2a-3f25-4a61-8b78-cea669f68e61)
Stekker X 357 OEM 33472-1259 Service Connector 19333239
* X357
![x357](https://github.com/user-attachments/assets/f8606e24-79bf-44c7-be57-06f41eca5868)
![x357 pen pos](https://github.com/user-attachments/assets/0cefb057-57d0-4745-a487-e8e244120611)

X358 
![X358](https://github.com/user-attachments/assets/1d9b89e4-3cd3-4a90-88d1-94fd11e6ed30)

![X358 pin pos ](https://github.com/user-attachments/assets/71f28f61-b2d6-4017-abb8-97f3c2675204)

 I had connectors but these fits also, one needs to modify by yourself 

 https://nl.aliexpress.com/item/1005008320223516.html?spm=a2g0o.detail.similar_items.1.3023vs3wvs3wW0&utparam-url=scene%3Aimage_search%7Cquery_from%3Adetail_bigimg&algo_pvid=8f883bc9-24d0-4568-842b-10652fe911a8&algo_exp_id=8f883bc9-24d0-4568-842b-10652fe911a8&pdp_ext_f=%7B%22order%22%3A%2216%22%7D&pdp_npi=4%40dis%21EUR%213.12%213.00%21%21%2125.47%2124.53%21%402103892f17530145939331841e26a9%2112000044596715592%21sea%21NL%21828630060%21X

Disconnect switch 24281696 24288304  24291219  or latest part number 24294004

Cotactors must be connected with lilygo via the GPIO pins see:
https://github.com/dalathegreat/Battery-Emulator/wiki/Contactor-Control-via-GPIO-pins

Please note that the precharge contactor is placed in the negative line of battery. To make precharge work the positive contactor must close before precharge contactor. Workaround: Swap Positive and negative control-lines to relays.

