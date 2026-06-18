### What is OTA?
Over-the-Air (OTA) update is a mechanism that allows firmware updates to be deployed to ESP32 devices wirelessly, eliminating the need for physical connections. The Battery-Emulator uses the library Elegant OTA to achieve this.

### Prerequisites
Before being able to use OTA update, ensure you have the following prerequisites:
* A Wifi connection established to the board
   * Either a direct connection to the board (192.168.4.1 IP after connecting to the BatteryEmulator network (default password 123456789)
   * OR a connection via a router (see [quickstart video](https://youtu.be/sR3t7j0R9Z0) for how to connect Battery-Emulator hardware directly to your home network)

> [!TIP]
> Starting from version 7.0.0 , many settings are stored to persistent memory. This means that all the things you configure in Webserver (Wifi settings, max charge/discharge rate, SOC scaling settings, Battery capacity) don't have to be configured again. The system will use the previously set settings automatically!

### Getting the updated file
You can download the latest release from [Github Releases](https://github.com/dalathegreat/Battery-Emulator/releases) section

After opening the release you want to update to, at the bottom of the page select the .bin file that matches your hardware

<img alt="image" src="https://github.com/user-attachments/assets/b5c3bd15-cd7a-45e8-9e1a-f065d86d1e55" />


### Performing the OTA update
* Start by navigating to the web address (Note that IP will be different compared to direct / router connection)
* At the bottom of the page, click the "Perform OTA update"

<img alt="image" src="https://github.com/user-attachments/assets/0e2ab64b-4176-4ca2-8b09-7fe832fba59c" />

* On the ElegantOTA page, select the "Select file" option

![image](https://github.com/user-attachments/assets/cb8a72f1-48f5-46a6-b3b7-58abacc8ba14)

* Select the `.bin` file that you want to update to

* Flashing will commence, once completed you will see this message:

![bild](https://github.com/dalathegreat/BYD-Battery-Emulator-For-Gen24/assets/26695010/15be4272-7666-45dc-88a2-1881f17436f1)

* Congratulations, you have now updated the firmware remotely over the air! 🥳 
