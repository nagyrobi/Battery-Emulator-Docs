## Why is Period Reset needed?
Some EV batteries are not able to operate 24/7 nonstop under all conditions. Over time the SOC% will become less and less accurate, and in some integrations like the Nissan LEAF, even the GIDS (Wh remaining) becomes confused (see [Issue 86](https://github.com/dalathegreat/Battery-Emulator/issues/86)). Balancing also becomes a problem on some packs.

### Batteries that benefit from Periodic Reset

- Nissan LEAF

## Taking it into use

<img width="448" height="39" alt="image" src="https://github.com/user-attachments/assets/85f72732-a5b4-4b2e-b6b4-8ee18ca57609" />

The solution is to periodically reset the 12V power going into the BMS. This can be automated by using the `Periodic BMS reset every 24h` option. When this is enabled, a GPIO pin (See the HAL file for your hardware to see which pin), will be toggled ON upon start, and after 24 hours it will turn off for 30seconds. When the power is cut to this pin, the emulator is put into Paused state, meaning no CAN messages will be sent towards the battery, and the charge/discharge limits will go to 0W. After the 30 seconds have passed, power is turned back on to the pin, and the emulator is unpaused. This 3.3V pin can be used to control the 12V power going into the BMS via an SSR or other type of relay.

This makes it possible to use the problematic BMS (like Nissan LEAF batteries) continuously, without any worry of SOC% drifting overtime.

The day starts when Battery-Emulator is powered on. So after 24 hours of uptime, the reset will be triggered.

By default the system will power off for 30 seconds during the daily reboots. This time can be tweaked in the Webserver settings if you want the reset to be shorter or longer. Some batteries are OK with as short resets as 2 seconds. Here is the setting:

<img width="363" height="95" alt="image" src="https://github.com/user-attachments/assets/d5731ec2-c1e2-4dcb-9758-16374e5fc86a" />
