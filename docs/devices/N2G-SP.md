---
title: "NET2GRID N2G-SP control via MQTT"
description: "Integrate your NET2GRID N2G-SP via Zigbee2MQTT with whatever smart home
 infrastructure you are using without the vendors bridge or gateway."
---

*To contribute to this page, edit the following
[file](https://github.com/Koenkk/zigbee2mqtt.io/blob/master/docs/devices/N2G-SP.md)*

# NET2GRID N2G-SP

| Model | N2G-SP  |
| Vendor  | NET2GRID  |
| Description | White Net2Grid power outlet switch with power meter |
| Supports | on/off, power measurement |
| Picture | ![NET2GRID N2G-SP](../images/devices/N2G-SP.jpg) |

## Notes


### Deprecated click event
By default this device exposes a deprecated `click` event. It's recommended to use the `action` event instead.

To disable the `click` event, set `legacy: false` for this device in `configuration.yaml`. Example:

```yaml
devices:
  '0x12345678':
    friendly_name: my_device
    legacy: false
```

### Device type specific configuration
*[How to use device type specific configuration](../information/configuration.md)*

* `legacy`: Set to `false` to disable the legacy integration (highly recommended!) (default: true)



## Exposes
### Switch 
The current state of this switch is in the published state under the `state` property (value is `ON` or `OFF`).
To control this switch publish a message to topic `zigbee2mqtt/[FRIENDLY_NAME]/set` with payload `{"state": "ON"}`, `{"state": "OFF"}` or `{"state": "TOGGLE"}`.
To read the current state of this switch publish a message to topic `zigbee2mqtt/[FRIENDLY_NAME]/get` with payload `{"state": ""}`.

### Power (numeric)
Instantaneous measured power.
Value can be found in the published state on the `power` property.
To read (`/get`) the value publish a message to topic `zigbee2mqtt/[FRIENDLY_NAME]/get` with payload `{"power": ""}`.
It's not possible to write (`/set`) this value.
The unit of this value is `W`.

### Energy (numeric)
Sum of consumed energy.
Value can be found in the published state on the `energy` property.
To read (`/get`) the value publish a message to topic `zigbee2mqtt/[FRIENDLY_NAME]/get` with payload `{"energy": ""}`.
It's not possible to write (`/set`) this value.
The unit of this value is `kWh`.

### Linkquality (numeric)
Link quality (signal strength).
Value can be found in the published state on the `linkquality` property.
It's not possible to read (`/get`) or write (`/set`) this value.
The minimimal value is `0` and the maximum value is `255`.
The unit of this value is `lqi`.

## Manual Home Assistant configuration
Although Home Assistant integration through [MQTT discovery](../integration/home_assistant) is preferred,
manual integration is possible with the following configuration:


{% raw %}
```yaml
switch:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    payload_off: "OFF"
    payload_on: "ON"
    value_template: "{{ value_json.state }}"
    command_topic: "zigbee2mqtt/<FRIENDLY_NAME>/set"

sensor:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    unit_of_measurement: "W"
    value_template: "{{ value_json.power }}"
    icon: "mdi:flash"

sensor:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    unit_of_measurement: "kWh"
    value_template: "{{ value_json.energy }}"
    icon: "mdi:power-plug"

sensor:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    unit_of_measurement: "lqi"
    value_template: "{{ value_json.linkquality }}"
    icon: "mdi:signal"
```
{% endraw %}

