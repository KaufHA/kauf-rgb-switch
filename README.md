# KAUF RGB Wall Switch (SRF10)
Files for the KAUF RGB Wall Switch

The recommended way to import a wall switch into your ESPHome dashboard is through the dashboard import feature.  The following yaml is the minimum required to utilize the yaml file in this github repo as a package, as with the dashboard import feature.  The friendly_name substitution is recommended and will not be automatically created by the ESPHome dashboard import.


```
substitutions:
  name: bedroom-light
  friendly_name: Bedroom Light

packages:
  kauf.plf10: github://KaufHA/kauf-rgb-switch/kauf-rgbs.yaml

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
```

## Repo Contents

This repo contains files for the KAUF RGB Light Switch.

***kauf-rgbs.yaml*** - The yaml file recommended to import a switch into your ESPHome dashboard and keep all custom Kauf functionality.  This is the yaml file incorporated automatically when the dashboard import feature is used.

***kauf-rgbs-lite.yaml*** - A yaml file without any Kauf custom components but otherwise keeping as much functionality from kauf-rgbs.yaml as possible.

***kauf-rgbs-minimal.yaml*** - A yaml file to import a switch into your ESPHome dashboard with only basic functionality.  The RGB lights will exist as light entities but will not be automated in any way

***kauf-rgbs-update.yaml*** - The yaml file to build the update bin file.  Generally not useful to end users.

***kauf-rgbs-factory.yaml*** - The yaml file to build the factory bin file.  Generally not useful to end users.

# Entities
If using the precompiled binary or kauf-rgbs.yaml as a package in the ESPHome dashboard, the following configuration entities are automatically created.  Entities listed as disabled by default can be enabled in Home Assistant, or simply modified through the web interface by clicking "Visit Device" in Home Assistant or typing the switch's IP address into a web browser.


## Control Entities

***Kauf RGB Sw*** (or $friendly_name if redefined) switch entity - Represents the main on/off state of the KAUF RGB Switch.  Toggling this switch will toggle the switch's relay as well, unless the relay is configured to always on or always off.

***Big Light*** light entity - directly controls the switch's big light, which occupies the entire button.

***Small Light*** light entity - directly controls the switch's small light, which occupies the bottom-right corner of the switch.


## Sensor Entities

***Button*** binary sensor entity - Indicates whether the switch's physical button is presently being pressed.


## Configuration Entities

***Big Off Value*** light entity - Allows setting of a light state, which will automatically be copied to the *Big Light* light entity when the switch turns off.

***Big On Value*** light entity - Allows setting of a light state, which will automatically be copied to the *Big Light* light entity when the switch turns on.

***Small Off Value*** light entity - Allows setting of a light state, which will automatically be copied to the *Small Light* light entity when the switch turns off.

***Small On Value*** light entity - Allows setting of a light state, which will automatically be copied to the *Small Light* light entity when the switch turns on.

***Disable Button*** switch entity - Pressing the button will not toggle the main switch or relay if this configuration switch is turned on.  The button binary sensor will still show when the button is pressed so the button can be used for a different automation.

***No HASS*** switch entity - Disabled by default.  Turn this switch on if not using Home Assistant with the switch.  Prevents the switch from flashing an error due to no API connection and rebooting every 15 minutes.

***Relay Config*** select entity - Allows the relay to be configured to always on, always off, or switched.  Switched means the relay will provide power to the load only when the main switch entity is on.

# Advanced Settings
When using kauf-rgbs.yaml as a package in the ESPHome dashboard, you can configure the following aspects by adding substitutions.  The substitutions section of kauf-rgbs.yaml has comments with more explanation as well.

***friendly_name*** - The friendly name will be used to name every entity in Home Assistant.  Add a substitution to change this to something descriptive for each device.

***disable_entities*** - Adding a substitution to redefine this to "false" will result in all entities being automatically enabled in Home Assistant.

***sub_on_press*** - Defines a script to be executed when the switch's button is pressed.

***sub_on_release*** - Defines a script to be executed when the switch's button is released.

***sub_on_turn_on*** - Defines a script to be executed when the switch is turned on.

***sub_on_turn_off*** - Defines a script to be executed when the switch is turned off.

***sub_blink_check*** - Defines a script that, if running, stops the error light from blinking.
