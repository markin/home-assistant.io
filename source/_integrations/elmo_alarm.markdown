---
title: Elmo Alarm
description: Instructions on how to integrate a e-connect Elmo SpA alarm system with Home Assistant.
logo: https://www.elmospa.com/images/loghi/resized/logo_elmo_m.png
ha_category:
  - Alarm
  - Binary Sensor
ha_release: 0.105
ha_iot_class: Local Push
ha_codeowners:
  - '@markin'
---

The `ness_alarm` integration will allow Home Assistant users who own a Elmo alarm system to leverage their alarm system and its sensors to provide Home Assistant with information about their homes. 
Connectivity between Home Assistant and the alarm is accomplished through the e-connect platform, so a connectivity module is required to be present in the alarm.

There is currently support for the following device types within Home Assistant:

- Binary Sensor: Reports on zones and inputs statuses
- Alarm Control Panel: Reports on alarm status, and can be used to arm/disarm the system

The module communicates via the e-connect platform (https://e-connect.elmospa.com/).

## Configuration

A `elmo_alarm` section must be present in the `configuration.yaml` file and contain the following options as required:

```yaml
# Example configuration.yaml entry
elmo_alarm:
  host: 'https://connect.elmospa.com'
  vendor: '<vendor>'
  username: '<username>'
  password: '<password>'
  scan_interval:
    seconds: 5
  states:
    - name: 'arm_home'
      zones: [1]
    - name: 'arm_away'
      zones: [1, 2, 3, 4]
    - name: 'arm_night'
      zones: [1, 2, 4]
    - name: 'arm_custom_bypass'
      zones: [3]
```

#### Configuration parameters explanation

- host:
  description: The hostname of e-connect platform (should be https://connect.elmospa.com).
  required: true
  type: string
- vendor:
  description: The installer name that represents the path on the e-connect url (e.g. https://connect.elmospa.com/wegma --> vendor should be 'wegma').
  required: true
  type: integer
- scan_interval:
  description: "Time interval between updates. Supported formats: `scan_interval: 'HH:MM:SS'`, `scan_interval: 'HH:MM'` and Time period dictionary (see example below)."
  required: false
  default: '00:01:00'
  type: time
- username:
  description: The username for e-connect platform.
  required: true
  type: string
- password:
  description: The password for e-connect platform.
  required: true
  type: string
- states:
  description: List of available states
  required: true
  type: list
  keys:
  zone_id:
    description: ID of the zone on the alarm system (i.e Zone 1 -> Zone 16).
    required: true
    type: integer
  name:
    description: Name of the state ("arm_home", "arm_away", "arm_night", "arm_custom_bypass").
    required: true
    type: string
  zones:
    description: The zones indexes associated with the state. 
    required: false
    type: list

#### Time period dictionary example

```yaml
scan_interval:
  # At least one of these must be specified:
  days: 0
  hours: 0
  minutes: 0
  seconds: 10
  milliseconds: 0
```
