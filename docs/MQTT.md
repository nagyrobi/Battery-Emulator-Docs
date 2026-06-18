# Introduction
The Battery-Emulator has MQTT support. It can be configured to publish data to your MQTT broker and then be used in whatever way you choose. You can modify the battery emulator code to publish more data, less data, or other data, as well as subscribing to MQTT topics. There are also advanced functions that can be triggered via MQTT, such as charge limits or balancing commands.

Main purpose for this implementation is better integration with popular home automation platforms such as Home Assistant, in order to for example keep track of battery temperature and cell deviation. If "MQTT" and "Home Assistant" are not familiar words, you will likely not benefit from this until you're up to speed on the current state of home automation.

- [Published data](#published-data)
  - [Custom publishing](#custom-publishing)
  - [Discovery](#discovery)
- [Subscriptions](#subscriptions)
  - [Usage](#usage)
- [Set Limits](#set_limits)
- [QoS and LWT](#QoS-and-LWT)
- [Future improvements](#future-improvements)
- [Inspiration](#inspiration)
- [Breaking Changes](#Breaking-Changes)
- [References](#references)

# Enabling MQTT in the software
To start using MQTT, Enable the checkbox "Enable MQTT" under the Connectivity settings in Webserver 

The server IP, username and password for MQTT can be set. Incase you want to use no login info, leave the fields empty.

<img alt="image" src="https://github.com/user-attachments/assets/c796de68-1783-4b03-b4fb-422065fb0290" />

**NOTE: Make sure that your LAN is not on 192.168.4.x since this causes issues with the builtin ESP accesspoint! **

# Published data
Out of the box, the MQTT implementation publishes to a ```BE/info``` topic that holds SOC, SOH, min/max temperature and min/max cell voltage. On select battery implementations that update cell voltage data, all cell voltages are published in a separate ```BE/spec_data``` topic. Battery emulator events are published to ```BE/events``` topic. By default, all data is published with the ```retain``` flag set, which simplifies having e.g. MQTT Sensors in Home Assistant.

```BE/info``` example payload (sensor configuration autodiscovery topic is avalilable for each value ):
```
{
  "SOC": 63.300,
  "StateOfHealth": 92.000,
  "temperature_min": 5.800,
  "temperature_max": 7.400,
  "cell_max_voltage": 3787,
  "cell_min_voltage": 3765
}
```

```BE/spec_data``` example payload (has a autodiscovery topic):
```
{
"cell_voltages": [3779, 3780, 3782, 3767, 3783, 3769, 3782, 3768, 3782, 3777, 3782, 3776, 3781, 3776, 3775, 3765, 3782, 3776, 3782, 3777, 3773, 3765, 3774, 3768, 3773, 3765, 3772, 3771, 3773, 3772, 3772, 3768, 3773, 3772, 3772, 3767, 3772, 3778, 3784, 3778, 3773, 3769, 3773, 3767, 3783, 3769, 3782, 3769, 3779, 3777, 3775, 3777, 3774, 3774, 3780, 3767, 3773, 3770, 3773, 3769, 3775, 3765, 3777, 3777, 3783, 3774, 3787, 3776, 3776, 3775, 3784, 3767, 3778, 3763, 3776, 3763, 3784, 3773, 3778, 3767, 3777, 3771, 3787, 3775, 3783, 3774, 3784, 3772, 3781, 3773, 3780, 3773, 3773, 3766, 3773, 3765]
}
```

```BE/events``` example payload (has a autodiscovery topic):
```
{
	"event_type": "RESET_SW",
	"severity": "INFO",
	"last_event": "9",
	"count": "1",
	"data": "3",
	"message": "Info: The board was reset via software, webserver or OTA. Normal operation",
	"milis": "10001"
}
```

## Discovery
If you want to implement discovery topics for your data, an example can be found in ```mqtt.cpp:publish_cell_data()```, in the block below ```if (mqtt_first_transmission == true)```. The example is a bit complex since it publishes discovery topics for a dynamic amount of cell voltages, but it should sink in after staring at it for a few minutes. The current discovery topic is hardcoded to ```homeassistant/...``` which should probably be configurable in the future.

homeassistant/sensor/BE/cell_voltage96/config example payload:
```
{
	"name": "Battery Cell Voltage 96",
	"object_id": "be_battery_voltage_cell96",
	"unique_id": "BE_battery_voltage_cell96",
	"device_class": "voltage",
	"state_class": "measurement",
	"state_topic": "BE/spec_data",
	"unit_of_measurement": "V",
	"enabled_by_default": true,
	"expire_after": 240,
	"value_template": "{{ value_json.cell_voltages[95] }}",
	"device": {
		"identifiers": [
			"battery-emulator"
		],
		"manufacturer": "DalaTech",
		"model": "BatteryEmulator",
		"name": "Battery Emulator"
	},
	"origin": {
		"name": "BatteryEmulator",
		"sw": "7.6.0",
		"url": "https://github.com/dalathegreat/Battery-Emulator"
	}
}
```
homeassistant/sensor/BE/SOC/config example payload:
```
{
	"name": "SOC (scaled)",
	"state_topic": "BE/info",
	"unique_id": "BE_SOC",
	"object_id": "be_SOC",
	"value_template": "{{ value_json.SOC }}",
	"unit_of_measurement": "%",
	"device_class": "battery",
	"state_class": "measurement",
	"enabled_by_default": true,
	"expire_after": 240,
	"device": {
		"identifiers": [
			"battery-emulator"
		],
		"manufacturer": "DalaTech",
		"model": "BatteryEmulator",
		"name": "Battery Emulator"
	},
	"origin": {
		"name": "BatteryEmulator",
		"sw": "7.6.0",
		"url": "https://github.com/dalathegreat/Battery-Emulator"
	}
}
```

## MQTT Sensor
All data plublished has a discovery topic so you dont have to configure anything on Home assistant. It's recommended your MQTT broker has persistence enabled to ensure the availability is retained when restarting HA.

You can use this for inspiration on how to implement your own MQTT data publishing and integrate it into Home Assistant without using discovery. Also, see the [References](#references) section.

# Subscriptions
The battery emulator subscribes to [topic_name]/command/+ which defaults to BE/command/+

The currently supported commands are
- BMSRESET - Triggers the BMS reset feature
- PAUSE - Triggers the pause feature
- RESUME - Resumes from the paused state
- RESTART - Triggers a restart of the battery emulator
- STOP - Triggers the stop feature of the battery emulator
- SET_LIMITS - Sets a temporary charge and/or discharge limit

- OPEN_CONTACTORS - Tesla specific CAN command 
- START_LFP_BALANCING - Tesla specific force charge above 99%
- STOP_LFP_BALANCING - Tesla specific STOP force charge above 99%
- TESLA_BMS_RESET - Tesla specific to reset the BMS via CAN. Useful to close contactors remotely

e.g. BE/command/BMSRESET

## SET_LIMITS

Sets a temporary charge and/or discharge limit for the period of [timeout] seconds which overrides any battery or manual limit in settings. Battery Emulator will use the lowest max_charge and max_discharge from the battery, manual setting or temporary setting.

Limits are set as deciAmpere i.e. 300 = 30.0 A

| Parameter | Data Type | Default |
| --------- | -------- | ------- |
| max_charge | number | | disable limit|
| max_discharge | number | disable limit |
| timeout | seconds | 30 |

Example payload (Max Charge 30A, Max Discharge 40A, Timeout 60 seconds):
```
{
  "max_charge": 300,
  "max_discharge": 400,
  "timeout": 60
}
```

Once the timeout expires it will revert back to unrestricted power. If you want to cancel your set limit quickly, you can send a new message with a short timeout, for instance 1 second.

Example of Webserver visualizing that a Remote MQTT limit is in use

<img width="490" height="363" alt="image" src="https://github.com/user-attachments/assets/51266daa-0288-4f66-8dbf-33bb6daeac51" />

# QoS and LWT
- Publishing defaults to QoS 0, you can override this by updating MQTT_QOS in USER_SETTINGS.h. If you set a QoS of 1 or higher the battery emulator could crash if the message queue overflows.
- Subscription is limited to QoS 1
- Last Will and Testament (LWT) is supported for Home Assistant.

# Inspiration
A fairly complex Home Assistant example that uses MQTT for cell voltages, SOC and temperature. Other graphs are based on a mix of MQTT, Fronius Modbus TCP and an internal Fronius API:

![HA_prt_sc](https://github.com/dalathegreat/Battery-Emulator/assets/81711263/f548ba20-a303-4d05-b805-cbd9a69956f2)

Another Battery-Emulator dashboard on Home Assistant:

![image](https://github.com/user-attachments/assets/b063ad70-56e9-4fd9-83a4-e091500ef5da)

# Breaking Changes

Changes in PRs [#566](https://github.com/dalathegreat/Battery-Emulator/pull/566) and [#567](https://github.com/dalathegreat/Battery-Emulator/pull/567) introduced on v7.6.0 have introduced updates to the MQTT topic naming conventions for Home Assistant (HA), sensor names, and the addition of a prefix to the cell information. These updates bring consistency across sensor object IDs but may break compatibility for users already using MQTT in HA.

For new installations, the default values are now set in usersettings.cpp as follows:

const char* mqtt_topic_name = "BE";

const char* mqtt_object_id_prefix = "be_";

const char* mqtt_device_name = "Battery Emulator";


To keep previous naming conventions and maintain backward compatibility, you can comment out the following line in user_settings.h:

#define MQTT_MANUAL_TOPIC_OBJECT_NAME

However, please note that the cell voltage sensor IDs will still change, as they now include the prefix to align with other sensors.

If you prefer to maintain the current sensor configurations in HA and avoid any updates, you can disable the autodiscovery feature by commenting out the following line in user_settings.h:

#define HA_AUTODISCOVERY

With HA_AUTODISCOVERY commented out, configuration topics will not be sent to HA, thereby preventing the creation of new sensors.

<b>Summary</b>
To retain the existing configuration from installations before v7.6.0 in upcoming updates, comment out the following lines:

#define MQTT_MANUAL_TOPIC_OBJECT_NAME

#define HA_AUTODISCOVERY

# References
- [Home Assistant MQTT overview](https://www.home-assistant.io/integrations/mqtt/)
  Covers brokers, discovery, various configuration and Home Assistant services related to MQTT

- [Home Assistant MQTT Sensor](https://www.home-assistant.io/integrations/sensor.mqtt/)
  Explains manual (non-discovery) setup of MQTT related sensors. In a way offers more control over details compared to using Discovery, but requires more work

- [PubSubClient](https://github.com/knolleary/pubsubclient)
  The library currently used for MQTT