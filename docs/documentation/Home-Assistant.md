# Home Assistant chart examples
### Plotly graph cell monitor 180 cell variant (Ref: https://github.com/dbuezas/lovelace-plotly-graph-card/discussions/487)
````
type: custom:plotly-graph
raw_plotly_config: true
title: Battery 0
fn: |-
  $ex {
    vars.x = [];
    vars.y = [];
    vars.z = [];
      

    for (let j = 0; j < 180; j++) {
      vars.z[j] = [];
      // Calculate the index in the 1D array corresponding to the 2D matrix element
      let index = j;

      // Populate the matrix with values from the 1D array
      vars.z[j][j] = hass.states["sensor.be_battery_voltage_cell" + (index+1).toString()].state
    }
    

    for (let j = 0; j < 180; j++) {
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
````
