# Home Assistant goodies

## Pause Charge/Discharge switch (instead of two buttons)

```yaml
template:
  - switch:
    - name: Pause Charge/Discharge
      default_entity_id: switch.battery_emulator_charge_discharge_paused
      icon: mdi:battery-remove
      state: "{{ is_state('sensor.be_pause_status', 'PAUSED') }}"
      turn_on:
        action: button.press
        target:
          entity_id: button.battery_emulator_pause_charge_discharge
      turn_off:
        action: button.press
        target:
          entity_id: button.battery_emulator_resume_charge_discharge
```

## Chart examples

### 2D cell monitor

<img width="386" height="312" alt="image" src="https://github.com/user-attachments/assets/ef9aebaf-bc9b-45c4-a8e4-7b9e799b7b2f" />

```yaml
type: custom:plotly-graph
raw_plotly_config: true
title: Battery 0
fn: |-
  $ex {
    vars.cellnum = 96; // Adjust the number of cells of your battery here
    vars.x = [];
    vars.y = [];
    vars.z = [];

    for (let j = 0; j < vars.cellnum; j++) {
      vars.z[j] = [];
      // Calculate the index in the 1D array corresponding to the 2D matrix element
      let index = j;

      // Populate the matrix with values from the 1D array
      vars.z[j][j] = hass.states["sensor.be_battery_voltage_cell" + (index+1).toString()].state
    }

    for (let j = 0; j < vars.cellnum; j++) {
      // Calculate the index in the 1D array corresponding to the 2D matrix element
      let index = j;
      vars.x[index] = index;
      // Populate the matrix with values from the 1D array

      vars.y[index] = hass.states["sensor.be_battery_voltage_cell" + (index+1).toString()].state
    }

    vars.ymin = Math.min(...vars.y) - (20.0 / 1000);
    vars.ymax = Math.max(...vars.y) + (20.0 / 1000);
  }
entities:
  - entity: ""
    color: "#ff0000"
    type: bar
    x: $ex vars.x
    "y": $ex vars.y
    z: $ex vars.z
    texttemplate: "%{y}"
    refresh_interval: 2
layout:
  xaxis:
    showgrid: false
    zeroline: false
    showticklabels: false
    ticks: false
    nticks: 1
    visible: false
    height: 410
  yaxis:
    range:
      - $ex vars.ymin
      - $ex vars.ymax
config:
  displayModeBar: false
  scrollZoom: false
```

### 3D Cell monitor (in relation time) for 96 cells

<img width="465" height="400" alt="image" src="https://github.com/user-attachments/assets/0474bd2d-6473-44b8-9312-9fac23bb1db5" />

```yaml
type: custom:plotly-graph
raw_plotly_config: true
hours_to_show: 1d
title: Battery 0
defaults:
  entity:
    internal: true
    filters:
      - resample: 1m
    fn: |
      $ex {
          vars.data.push(ys);
          vars.xs = xs;
       }
fn: $ex vars.data = []
layout:
  height: 400
  margin:
    t: 0
    l: 0
    r: 90
    b: 0
  scene:
    camera:
      eye:
        x: 2
        "y": 2
        z: 2
    xaxis:
      title: Hour of Day
      tickformat: "%H:%M:%S"
    yaxis:
      title: BMS
      tickmode: linear
      dtick: 1
    zaxis:
      title: V
entities:
  - entity: sensor.be_battery_voltage_cell1
  - entity: sensor.be_battery_voltage_cell2
  - entity: sensor.be_battery_voltage_cell3
  - entity: sensor.be_battery_voltage_cell4
  - entity: sensor.be_battery_voltage_cell5
  - entity: sensor.be_battery_voltage_cell6
  - entity: sensor.be_battery_voltage_cell7
  - entity: sensor.be_battery_voltage_cell8
  - entity: sensor.be_battery_voltage_cell9
  - entity: sensor.be_battery_voltage_cell10
  - entity: sensor.be_battery_voltage_cell11
  - entity: sensor.be_battery_voltage_cell12
  - entity: sensor.be_battery_voltage_cell13
  - entity: sensor.be_battery_voltage_cell14
  - entity: sensor.be_battery_voltage_cell15
  - entity: sensor.be_battery_voltage_cell16
  - entity: sensor.be_battery_voltage_cell17
  - entity: sensor.be_battery_voltage_cell18
  - entity: sensor.be_battery_voltage_cell19
  - entity: sensor.be_battery_voltage_cell20
  - entity: sensor.be_battery_voltage_cell21
  - entity: sensor.be_battery_voltage_cell22
  - entity: sensor.be_battery_voltage_cell23
  - entity: sensor.be_battery_voltage_cell24
  - entity: sensor.be_battery_voltage_cell25
  - entity: sensor.be_battery_voltage_cell26
  - entity: sensor.be_battery_voltage_cell27
  - entity: sensor.be_battery_voltage_cell28
  - entity: sensor.be_battery_voltage_cell29
  - entity: sensor.be_battery_voltage_cell30
  - entity: sensor.be_battery_voltage_cell31
  - entity: sensor.be_battery_voltage_cell32
  - entity: sensor.be_battery_voltage_cell33
  - entity: sensor.be_battery_voltage_cell34
  - entity: sensor.be_battery_voltage_cell35
  - entity: sensor.be_battery_voltage_cell36
  - entity: sensor.be_battery_voltage_cell37
  - entity: sensor.be_battery_voltage_cell38
  - entity: sensor.be_battery_voltage_cell39
  - entity: sensor.be_battery_voltage_cell40
  - entity: sensor.be_battery_voltage_cell41
  - entity: sensor.be_battery_voltage_cell42
  - entity: sensor.be_battery_voltage_cell43
  - entity: sensor.be_battery_voltage_cell44
  - entity: sensor.be_battery_voltage_cell45
  - entity: sensor.be_battery_voltage_cell46
  - entity: sensor.be_battery_voltage_cell47
  - entity: sensor.be_battery_voltage_cell48
  - entity: sensor.be_battery_voltage_cell49
  - entity: sensor.be_battery_voltage_cell50
  - entity: sensor.be_battery_voltage_cell51
  - entity: sensor.be_battery_voltage_cell52
  - entity: sensor.be_battery_voltage_cell53
  - entity: sensor.be_battery_voltage_cell54
  - entity: sensor.be_battery_voltage_cell55
  - entity: sensor.be_battery_voltage_cell56
  - entity: sensor.be_battery_voltage_cell57
  - entity: sensor.be_battery_voltage_cell58
  - entity: sensor.be_battery_voltage_cell59
  - entity: sensor.be_battery_voltage_cell60
  - entity: sensor.be_battery_voltage_cell61
  - entity: sensor.be_battery_voltage_cell62
  - entity: sensor.be_battery_voltage_cell63
  - entity: sensor.be_battery_voltage_cell64
  - entity: sensor.be_battery_voltage_cell65
  - entity: sensor.be_battery_voltage_cell66
  - entity: sensor.be_battery_voltage_cell67
  - entity: sensor.be_battery_voltage_cell68
  - entity: sensor.be_battery_voltage_cell69
  - entity: sensor.be_battery_voltage_cell70
  - entity: sensor.be_battery_voltage_cell71
  - entity: sensor.be_battery_voltage_cell72
  - entity: sensor.be_battery_voltage_cell73
  - entity: sensor.be_battery_voltage_cell74
  - entity: sensor.be_battery_voltage_cell75
  - entity: sensor.be_battery_voltage_cell76
  - entity: sensor.be_battery_voltage_cell77
  - entity: sensor.be_battery_voltage_cell78
  - entity: sensor.be_battery_voltage_cell79
  - entity: sensor.be_battery_voltage_cell80
  - entity: sensor.be_battery_voltage_cell81
  - entity: sensor.be_battery_voltage_cell82
  - entity: sensor.be_battery_voltage_cell83
  - entity: sensor.be_battery_voltage_cell84
  - entity: sensor.be_battery_voltage_cell85
  - entity: sensor.be_battery_voltage_cell86
  - entity: sensor.be_battery_voltage_cell87
  - entity: sensor.be_battery_voltage_cell88
  - entity: sensor.be_battery_voltage_cell89
  - entity: sensor.be_battery_voltage_cell90
  - entity: sensor.be_battery_voltage_cell91
  - entity: sensor.be_battery_voltage_cell92
  - entity: sensor.be_battery_voltage_cell93
  - entity: sensor.be_battery_voltage_cell94
  - entity: sensor.be_battery_voltage_cell95
  - entity: sensor.be_battery_voltage_cell96
  - entity: ""
    internal: false
    type: surface
    x: $ex vars.xs
    "y": $ex vars.data.map((_,i)=>i)
    z: $ex vars.data
```


(Ref: https://github.com/dbuezas/lovelace-plotly-graph-card/discussions/487)
