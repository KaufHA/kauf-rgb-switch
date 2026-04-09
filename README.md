# KAUF RGB Wall Switch (SRF10)
Files for the KAUF RGB Wall Switch

The recommended way to import a wall switch into your ESPHome dashboard is through the dashboard import feature.  The following yaml is the minimum required to utilize the yaml files in this github repo as packages, as with the dashboard import feature.


```
substitutions:
  name: bedroom-light
  friendly_name: Bedroom Light

packages:
  # Use the entrypoint matching your hardware variant (1MiB or 4MiB flash).
  # The flash size is printed on the label on the back of the switch.
  # See kauf-rgbs-1m-4m-comparison.md for more ways to identify your variant.
  Kauf.RGBSw: github://KaufHA/kauf-rgb-switch/packages/kauf-srf10-1m.yaml
  # Kauf.RGBSw: github://KaufHA/kauf-rgb-switch/packages/kauf-srf10-4m.yaml

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
```

## Repo Contents

***packages/kauf-srf10-1m.yaml*** - Entrypoint for 1MiB ESP8266 switches.  Defaults to kauf components and default profile.

***packages/kauf-srf10-4m.yaml*** - Entrypoint for 4MiB ESP8266 switches.  Defaults to kauf components and default profile.

***packages/kauf-rgbs.yaml*** - Generic entrypoint with no defaults.  Requires `components`, `profile`, and `target` substitutions to be defined.

***packages/profiles/*** - Profile yaml files defining device functionality.  See [Profile Options](#profile-options) for details.

***packages/targets/*** - Target yaml files defining hardware configuration for each board variant.  See [Target Options](#target-options) for details.

***packages/updates/*** - Yaml files used to build OTA update binaries.  Generally not useful to end users.

***yaml-features/*** - Contains additional yaml files that can be compiled in to add features.  See readme file in that directory for more information.

***[kauf-rgbs-1m-4m-comparison.md](kauf-rgbs-1m-4m-comparison.md)*** - Explains the differences between 1MiB and 4MiB switch variants and how to identify which variant you have.

# Entities
If using the precompiled binary or the default packages in the ESPHome dashboard, the following configuration entities are automatically created.  Entities listed as disabled by default can be enabled in Home Assistant, or simply modified through the web interface by clicking "Visit Device" in Home Assistant or typing the switch's IP address into a web browser.


## Control Entities

***Kauf RGB Sw*** (or $friendly_name if redefined) switch entity - Represents the main on/off state of the KAUF RGB Switch.  Toggling this switch will toggle the switch's relay as well, unless the relay is configured to always on or always off.

***Big Light*** light entity - directly controls the switch's big light, which occupies the entire button.

***Small Light*** light entity - directly controls the switch's small light, which occupies the bottom-right corner of the switch.


## Sensor Entities

***Button*** binary sensor entity - Indicates whether the switch's physical button is currently being pressed.

***Double Press*** binary sensor entity - Goes high for 250ms after the switch's button is double pressed.

***Hold*** binary sensor entity - Goes high when the switch's button is held for 750ms.  Stays high until the button is released.

***Release Without Hold*** binary sensor entity - Goes high when the switch's button is released if the button was held for less than 750ms, indicating that a press occured but not a hold.  This binary sensor is faster than the *Single Press* entity because this entity goes high immediately upon release rather than waiting 333ms to see if a double press will occur.

***Single Press*** binary sensor entity - Goes high for 250ms after the switch's button is single pressed.  A press only counts as a single press if it is held for less than 750ms (otherwise it would be a hold) and is not followed by a second press within 333ms of release (otherwise it would be the first press of a double press).


## Configuration Entities

***Big Off Value*** light entity - Allows setting of a light state that will automatically be copied to the *Big Light* light entity when the main switch turns off.  If this configuration light entity is turned off, then the big light will turn off when the main switch turns off.  To disable automation of the big light when the main switch turns off, turn this configuration light entity on and then select the Disabled effect.

***Big On Value*** light entity - Allows setting of a light state that will automatically be copied to the *Big Light* light entity when the main switch turns on.  If this configuration light entity is turned off, then the big light will turn off when the main switch turns on.  To disable automation of the big light when the main switch turns on, turn this configuration light entity on and then select the Disabled effect.

***Small Off Value*** light entity - Allows setting of a light state that will automatically be copied to the *Small Light* light entity when the main switch turns off.  If this configuration light entity is turned off, then the small light will turn off when the main switch turns off.  To disable automation of the small light when the main switch turns off, turn this configuration light entity on and then select the Disabled effect.

***Small On Value*** light entity - Allows setting of a light state that will automatically be copied to the *Small Light* light entity when the main switch turns on.  If this configuration light entity is turned off, then the small light will turn off when the main switch turns on.  To disable automation of the small light when the main switch turns on, turn this configuration light entity on and then select the Disabled effect.

***No HASS*** switch entity - Disabled by default.  Turn this switch on if not using Home Assistant with the switch.  Prevents the switch from flashing an error due to no API connection and rebooting every 15 minutes.

***Relay Config*** select entity - Allows the relay to be configured to always on, always off, or switched.  Switched means the relay will provide power to the load only when the main switch entity is on.

***Button Config*** select entity - Configures how the button is used to toggle the UI switch (and the relay if the relay is configured to be switched).
- **Toggle on Press** - The UI switch toggles any time the button is physically pressed.  A double press will result in the UI switch being toggled twice.
- **Disabled** - The button will not toggle the UI switch.
- **Toggle on Release** - The UI switch toggles any time the button is physically released.
- **Toggle on Single Press** - The UI switch toggles any time the button is single pressed (i.e., pressed once without being held and without a double press).
- **Toggle on Double Press** - The UI switch toggles any time the button is double pressed.
- **Toggle on Hold** - The UI switch toggles any time the button is held.


## Diagnostic Entities

***Button Press Duration*** sensor entity - gives the duration of the most recent button press.  Goes to 0 while button is pressed.

***IP Address*** sensor entity - gives the switch's IP address.

***Restart Firmware*** button entity - restarts the switch's firmware when pressed.

***Restore Lights*** button entity - restores both lights based on configuration light entities and switch state.  If you change light values, this button can be used to restore them back to their normal states.

***Uptime*** sensor entity - gives the switch's uptime in seconds.

# Advanced Settings
When using the default yaml packages in the ESPHome dashboard, you can configure the following aspects by adding substitutions.

***components*** - Selects which set of components to use.  Defaults to `kauf`.  Set to `stock` to use stock ESPHome components without any Kauf customizations.

***profile*** - Selects which profile to use.  Defaults to `default`.  Set to `minimal` for basic functionality with no button automation, light automation, or configuration entities.

***friendly_name*** - The friendly name will be used to name every entity in Home Assistant.  Add a substitution to change this to something descriptive for each device.

***disable_entities*** - Adding a substitution to redefine this to "false" will result in all entities being automatically enabled in Home Assistant.  By default, this is true so that a lot of rarely-used entities are hidden in Home Assistant.

***sub_on_press*** - Defines a script to be executed when the switch's button is pressed.

***sub_on_release*** - Defines a script to be executed when the switch's button is released.

***sub_on_single_press*** - Defines a script to be executed when the switch's button is single pressed.

***sub_on_double_press*** - Defines a script to be executed when the switch's button is double pressed.

***sub_on_hold*** - Defines a script to be executed when the switch's button is held.

***sub_on_turn_on*** - Defines a script to be executed when the switch is turned on.

***sub_on_turn_off*** - Defines a script to be executed when the switch is turned off.

***sub_blink_check*** - Defines a script that, if running, stops the error light from blinking.

***sub_transition_length*** - Change the default transition length for both lights.

***wifi_ap_timeout*** - Change the timeout before the switch's Wi-Fi access point turns off when no client connects.  Defaults to 2 minutes.

***sub_double_press_max_gap*** - A length of time defining the maximum gap between two presses that will result in a double press.  This also defines the delay after a press when a single press will be recognized, due to having to wait this long to distinguish between a single and a double press.

***sub_hold_time*** - A length of time defining how long the button must be held to count as a hold.  This also defines the maximum amount of time for a single press and the maximum amount of time for the first press of a double press.

***sub_button_sensor_duration*** - A length of tiem defining how long the single press and double press binary sensors will be high for whenever triggered.

# Package Structure

The yaml files are organized as a profile combined with a target, located under the `packages/` directory.  The hardware-specific entrypoint files default to kauf components and the default profile.  The profile and components can be changed using substitutions without having to switch entrypoint files.

For example, to use the minimal profile instead of the default you only need to add the profile substitution:

```
substitutions:
  name: bedroom-light
  friendly_name: Bedroom Light
  profile: minimal

packages:
  Kauf.RGBSw: github://KaufHA/kauf-rgb-switch/packages/kauf-srf10-1m.yaml

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
```

To use stock ESPHome components instead of Kauf custom components simply add the components substitution.  Some functionality may be lost:

```
substitutions:
  name: bedroom-light
  friendly_name: Bedroom Light
  components: stock

packages:
  Kauf.RGBSw: github://KaufHA/kauf-rgb-switch/packages/kauf-srf10-1m.yaml

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
```

## Profile Options

***default*** - Full functionality including button automation, light automation, and all configuration entities.

***minimal*** - Basic functionality only: button toggles relay, no light automation, no configuration entities.

## Components Options

***kauf*** - Includes Kauf custom components and stable flash layout.  (default)

***stock*** - Stock ESPHome components only, no Kauf-specific customizations.

## Target Options

The target is selected by which entrypoint file is used.  Targets define hardware-specific configuration such as GPIO pin assignments and flash size.

***kauf-srf10-1m*** - ESP8266 1MiB flash (esp01_1m board).

***kauf-srf10-4m*** - ESP8266 4MiB flash (esp07s board).


# yaml-migration

The top-level `kauf-rgbs.yaml` and `kauf-rgbs-4m.yaml` files are deprecated in favor of the new package structure described above.

**If you are currently using `kauf-rgbs.yaml`**, change the following line in your yaml file:


Change this:
```
packages:
  Kauf.RGBSw: github://KaufHA/kauf-rgb-switch/kauf-rgbs.yaml
```
to this:

```
packages:
  Kauf.RGBSw: github://KaufHA/kauf-rgb-switch/packages/kauf-srf10-1m.yaml
```

**If you are currently using `kauf-rgbs-4m.yaml`**, change the following line in your yaml file:


Change this:
```
packages:
  Kauf.RGBSw: github://KaufHA/kauf-rgb-switch/kauf-rgbs-4m.yaml
```
to this:

```
packages:
  Kauf.RGBSw: github://KaufHA/kauf-rgb-switch/packages/kauf-srf10-4m.yaml
```

All substitutions and customizations from the old yaml files are compatible with the new files.


# Troubleshooting

If one or more of the LED colors are not working, the LED board may have jostled loose during shipping.  The ground wire may also disconnect the LED board if pressed in too far.  The LED board pins can be pressed back down to restore connection by removing the plastic front cover of the light.  Just press down on the two tabs indicated by the orange arrows below.  The tabs on either side of the plastic cover can be used, or you can press in all 4 at once using both hands.

![Step 1](https://user-images.githubusercontent.com/89616381/175793362-fc314ea4-e336-4d86-813c-54c6e8141e6c.jpg)

Once you have the front cover off, you'll see the LED board as in the image below.  The big light LEDs are controlled through the terminal indicated by the orange arrow below.  Press down there to re-establish connection.  There are also two more terminal blocks near the bottom of the switch that may need pressed in.  The three-pin block beside the button is RGB for the small light.  The two-terminal block under the button is the ground connection for both the big and small light.

![Step 2](https://user-images.githubusercontent.com/89616381/175793457-2f06804a-17e9-46d8-a647-dcf889f1c646.jpg)


Additional troubleshooting ideas that apply to all products are located in the [Common repo's readme](https://github.com/KaufHA/common/blob/main/README.md#troubleshooting).

# Links

- [Purchase switches on Amazon](https://www.amazon.com/dp/B0B1CKWV7K)
- [KAUF YouTube Channel](https://www.youtube.com/channel/UCjgziIA-lXmcqcMIm8HDnYg)
- [KaufHA Website](https://kaufha.com/srf10)
