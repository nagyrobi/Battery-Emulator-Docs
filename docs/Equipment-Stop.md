### What is Equipment Stop?

<table >
  <tr>
    <td style="text-align: center;" width="175">
      <img width="165" height="126" src="https://github.com/user-attachments/assets/ce241f65-9ae7-435b-b516-cdae80f5e34f"/>
    </td>
    <td style="padding-left: 20px;">
      <p"><b>Equipment Stop</b>, is a critical safety feature implemented in various systems, including the battery emulator project. Its primary purpose is to halt all operations immediately in case of an emergency or potentially hazardous situation.
The <b>Equipment Stop</b> feature is designed to enhance safety by immediately halting all operations. When activated, it opens battery contactors controlled by the emulator, interrupting high-voltage connections to prevent potential hazards. Additionally it signals pause, effectively ceasing any ongoing charging or discharging processes by setting the maximum charge and discharge values to zero.</p>
    </td>
  </tr>
</table>

> [!CAUTION]
> Equipment stop should not be confused with Emergency stop. Equipment performs a graceful stop (which can fail), while an emergency stop button cuts power to the system. If you need more safety than an equipment stop button can offer, consider wiring an emergency stop button instead

[IP67 1NO1NC Stop Switch](https://vi.aliexpress.com/item/1005008119829541.html)

[Resistor kit](https://vi.aliexpress.com/item/1005006699173023.html)

### How (hardware)
Equipment stop can be added via a Normally Closed (NC) latching switch. This is how most equipment/emergency stop buttons work.

#### LilyGo board
Connect the Equipment stop button to the GPIO pins on the LilyGo

- GPIO Pin35 - Switch NC
- Any VDD pin - Switch NC

> [!NOTE]  
> There are not many free GPIO pins on the LilyGo board. The Pin35 might already be in use if you use some other optional functionality. If a collision happens, you can check the `Battery-Emulator/Software/src/devboard/hal/hw_lilygo.h` for free pins and remap

#### Stark CMR

If you're using Stark CMR v2 you do not need resistors as it has an isolated signal input which can be used with 3.3VDC, 5VDC or 12VDC. 5VDC is preferred. <br>
Connect 5VDC to one side of the NC E-stop and the other side of the E-stop to SIG IN. Then connect SIG GND to GND.

_Stark CMR v1 does not have a dedicated Signal input, use the resistor method as with the lilygo._<br>
_- GPIO Pin2 - Switch NC_<br>
_- VDC pin - Switch NC_<br>

<b>External Pull Resistors for Equipment Stop Button</b>

<table >
  <tr>
    <td style="text-align: center;" width="175">
      <img src="https://github.com/user-attachments/assets/4d372469-8f85-4732-970b-94dc68f77818"/>
    </td>
    <td style="padding-left: 20px;">
      <p>In our system, we use external pull resistors to ensure stable and reliable readings from the equipment stop button. Specifically, we use a 2kΩ resistor between the switch and VCC (3.3V) and a 10kΩ resistor between the GPIO pin and GND. This configuration creates a pull-down circuit, which stabilizes the signal and prevents floating values when the button is not pressed.</p>
<br/>
<ul>
  <li>The <b>2kΩ resistor</b> ensures that when the button is in its <b>normal (released) state</b> (NC circuit closed), the GPIO pin is pulled to a reliable high signal (3.3V) without excessive current flow.</li>
  <li>The <b>10kΩ pull-down resistor</b> ensures that when the button is <b>pressed</b> (NC circuit open), the GPIO pin is pulled to ground, preventing it from floating and avoiding false triggers.</li>
</ul>
<br/>
<b>Important:</b> Users must add these external resistors to their setup to ensure proper functionality of the button. Without these resistors, the GPIO pin may experience unstable readings or false triggers.
    </td>
  </tr>
</table>

### How (software)

- **Step 1**: Enable EQUIPMENT_STOP_BUTTON in USER_SETTINGS.h

To enable the Equipment Stop Button functionality, ensure the following line is uncommented in your USER_SETTINGS.h file:

![image](https://github.com/user-attachments/assets/b0a7eb2d-6206-442e-996c-24efd7b0f792)

This activates the functionality for an external button to stop the equipment.

- **Step 2**: Set the Switch Behavior in USER_SETTINGS.cpp

Next, configure the switch behavior in USER_SETTINGS.cpp. You need to set the STOP_BUTTON_BEHAVIOR variable to either LATCHING_SWITCH or MOMENTARY_SWITCH, depending on your desired button action.

![image](https://github.com/user-attachments/assets/57c18a8f-03e9-44b4-9801-8972c5f19c37)

LATCHING_SWITCH: A normally closed (NC) latching switch. When the switch is pressed, it activates the equipment stop and keeps the stop active as long as the physical switch remains in the pressed position. The stop will only deactivate when the switch is manually returned to its normal (closed) state.

MOMENTARY_SWITCH: A normally closed (NC) push switch. A short press activates the equipment stop. To deactivate, press and hold the button for 15 seconds. This mode keeps the stop state persistent between reboots.


<b>Internal Debounce Module</b>

We have also implemented an internal software debounce module for handling button inputs more reliably, preventing false triggers caused by mechanical noise. 

### Why

The **Equipment Stop** feature was developed to enhance the overall safety and control of the equipment. It provides users with a reliable method to **halt operations** when needed, ensuring that the system can be effectively managed during both routine use and unexpected situations.

This feature is essential for maintaining operational integrity, as it opens the battery contactors controlled by the emulator, interrupting high-voltage connections and mitigating potential hazards. Additionally, it signals a pause in power usage by setting both the maximum and minimum charge values to zero, thereby preventing any unintended operation of the equipment.

Overall, the development of the **Equipment Stop** functionality reflects our commitment to providing users with a comprehensive safety mechanism that promotes responsible equipment management and ensures the reliability of the system.