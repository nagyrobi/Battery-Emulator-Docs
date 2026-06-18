## Webserver
You can interact with the Battery-Emulator via the built in Webserver. Here you can check battery status, change battery settings, update the software over-the-air, check active events, monitor cellvoltages plus much more! It is easy to commission a new system, and the preferred way to monitor a newly setup battery system.

> [!IMPORTANT]  
> Never expose the Battery-Emulator to the internet without a firewall. If extreme cybersecurity is a concern, you can disable network connectivity entirely in the settings.

## Prerequisites
To be able to use the Webserver, you need to connect to the Battery-Emulator to either your home Wifi, or connect directly to the access point that the Battery-Emulator itself is broadcasting.

### A: Connect to your home network
To have the Battery-Emulator accessible in your home network, you need to enter your home Wifi credentials into the webserver settings. See the quickstart guide for more information on how to perform the initial Wifi setup.

> [!NOTE]  
> SSID can max be 63 chars, and password needs to be atleast 8 chars long. Only 2.4Ghz networks are supported, 5Ghz will NOT work! 

When the board boots, it will attempt to connect to the wifi network you specified. Your router will give it a unique IP, so next up is figuring out what the actual address is. There are a few options you can do

- Connect temporarily to the Battery-Emulator wifi network, navigate to 192.168.4.1 via a webbrowser, and read out the IP address it got

![image](https://github.com/user-attachments/assets/b84b4c7a-2b54-4343-a2fc-57ba555b9ab0)

- Connect via USB and read out the serial print via Arduino serial monitor. When the board boots, it will post which IP address it got assigned to. If you only see ????? in the serial monitor, change the baud rate from 9600 to 115200
- Check your router info. Incase your home router has a login page (typically 192.168.1.1), you can via this see what devices are connected to your home network. The board will show up as esp32-xxxxx here in your router table.
- Bruteforce guessing. If your PC has the IP 192.168.1.10 , you can typically just replace the last number with a few numbers up/down and this way get a connection. For instance if your PC is 192.168.1.10 and the only other device on the network, the emulator might get the next available address, 192.168.1.11

Once the IP of the board has been determined, open a webbrowser on a device that is connected to the same home network, and type in the IP. This will open the Webserver user interface

### B: Connect to the Access Point
By default, there is an access point broadcasted by the board, called "Battery Emulator". The default password is "123456789". This can later be changed in the webserver incase you want to improve cyber-security.

After connecting your laptop/phone to this network, you can open a webbrowser and access the interface. To login , browse to the page [192.168.4.1](192.168.4.1) , this will open the Webserver user interface 

> [!TIP]  
> You can improve signal quality on the LilyGo board by adding an external Wifi antenna. You can easily salvage one from an old router. There is a SMD resistor that needs to be moved in order for the board to use the external antenna

![image](https://github.com/user-attachments/assets/31867de0-66a2-4674-a80c-a9193d389e9b)


## Using the Webserver
The front page will contain some quick information about the system. What software version the system has, Inverter protocol, Battery type, Livedata from the battery transmitted to the Inverter, along with some buttons to go to other pages. The page will be green incase all is well, go yellow incase there is an active warning, and go red incase an error is active and blocking operation. Incase there is a warning/error active, you can click the `Events` button to go to this view.

![image](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/ce1a8bef-bb20-4506-83f8-f2572bcbc45c)

#### Limiting factor
Using the webserver you can see what part is limiting the charge/discharge. It will show you if the battery is the bottleneck, or if the inverter is the limiting factor.

![image](https://github.com/user-attachments/assets/72dc64c0-4a2c-443a-87e3-9cfff4313033)

![image](https://github.com/user-attachments/assets/5084d135-9c62-4fc7-b735-a5d3be8241a2)

Note, if no power is being put in/out of the battery, the text will simply say Battery Idle

Above this text you can also see the Amperages allowed by the Emulator. You can see when the charge/discharge amperage values are limited by the battery itself (BMS), or by the user configurable settings (Manual)

![image](https://github.com/user-attachments/assets/fe0206df-7c84-480c-b711-976f7c463b43)

## Events
This page contains information about events that have occured while the system has been running. All events are timestamped, and have an occurance counter so you know if many events of the same type has triggered. 

![image](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/b9da57aa-77b3-4a9e-a543-074d8d3e7a08)

Each event also has a description field with more info. The events are grouped into three categories:

### Info ℹ️ 
Info events contain useful information like when battery has been charged full, completely discharged, reset reason etc. Having info events present does not warranty any user action

### Warning 🟡 
Warning events contain info that users might want to act upon. The system will try to mitigate certain warnings, like incase the battery is reaching too high voltage, the system will raise a warning event and prevent further charging (only discharging will be possible). Warning events should be analyzed when spotted. The front page of the webserver, plus the LED on the board will also turn yellow when a warning event is active.

### Error 🔴 
Critical error events contain info about why the system has stopped operation. Incase it is no longer safe to continue using the battery, an error event will be generated and charging/discharging is set to 0W allowed. Check the Error event description for information on how to proceed or what to check. The front page of the webserver, plus the LED on the board will also turn red when an error event is active.

## Settings
This page contains settings for how the battery should be used. They are the same settings you can put in via the 'settings_html.h' file. All settings can be changed on the fly.

![image](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/fb09db6b-72e2-46cb-a9b4-c3e7dfbe738a)

#### Battery Capacity
How much energy can your battery store? Some batteries autodetect this via CAN communication, but for some battery types that do not have this it is good to manually define the value so that your inverter knows how large the battery is.

#### Rescale SOC%
Increases battery life. If enabled, the system will rescale SOC% between the configured min/max-percentage. By not using the entire battery, the amount of cycles the battery can last increases. Good practice is to use this feature, and restrict SOC% between 20-80%.

![image](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/7fbe14b7-6e74-4aed-9db4-1b3e362618ca)

![image](https://github.com/user-attachments/assets/74e9607e-be35-4da8-8d15-162895f0692c)


> [!NOTE]
> For some battery chemistries (LFP especially), rescaling SOC% prevents the battery from top-balancing properly. For these chemistries it is recommended only to rescale the bottom section (e.g. using 20-100%) 

> [!TIP]
> Starting from software 8.10.0, it is now possible to do [negative rescaling](https://github.com/dalathegreat/Battery-Emulator/pull/1040)

#### Battery charge/discharge limit
- Max charge speed (A)
- Max discharge speed (A)

This setting artifically limits the amount of power that can go in/out of the battery. Even though most EV packs can push out hundreds of ampere, most inverters cannot handle large amounts of current. Some inverters even stop functioning incase they see a large value allowed. By default this is set to 30A on charge and discharge. You can decrease this value if the wiring or any fuses in your system cannot handle the amount of power. You can also increase this value if you have a high power inverter that becomes limited by the setting.

#### Manual charge voltage limits
Disabled by default. This option can be enabled to manually limit min/max voltage in the system. Note that not all inverters support voltage based limits, the setting was primarily developed for BYD_CAN. If left disabled, the system will automatically use the entire voltage range of your battery (unless Rescale SOC% is enabled)

![image](https://github.com/user-attachments/assets/16184922-ff83-4cc4-9705-68422cc695c4)


## Cellmonitor
Via this page you can keep track of all the cells in your battery. At the top of the page there is a quick readout of Min/Max/Deviation inside the battery. The view also has a grid view of all cells and their values, along with a graph at the bottom for quick visualization on how balanced the battery is. The two cells that are lowest and highest will be highlighted red for quicker identification where they are.

![image](https://github.com/dalathegreat/Battery-Emulator/assets/26695010/b03a0865-08e4-47f9-9472-1602ac02cf64)

### Interpreting the values
In general, the lower the voltage deviation in mV, the better. A battery with 10mV deviation is considerably healthier than one with 100mV deviation. Individual cells that are lower than the rest can be a sign of early stages of cellfailures/degradation/overheating, however, this depends heavily on the chemistry of the battery. Some chemistries like LMO can have way larger deviations at lower SOC% compared to NCM chemistries. 

Deviations can also grow under heavy load. If you pull tens of kW out of the battery, the mV deviation usually increases. This is completely normal.

The system will automatically go into a warning state incase a cellvoltage goes too high or too low. If this happens, an Event will be raised (see the event page), and further charging/discharging will be halted. 

### Balancing status
On some battery types (Nissan LEAF, Renault Zoe Gen2, more), we visualize the balancing status that the BMS sends in the graph view. You will see cyan colored bars for cells that balance, along with a text saying BALANCING when you hover over the cell.

<img width="954" height="312" alt="image" src="https://github.com/user-attachments/assets/196174c1-c83a-46a0-b52e-88b4b9e2f0a3" />


## Perform OTA Update
Via this page you can update the software. [See the page OTA Update for more info how](https://github.com/dalathegreat/Battery-Emulator/wiki/OTA-Update)

## Reboot Emulator
This button will restart the emulator. Can be useful to get out of a latched error message blocking operation (critical cell condition, etc.). Pressing the button will prompt you, "Are you sure you want to reboot?"

> [!Note]  
> Rebooting the Emulator might open contactors! If you have configured the hardware to control contactors via GPIO (see [Automatic Contactor Control](https://github.com/dalathegreat/Battery-Emulator/wiki/Contactor-Control-via-GPIO-pins)), they will absolutely open during a reboot! CAN controlled contactors have undefined behavior during reboot

> [!Note]  
> Rebooting the Emulator might put your inverter in a fault state. Some inverters take the reboot without any issues (Fronius Gen24), but others can properly lock themselves (SMA Tripower), and require a reset on the inverter side to get going again. 

## CAN logging
See the page about [CAN logging](https://github.com/dalathegreat/Battery-Emulator/wiki/CAN-logging) for more information about the CAN logging function.

## Web Server Authentication
This protection level is not particularly robust (Digest access authentication), however, it is sufficient to prevent non-malicious usage within the internal network on which it operates and the username and password are not sent in clear text. 
To force web server authentication, you need to configure webserver_auth to true in `webserver.cpp` and specify http_username and http_password in `webserver.cpp`

## Troubleshooting WiFi Connection / Back to factory settings on Lilygo
The following can help with instability with WiFi or slow performance found on newer boards of lilygo V1.1 boards from 2024-04-25, you might try to follow next steps to flash V1.0.0 version of original firmware, it is also bringing back to factory settings of lilygo so if you checked by mistake "Erase All Flash Before Sketch Upload" to enabled and your lilygo will be half bricked (wifi no connection at all, some strange errors in BE) you can back to factory and then again flash fresh BE. To do that follow next steps.

1. Download ESP32 Flash download tool https://dl.espressif.com/public/flash_download_tool.zip
2. Download V1.0.0 firmware https://github.com/Xinyuan-LilyGO/T-CAN485/tree/arduino-esp32-libs_v3.0.1/firmware
3. Run ESP32 Flash download Tool and set it like on picture (ESP32, Develop) and click OK

![image](https://github.com/user-attachments/assets/2e3ce09d-57f2-4c74-b518-d6e8064ccb0e)

4. Load V1.0.0 firmware like on picture and write 0x00 next to it, then select COM of your lilygo (arduino IDE need to be closed) and then click START

![image](https://github.com/user-attachments/assets/978091b5-4b57-40d5-a914-f7f910c93774)

5. Wait few seconds until Finish appear and Volia - your lilygo is like fresh new delivery and 100% working firmware!

