> [!CAUTION]
> Working with high voltage is dangerous. Always follow local laws and regulations regarding high voltage work. If you are unsure about the rules in your country, consult a licensed electrician for more information.

## Software setup

<img width="512" height="156" alt="image" src="https://github.com/user-attachments/assets/6bf9ce4d-02e8-4d38-9c13-a7c98cb11348" />

Set the software to use the Think city battery, and either use Molten salt or NCM depending on what battery you have

## Notes on isolation monitoring
The isolation monitoring circuit inside the Think battery is very sensitive. When connecting it to a solar inverter, the inverters own isolation detection might interfere with the Think's, and cause contactor opening. One user reported the following:

After about 3-4 minutes the battery BMS switches off its internal contactors with a service code in message ID 610h: "Isolation Fault With Contactors Off"

_To use the Think-Battery in stationary storage, the BMS needs to be isolated to prevent the "Isolation Fault" I suggest isolating the entire battery and using a separate power supply for the BMS. My Think battery is standing on wooden slats and cannot be hung on the wall. Therefore, I disconnected the grounding conductor and measured the insulation between the battery box and the conductor; the result - very high resistance (by disconnected power supply). In the next step I used a separate (small) power supply for the Battery Emulator..._
_(instead of make a "floating BMS" I made a "floating Battery"). Since then, my Think-Battery / SMA SBS2.5 system has been running continuously all day._


![Image](https://github.com/user-attachments/assets/85dc2e0f-93ee-4d2f-bf90-f8ab071dd5f9)

![Image](https://github.com/user-attachments/assets/dbd7d93e-5abd-4cc1-b25d-f64ba3e5f745)

> [!CAUTION]
> The battery box is not grounded! For battery function testing only! Or for complete and safe isolation of the battery box to prevent contact by people and animals.
![Data_ENER1-A306_Think-Battery](https://github.com/user-attachments/assets/1e012f9c-c407-4eec-9195-7cf2f696f496)
![ENER1-A306_Batt](https://github.com/user-attachments/assets/258fef6e-6548-48d4-8cfe-80058b58c99e)


