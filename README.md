# KAUF RGB Wall Switch (SRF10)
Files for the KAUF RGB Wall Switch

The recommended way to import a wall switch into your ESPHome dashboard is through the dashboard import feature.  The following yaml is the minimum required to utilize the yaml file in this github repo as a package, as with the dashboard import feature.  The friendly_name substitution is recommended and will not be automatically created by the ESPHome dashboard import.


```
substitutions:
  name: bedroom-light
  friendly_name: Bedroom Light

packages:
  Kauf.RGBSw: github://KaufHA/kauf-rgb-switch/kauf-rgbs.yaml

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

***yaml-features* directory** - Contains additional yaml files that can be compiled in to add features.  See readme file in that directory for more information.

# Entities
If using the precompiled binary or kauf-rgbs.yaml as a package in the ESPHome dashboard, the following configuration entities are automatically created.  Entities listed as disabled by default can be enabled in Home Assistant, or simply modified through the web interface by clicking "Visit Device" in Home Assistant or typing the switch's IP address into a web browser.


## Control Entities

***Kauf RGB Sw*** (or $friendly_name if redefined) switch entity - Represents the main on/off state of the KAUF RGB Switch.  Toggling this switch will toggle the switch's relay as well, unless the relay is configured to always on or always off.

***Big Light*** light entity - directly controls the switch's big light, which occupies the entire button.

***Small Light*** light entity - directly controls the switch's small light, which occupies the bottom-right corner of the switch.


## Sensor Entities

***Button*** binary sensor entity - Indicates whether the switch's physical button is presently being pressed.

***Double Press*** binary sensor entity - Goes high for 250ms after the switch's button is double pressed



## Configuration Entities

***Big Off Value*** light entity - Allows setting of a light state that will automatically be copied to the *Big Light* light entity when the main switch turns off.  If this configuration light entity is turned off, then the big light will turn off when the main switch turns off.  To disable automation of the big light when the main switch turns off, turn this configuration light entity on and then select the Disabled effect.

***Big On Value*** light entity - Allows setting of a light state that will automatically be copied to the *Big Light* light entity when the main switch turns on.  If this configuration light entity is turned off, then the big light will turn off when the main switch turns on.  To disable automation of the big light when the main switch turns on, turn this configuration light entity on and then select the Disabled effect.

***Small Off Value*** light entity - Allows setting of a light state that will automatically be copied to the *Small Light* light entity when the main switch turns off.  If this configuration light entity is turned off, then the small light will turn off when the main switch turns off.  To disable automation of the small light when the main switch turns off, turn this configuration light entity on and then select the Disabled effect.

***Small On Value*** light entity - Allows setting of a light state that will automatically be copied to the *Small Light* light entity when the main switch turns on.  If this configuration light entity is turned off, then the small light will turn off when the main switch turns on.  To disable automation of the small light when the main switch turns on, turn this configuration light entity on and then select the Disabled effect.

***No HASS*** switch entity - Disabled by default.  Turn this switch on if not using Home Assistant with the switch.  Prevents the switch from flashing an error due to no API connection and rebooting every 15 minutes.

***Relay Config*** select entity - Allows the relay to be configured to always on, always off, or switched.  Switched means the relay will provide power to the load only when the main switch entity is on.

***Button Config*** select entity - Pressing the button will not toggle the main switch or relay if the Disabled option is selected.  The button binary sensor will still show when the button is pressed so the button can be used for a different automation.  Otherwise, either toggling on press or toggling on release can be selected.


## Diagnostic Entities

***Button Press Duration*** sensor entity - gives the duration of the most recent button press.  Goes to 0 while button is pressed.

***IP Address*** sensor entity - gives the switch's IP address.

***Restart Firmware*** button entity - restarts the switch's firmware when pressed.

***Restore Lights*** button entity - restores both lights based on configuration light entities and switch state.  If you change light values, this button can be used to restore them back to their normal states.

***Uptime*** sensor entity - gives the switch's uptime in seconds.

# Advanced Settings
When using kauf-rgbs.yaml as a package in the ESPHome dashboard, you can configure the following aspects by adding substitutions.  The substitutions section of kauf-rgbs.yaml has comments with more explanation as well.

***friendly_name*** - The friendly name will be used to name every entity in Home Assistant.  Add a substitution to change this to something descriptive for each device.

***disable_entities*** - Adding a substitution to redefine this to "false" will result in all entities being automatically enabled in Home Assistant.

***sub_on_press*** - Defines a script to be executed when the switch's button is pressed.

***sub_on_release*** - Defines a script to be executed when the switch's button is released.

***sub_on_turn_on*** - Defines a script to be executed when the switch is turned on.

***sub_on_turn_off*** - Defines a script to be executed when the switch is turned off.

***sub_blink_check*** - Defines a script that, if running, stops the error light from blinking.

***sub_transition_length*** - Change the default transition length for both lights.

# Troubleshooting

If one or more of the LED colors are not working, the LED board may have jostled loose during shipping.  The LED board pins can be pressed back down to restore connection by removing the plastic front cover of the light.  Just press down on the two tabs indicated by the orange arrows below.  The tabs on either side of the plastic cover can be used, or you can press in all 4 at once using both hands.

![Step 1](https://user-images.githubusercontent.com/89616381/175793362-fc314ea4-e336-4d86-813c-54c6e8141e6c.jpg)

Once you have the front cover off, you'll see the LED board as in the image below.  The big light LEDs are controlled through the terminal indicated by the orange arrow below.  Press down there to re-establish connection.  There are also two more terminal blocks near the bottom of the switch that may need pressed in.  The three-pin block beside the button is RGB for the small light.  The two-terminal block under the button is the ground connection for both the big and small light.

![Step 2](https://user-images.githubusercontent.com/89616381/175793457-2f06804a-17e9-46d8-a647-dcf889f1c646.jpg)


Additional troubleshooting ideas that apply to all products are located in the [Common repo's readme](https://github.com/KaufHA/common/blob/main/README.md#troubleshooting).

# Links

- [Purchase switches on Amazon](https://www.amazon.com/dp/B0B1CKWV7K)
- [KAUF YouTube Channel](https://www.youtube.com/channel/UCjgziIA-lXmcqcMIm8HDnYg)
- [KaufHA Website](https://kaufha.com/srf10)
