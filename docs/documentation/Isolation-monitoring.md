> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

# What is isolation monitoring?

## Isolation monitoring inside an EV battery
All EVs have some form of isolation monitoring. The vehicle/battery constantly checks the isolation resistance between the HV+ to chassis ground, along with HV- to chassis ground. If a high voltage leak is detected, the vehicle will set a fault code, and if the leak is severe it will halt the operation of the vehicle.

See this excellent Youtube video explaining isolation resistance on EV side: https://www.youtube.com/watch?v=00eEj_EgMas

## Isolation monitoring on solar inverter side
All solar inverters perform isolation monitoring. The inverter keeps track of PV panel strings, and if these leak high voltage DC to protective earth, error codes are set. The solar inverter also keeps track of HV Battery, and if either HV+ or HV- is leaking towards protective earth, error codes are set and operation might be halted.

## Isolation monitoring in hybrid inverters
Hybrid inverters usually have two different types of isolation monitoring:

1. Active pre-check isolation monitoring

   Before the inverter connects the battery to the grid, it checks for leakage between the HV terminals and the grounded battery case, by creating a known-impedance current path from one terminal to ground and measuring the effect on the other, and repeating for the other terminal. If no leakage is detected, the inverter will allow the startup to continue.

2. Passive residual-current monitoring

   Once the inverter is in use, the inverter checks for leakage by measuring the 'residual current' (the difference between the currents in both battery terminals), to detect if any current is leaking to ground.

## How inverters affect the battery isolation monitoring
Most hybrid inverters are 'transformerless', meaning they do not have galvanic isolation between the battery HV connections and the grid live/neutral. When an EV battery is connected and in use, a current path is created from the HV connections, through the inverter to grid neutral, through the neutral-PE tie, and back through PE to the battery case. This appears to the battery as a low (and very noisy) isolation resistance.

This is readily detected by the battery as an isolation failure. Despite this, most EV batteries will still allow the contactors to remain closed:

- Some batteries only perform isolation measurement at start up, before the inverter has connected, and don't check again.

- Some batteries see the isolation failure, but allow the battery to continue to be used (so the driver can safely get off the highway).

- Some batteries have more sophisticated isolation monitoring using DSP that can distinguish between the transformerless inverter leakage and a real isolation fault.

## How batteries affect the inverter isolation monitoring

EV batteries do not normally cause problems with the inverter's isolation monitoring. The battery's test currents are usually low enough to not be detected as leakage by the inverter, although an exceptionally sensitive inverter might still detect them.

Some batteries do have Y-capacitors between the HV terminals and ground, for interference suppression (and sometimes also as part of the isolation measurement circuit). These will leak some AC to ground, which may cause a leakage current high enough for the inverter to detect (or even to trip an RCD, if the capacitors are large enough).

## Battery/inverter compatibility
In stationary use, the battery's own isolation monitoring is unnecessary (and often counterproductive). The inverter, plus the earth bonding which grounds the battery case, should ensure safety and detect any true isolation failures.

It is hard to be sure whether a given battery/inverter combination will have problems with isolation monitoring, without testing it, as there are many variables - the type of isolation testing each uses, the current thresholds that cause trips, the timing of when these tests are performed, and the response if isolation failures are detected.

If the battery or inverter detects an isolation failure and decides to open the contactors under load, this is bad as it will damage contactors.

### Contactor opening issues
Telltale signs of an issue might be that the battery runs fine for a few seconds/minutes, but then instantly opens contactors. This has been noticed on many EV platforms, for instance the Stellantis ECMP is notorious for opening contactors if battery detects leakage.

To get around this issue, users have experimented with disabling the isolation monitoring on the battery side, either via software mod or hardware mod.

#### Disabling isolation monitoring via software
It is very rare to be able to do this, but for instance on the "Stellantis CMP Smart" platform, we are able to via CAN command the battery to not perform any type of insulation testing. This avoids any issues when using the battery in stationary storage mode. No other EV platform has been cracked as well as this one!

#### Disabling isolation monitoring via hardware
Another way to get around this issue is to break the battery BMS way of performing isolation monitoring. This can involve isolating the BMS from the ground plane, clever connection of PE wiring, using galvanically isolated CAN wiring setups etc. See each batteries Wiki page for more info on any potential workarounds needed. [Example of ECMP platform, disabling isolation check via HW mod](https://github.com/dalathegreat/Battery-Emulator/wiki/Battery:-Stellantis-eCMP-(Citroen,-DS,-Opel,-Peugeot)#disabling-isolation-monitoring-via-hw-modification)