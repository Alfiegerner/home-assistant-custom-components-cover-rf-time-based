# Cover Time Based Component (RF version)
Cover Time Based Component for your Home-Assistant (http://www.home-assistant.io) based on https://github.com/davidramosweb/home-assistant-custom-components-cover-time-based , modified for RF covers.

With this component you can add a time-based cover. You have to set RF-sending scripts to open, close and stop the cover. 
For example, you can use an RF-Bridge running Tasmota firmware to send out RF codes, with RF-based motor roller shutters to do this.

You can adapt it to your requirements, actually any cover system could be used which uses 3 triggers: up, stop, down. The idea is to embed your triggers into scripts which can be hooked into this component as below.

## Installation
* Copy all files in custom_components/cover_rf_time_based to your <config directory>/custom_components/cover_rf_time_based/ directory.
* Restart Home-Assistant.
* Create the required scripts in scripts.yaml.
* Add the configuration to your configuration.yaml.
* Restart Home-Assistant again.

### Usage
To use this component in your installation, you have to set RF-sending scripts to open, close and stop the cover (see below), and add the following to your configuration.yaml file:

### Example configuration.yaml entry

```
cover:
  - platform: cover_rf_time_based
    devices:
        my_room_cover_time_based:
        name: My Room Cover
        travelling_time_up: 36
        travelling_time_down: 34
        close_script_entity_id: script.rf_myroom_cover_down
        stop_script_entity_id: script.rf_myroom_cover_stop
        open_script_entity_id: script.rf_myroom_cover_up
        aliases:
          - my_room_cover_time_based
```
### Example scripts.yaml entry
This example assumes that you're using an MQTT-RF bridge running Tasmota open source firmware (https://tasmota.github.io/docs/devices/Sonoff-RF-Bridge-433/). Of course you can customize based on what ever other way to trigger these 3 type of movements.
```
'rf_myroom_cover_down':
  alias: 'RF send MyRoom Cover DOWN'
  sequence:
  - service: mqtt.publish
    data:
      topic: 'cmnd/rf-bridge-1/backlog'
      payload: 'rfraw XXXXXXXXX....XXXXXXXXXX;rfraw 0'

'rf_myroom_cover_stop':
  alias: 'RF send MyRoom Cover STOP'
  sequence:
  - service: mqtt.publish
    data:
      topic: 'cmnd/rf-bridge-1/backlog'
      payload: 'rfraw XXXXXXXXX....XXXXXXXXXX;rfraw 0'

 'rf_myroom_cover_up':
  alias: 'RF send MyRoom Cover UP'
  sequence:
  - service: mqtt.publish
    data:
      topic: 'cmnd/rf-bridge-1/backlog'
      payload: 'rfraw XXXXXXXXX....XXXXXXXXXX;rfraw 0'
```

### Icon customization
For proper icon display (opened/closed) customization can be added to `configuration.yaml` based of what type of covers you have, either one by one, or for all covers at once:

```
homeassistant:
  customize:
    cover.my_room_cover_time_based:
      device_class: shutter
  customize_domain:
     cover:
      device_class: shutter
```
More details: https://www.home-assistant.io/docs/configuration/customizing-devices/#device-class
