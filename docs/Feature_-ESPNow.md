## Some ESPNow background 

ESP-NOW is a low-power, low-latency wireless protocol by Espressif that allows direct device-to-device communication without a router. It works on the data-link layer, bypassing higher OSI layers, which results in fast response times and minimal overhead. It supports ESP8266, ESP32, ESP32-S, and ESP32-C series chips and can coexist with Wi-Fi and Bluetooth LE.
It’s ideal for smart home devices, remote controls, and sensor networks, supporting one-to-one, one-to-many, and many-to-many communication with payloads up to 250 bytes.

### **Key Features:**
* Low latency (millisecond-level delay)
* Ultra-low power consumption
* No gateway required
* Encrypted or unencrypted communication
* Range up to ~220 meters in open space
* Supports callbacks for send/receive events

## **ESPNow in Battery Emulator context**
Any ESP32 nearby devices could be used to display the Battery Emulator data without needing any physical connection with the Battery Emulator.
The example below is printing some battery information in the ESP console but the implementation could be extended to use External Displays.
The battery data is broadcasted by using the ESPNow message format to all the ESP32 nearby devices supporting ESPNow V1 (250 bytes)
Due to the size limitation of the ESPNow V1 message , the Battery data was split into 4 different categories/structure:
* battery info - contains the battery general information, capacity, etc
* battery status - contains the current status of the battery 
* battery cell status - contains the current cell voltages 
* battery balancing status - contains the current balancing status of each cell  

###  **Example of implementation**
Contents of **battery.h** file:
```C
  #include <Arduino.h>

  #define MAX_AMOUNT_CELLS 192

  enum bms_status_enum { STANDBY = 0, INACTIVE = 1, DARKSTART = 2, ACTIVE = 3, FAULT = 4, UPDATING = 5 };
  enum real_bms_status_enum { BMS_DISCONNECTED = 0, BMS_STANDBY = 1, BMS_ACTIVE = 2, BMS_FAULT = 3 };
  enum balancing_status_enum {
    BALANCING_STATUS_UNKNOWN = 0,
    BALANCING_STATUS_ERROR = 1,
    BALANCING_STATUS_READY = 2,  //No balancing active, system supports balancing
    BALANCING_STATUS_ACTIVE = 3  //Balancing active!
  };
  enum battery_chemistry_enum { Autodetect = 0, NCA = 1, NMC = 2, LFP = 3, ZEBRA = 4, Highest };

  enum class comm_interface {
    Modbus = 1,
    RS485 = 2,
    CanNative = 3,
    CanFdNative = 4,
    CanAddonMcp2515 = 5,
    CanFdAddonMcp2518 = 6,
    Highest
  };

  enum led_mode_enum { CLASSIC, FLOW, HEARTBEAT };
  enum PrechargeState {
    AUTO_PRECHARGE_IDLE,
    AUTO_PRECHARGE_START,
    AUTO_PRECHARGE_PRECHARGING,
    AUTO_PRECHARGE_OFF,
    AUTO_PRECHARGE_COMPLETED,
    AUTO_PRECHARGE_FAILURE
  };
  enum BMSResetState {
    BMS_RESET_IDLE = 0,
    BMS_RESET_WAITING_FOR_PAUSE,
    BMS_RESET_POWERED_OFF,
    BMS_RESET_POWERING_ON,
  };

  #define DISCHARGING 1
  #define CHARGING 2

  #define CAN_STILL_ALIVE 60
  // Set by battery each time we get a CAN message. Decrements every second. When reaching 0, sets event

  enum CAN_Interface {
    // Native CAN port on the LilyGo & Stark hardware
    CAN_NATIVE = 0,
    // Native CANFD port on the Stark CMR hardware
    CANFD_NATIVE = 1,
    // Add-on CAN MCP2515 connected to GPIO pins
    CAN_ADDON_MCP2515 = 2,
    // Add-on CAN-FD MCP2518 connected to GPIO pins
    CANFD_ADDON_MCP2518 = 3,
    // No CAN interface
    NO_CAN_INTERFACE = 4
  };

  struct BATTERY_INFO_TYPE {
    /** uint32_t */
    /** Total energy capacity in Watt-hours 
     * Automatically updates depending on battery integration OR from settings page
    */
    uint32_t total_capacity_Wh = 30000;
    uint32_t reported_total_capacity_Wh = 30000;

    /** uint16_t */
    /** The maximum intended packvoltage, in deciVolt. 4900 = 490.0 V */
    uint16_t max_design_voltage_dV = 5000;
    /** The minimum intended packvoltage, in deciVolt. 3300 = 330.0 V */
    uint16_t min_design_voltage_dV = 2500;
    /** The maximum cellvoltage before shutting down, in milliVolt. 4300 = 4.300 V */
    uint16_t max_cell_voltage_mV = 4300;
    /** The minimum cellvoltage before shutting down, in milliVolt. 2700 = 2.700 V */
    uint16_t min_cell_voltage_mV = 2700;
    /** The maxumum allowed deviation between cells, in milliVolt. 500 = 0.500 V */
    uint16_t max_cell_voltage_deviation_mV = 500;

    /** uint8_t */
    /** Total number of cells in the pack */
    uint8_t number_of_cells;

    /** Other */
    /** Chemistry of the pack. Autodetect, or force specific chemistry */
    battery_chemistry_enum chemistry = battery_chemistry_enum::NCA;
  };  // 24 bytes

  struct BATTERY_STATUS_TYPE {
    /** uint32_t */
    /** Remaining energy capacity in Watt-hours */
    uint32_t remaining_capacity_Wh = 0;
    /** The remaining capacity reported to the inverter based on min percentage setting, in Watt-hours 
     * This value will either be scaled or not scaled depending on the value of
     * battery.settings.soc_scaling_active
     */
    uint32_t reported_remaining_capacity_Wh;
    /** Maximum allowed battery discharge power in Watts. Set by battery */
    uint32_t max_discharge_power_W = 0;
    /** Maximum allowed battery charge power in Watts. Set by battery */
    uint32_t max_charge_power_W = 0;
    /* Some early integrations do not support reading allowed charge power from battery
    On these integrations we need to have the user specify what limits the battery can take */
    /** Overriden allowed battery discharge power in Watts. Set by user */
    uint32_t override_discharge_power_W = 0;
    /** Overriden allowed battery charge power in Watts. Set by user */
    uint32_t override_charge_power_W = 0;

    /** int32_t */
    /** Instantaneous battery power in Watts. Calculated based on voltage_dV and current_dA */
    /* Positive value = Battery Charging */
    /* Negative value = Battery Discharging */
    int32_t active_power_W = 0;
    int32_t total_charged_battery_Wh = 0;
    int32_t total_discharged_battery_Wh = 0;

    /** uint16_t */
    /** Maximum allowed battery discharge current in dA. Calculated based on allowed W and Voltage */
    uint16_t max_discharge_current_dA = 0;
    /** Maximum allowed battery charge current in dA. Calculated based on allowed W and Voltage  */
    uint16_t max_charge_current_dA = 0;
    /** State of health in integer-percent x 100. 9900 = 99.00% */
    uint16_t soh_pptt = 9900;
    /** Instantaneous battery voltage in deciVolts. 3700 = 370.0 V */
    uint16_t voltage_dV = 3700;
    /** Maximum cell voltage currently measured in the pack, in mV */
    uint16_t cell_max_voltage_mV = 3700;
    /** Minimum cell voltage currently measured in the pack, in mV */
    uint16_t cell_min_voltage_mV = 3700;
    /** The "real" SOC reported from the battery, in integer-percent x 100. 9550 = 95.50% */
    uint16_t real_soc;
    /** The SOC reported to the inverter, in integer-percent x 100. 9550 = 95.50%.
     * This value will either be scaled or not scaled depending on the value of
     * battery.settings.soc_scaling_active
     */
    uint16_t reported_soc;
    /** A counter that increases incase a CAN CRC read error occurs */
    uint16_t CAN_error_counter;

    /** int16_t */
    /** Maximum temperature currently measured in the pack, in d°C. 150 = 15.0 °C */
    int16_t temperature_max_dC;
    /** Minimum temperature currently measured in the pack, in d°C. 150 = 15.0 °C */
    int16_t temperature_min_dC;
    /** Instantaneous battery current in deciAmpere. 95 = 9.5 A */
    int16_t current_dA;
    /** Instantaneous battery current in deciAmpere. Sum of all batteries in the system 95 = 9.5 A */
    int16_t reported_current_dA;

    /** uint8_t */
    /** A counter set each time a new message comes from battery.
     * This value then gets decremented every second. Incase we reach 0
     * we report the battery as missing entirely on the CAN bus.
     */
    uint8_t CAN_battery_still_alive = CAN_STILL_ALIVE;
    /** The current system status, which for now still has the name bms_status */
    bms_status_enum bms_status = ACTIVE;
    /** The current battery status, which for now has the name real_bms_status */
    real_bms_status_enum real_bms_status = BMS_DISCONNECTED;
    /** LED mode, customizable by user */
    led_mode_enum led_mode = CLASSIC;
    /** Balancing status */
    balancing_status_enum balancing_status = BALANCING_STATUS_UNKNOWN;
  };  // 80 bytes

  struct BATTERY_BALANCING_STATUS_TYPE {
    /** All balancing resistors status inside the pack, either on(1) or off(0).
     * Use with battery.info.number_of_cells to get valid data.
     * Not available for all battery manufacturers.
     */
    bool cell_balancing_status[MAX_AMOUNT_CELLS];

    /** uint8_t */
    /** Total number of cells in the pack */
    uint8_t number_of_cells;

  };  // 193 bytes

  struct BATTERY_CELL_STATUS_TYPE {

    /** All cell voltages currently measured in the pack, in mV. 212 * 20 = 4240 mV
     * Use with battery.info.number_of_cells to get valid data.
     */
    uint8_t cell_voltages_mV[MAX_AMOUNT_CELLS];

    /** uint8_t */
    /** Total number of cells in the pack */
    uint8_t number_of_cells;
  };  // 193 bytes

  enum espnow_message_enum { BAT_INFO = 1, BAT_STATUS = 2, BAT_BALANCE = 3, BAT_CELL_STATUS = 4 };

  struct ESPNOW_BATTERY_MESSAGE {
    uint16_t emulator_id;
    uint8_t battery_id;
    uint8_t esp_message_type;
    uint8_t esp_message[];
  } __packed;
```
Contents of **BE_ESPNow_Console.ino** file:
```C
/*
 * Example for using ESPNow feature on Battery Emulator
 */
#include <Arduino.h>
#include "pin_config.h"
#include "WiFi.h"
#include <esp_now.h>
#include "battery.h"

// ---------------- BATTERY DATA DEFAULT VALUES ----------------
int pvPower = 1200;
int chargePower = 1200;
int dischargePower = 800;
int loadPower = -200;
int cellMin = 0;
int cellMax = 0;
int remainingCap = 0;
int batterySOC = 20;
int chargeCurrent = 10;
int dischargeCurrent = 10;
float batterySOH = 100.0;
int batteryVoltage = 375;
int batteryCurrent = 10;
int totalCharged = 1200;
int totalDischarged = 1187;
int tempMax = 40;
int tempMin = -10;

uint8_t cellVoltages[192] = {
  185, 186, 184, 187,
  185, 186, 185, 184,
  186, 185, 187, 185,
  186, 185, 184, 185,
  185, 186, 184, 187,
  185, 186, 185, 184,
  186, 185, 187, 185,
  186, 185, 184, 185,
  185, 186, 184, 187,
  185, 186, 185, 184,
  186, 185, 187, 185,
  186, 185, 184, 185,
  185, 186, 184, 187,
  185, 186, 185, 184,
  186, 185, 187, 185,
  186, 185, 184, 185,
  185, 186, 184, 187,
  185, 186, 185, 184,
  186, 185, 187, 185,
  186, 185, 184, 185,
  185, 186, 184, 187,
  185, 186, 185, 184,
  186, 185, 187, 185,
  186, 185, 184, 185,
};
uint8_t cellNo = 32;

struct BATTERY_STATUS_TYPE b_status;
struct BATTERY_INFO_TYPE b_info;
struct BATTERY_BALANCING_STATUS_TYPE b_ballancing;
struct BATTERY_CELL_STATUS_TYPE b_cell_status;
struct ESPNOW_BATTERY_MESSAGE b_message;

void OnDataRecv(const uint8_t * mac, const uint8_t *incomingData, int len) {
  memcpy(&b_message, incomingData, len);
  if(b_message.esp_message_type == BAT_INFO) {
    memcpy(&b_info, b_message.esp_message, sizeof(b_info));
    cellNo = b_info.number_of_cells;
    Serial.print("Bytes received (INFO): ");
    Serial.println(len);

  } else if(b_message.esp_message_type == BAT_STATUS) {
    memcpy(&b_status, b_message.esp_message, sizeof(b_status));
    Serial.print("Bytes received (STATUS): ");
    Serial.println(len);
    batterySOC = b_status.real_soc/100;
    loadPower = b_status.active_power_W;
    chargePower = b_status.max_charge_power_W;
    dischargePower = b_status.max_discharge_power_W;
    cellMin = b_status.cell_min_voltage_mV;
    cellMax = b_status.cell_max_voltage_mV;
    remainingCap = b_status.remaining_capacity_Wh;

    chargeCurrent = b_status.max_charge_current_dA/10;
    dischargeCurrent = b_status.max_discharge_current_dA/10;
    batterySOH = b_status.soh_pptt/100;
    batteryVoltage = b_status.voltage_dV/10;
    batteryCurrent = b_status.current_dA/10;

    totalCharged = b_status.total_charged_battery_Wh/1000;
    totalDischarged = b_status.total_discharged_battery_Wh/1000;
    tempMax = b_status.temperature_max_dC;
    tempMin = b_status.temperature_min_dC;

  } else if(b_message.esp_message_type == BAT_BALANCE) {
    memcpy(&b_ballancing, b_message.esp_message, len - 4);
    Serial.print("Bytes received (BALANCE): ");
    Serial.println(len);

  } else if(b_message.esp_message_type == BAT_CELL_STATUS) {
    memcpy(&b_cell_status, b_message.esp_message, sizeof(b_cell_status));
    cellNo = b_cell_status.number_of_cells;
    memcpy(&cellVoltages, b_cell_status.cell_voltages_mV, min(sizeof(b_cell_status), sizeof(cellVoltages)));
    Serial.print("Bytes received (CELL): ");
    Serial.println(len);

  } else {
    Serial.print("Bytes received (UNKNOWN TYPE): ");
    Serial.println(len);
  }
}

// ---------------- DATA RENDERERS ----------------
void print_battery_data() {
  Serial.println("========BATTERY DATA=========");
  Serial.printf("Battery SOC          = %d %s\n", batterySOC, "%");
  Serial.printf("Max Charge           = %d %s\n", chargePower, "W");
  Serial.printf("Load                 = %d %s\n", loadPower, "W");
  Serial.printf("Cell Max Voltage     = %d %s\n", cellMax, "mV");
  Serial.printf("Max Discharge        = %d %s\n", dischargePower, "W");
  Serial.printf("Remaining Cap        = %d %s\n", remainingCap, "Wh");
  Serial.printf("Cell Min Voltage     = %d %s\n", cellMin, "mV");
  Serial.printf("Max Charge Current   = %d %s\n", chargeCurrent, "A");
  Serial.printf("Total Charged        = %d %s\n", totalCharged, "kWh");
  Serial.printf("Max Cell Temperature = %d %s\n", tempMax, "dC"); //°C
  Serial.printf("Max Discharge Curent = %d %s\n", dischargeCurrent, "A");
  Serial.printf("Total Discharged     = %d %s\n", totalDischarged, "kWh");
  Serial.printf("Min Cell Temperature = %d %s\n", tempMin, "dC"); //°C
  Serial.printf("Battery SOH          = %f %s\n", batterySOH, "%");
  Serial.printf("Battery Voltage      = %d %s\n", batteryVoltage, "V");
  Serial.printf("Current              = %d %s\n", batteryCurrent, "A");
  Serial.println("=============================");
}

// ---------------- SETUP ----------------
void setup() {

    Serial.begin(115200);
    Serial.println("Hello BE");
 
   // Set device as a Wi-Fi Station
    WiFi.mode(WIFI_STA);

    // Init ESPNow
    if (esp_now_init() != ESP_OK) {
        Serial.println("Error initializing ESPNow");
        return;
    }

    // Once ESPNow is successfully Init, we will register for recv CB to
    // get recv packer info
    esp_now_register_recv_cb(esp_now_recv_cb_t(OnDataRecv));

}

// ---------------- LOOP ----------------
void loop() {
  delay(2000);
  print_battery_data();
}

```
Example of output:
```
Bytes received (INFO): 28
Bytes received (STATUS): 84
Bytes received (CELL): 197
Bytes received (BALANCE): 197
========BATTERY DATA=========
Battery SOC          = 50 %
Max Charge           = 5000 W
Load                 = 0 W
Cell Max Voltage     = 3596 mV
Max Discharge        = 5000 W
Remaining Cap        = 15000 Wh
Cell Min Voltage     = 3500 mV
Max Charge Current   = 13 A
Total Charged        = 0 kWh
Max Cell Temperature = 60 dC
Max Discharge Curent = 13 A
Total Discharged     = 0 kWh
Min Cell Temperature = 50 dC
Battery SOH          = 99.00 %
Battery Voltage      = 370 V
Current              = 0 A
=============================
```
