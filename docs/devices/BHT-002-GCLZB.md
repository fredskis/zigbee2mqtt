---
title: "Moes BHT-002-GCLZB control via MQTT"
description: "Integrate your Moes BHT-002-GCLZB via Zigbee2MQTT with whatever smart home
 infrastructure you are using without the vendors bridge or gateway."
---

*To contribute to this page, edit the following
[file](https://github.com/Koenkk/zigbee2mqtt.io/blob/master/docs/devices/BHT-002-GCLZB.md)*

# Moes BHT-002-GCLZB

| Model | BHT-002-GCLZB  |
| Vendor  | Moes  |
| Description | Moes BHT series Thermostat |
| Supports | thermostat, temperature |
| Picture | ![Moes BHT-002-GCLZB](../images/devices/BHT-002-GCLZB.jpg) |

## Notes


### Stop message flooding
This unit has a bug that makes it send multiple messages when updating. To stop this from flooding your MQTT Queues, please add the following to your `configuration.yaml` file:

{% raw %}
devices:
  '0x12345678':
    friendly_name: thermostat
    debounce: 1
{% endraw %}



## Exposes
### Lock 
TODO

### Climate 
TODO

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
lock:
  - platform: "mqtt"
    state_topic: true
    availability_topic: "zigbee2mqtt/bridge/state"
    command_topic: "zigbee2mqtt/<FRIENDLY_NAME>/set"
    value_template: "{{ value_json.child_lock }}"
    payload_lock: "LOCK"
    payload_unlock: "UNLOCK"
    state_locked: "LOCKED"
    state_unlocked: "UNLOCKED"
    command_topic_postfix: "child_lock"

climate:
  - platform: "mqtt"
    availability_topic: "zigbee2mqtt/bridge/state"
    temperature_unit: "C"
    temp_step: 1
    min_temp: "5"
    max_temp: "30"
    current_temperature_topic: true
    current_temperature_template: "{{ value_json.local_temperature }}"
    mode_state_topic: true
    mode_state_template: "{{ value_json.system_mode }}"
    modes: 
      - "off"
      - "heat"
    mode_command_topic: true
    action_topic: true
    action_template: "{% set values = {'idle':'off','heat':'heating','cool':'cooling','fan only':'fan'} %}{{ values[value_json.running_state] }}"
    temperature_command_topic: "current_heating_setpoint"
    temperature_state_template: "{{ value_json.current_heating_setpoint }}"
    temperature_state_topic: true
    hold_modes: 
      - "hold"
      - "program"
    hold_command_topic: true
    hold_state_template: "{{ value_json.preset }}"
    hold_state_topic: true

sensor:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    unit_of_measurement: "lqi"
    value_template: "{{ value_json.linkquality }}"
    icon: "mdi:signal"
```
{% endraw %}

