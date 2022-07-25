# YAML features for KAUF RGB Wall Switch

These files can be compiled with the main kauf-rgbs.yaml file to add features.

## rotate-colors-sync-to-ha-light.yaml

Primarily intended to allow the switch to control a separate smart light or a group of lights via Home Assistant.  Pressing the switch will toggle the light/group on and off.  Holding the switch will cause the switch and the light/group to cycle through colors.  This is all done by the switch without having to add any automations to Home Assistnt.  Using this yaml file without specifying a Home Assistant light entity just gives you the ability to cycle through colors on the switch by holding it down.

### Video Walkthrough
https://www.youtube.com/watch?v=GERPRWChSQw

### Installation
Include this yaml file as a package along with kauf-rgbs.yaml.  You should have the following in your local yaml file.  rotate-colors-sync-to-ha-light.yaml must be include before kauf-rgbs.yaml.
```
packages:
  rotate_colors: github://bkaufx/kauf-rgb-switch/yaml-features/rotate-colors-sync-to-ha-light.yaml
  Kauf.RGBSw: github://KaufHA/kauf-rgb-switch/kauf-rgbs.yaml
```

### Configuration

Just compiling in the yaml as described above will allow the RGB Switch's big light to rotate through the color wheel by holding the button.

**Home Assistant Light Entity**

To control a light entity with the switch and sync the light entity to the same color through Home Assistant, add the following substitution to define which Home Assistant light entity to sync.

```
substitions:
  hass_light_entity: light.kitchen_lights
```

**Save On/Off State**

The substitutions **save_on_state** and **save_off_state** can be used to save the cycled-to color as the big light's on and off state going forward.  By default, the on-state is automatically saved but the off state is not.  Therefore, when you cycle through colors while the switch is on, the big light will return to the same color when the switch is turned off and back on.

**Delay Settings**

There are two different substitutions to change the hold-time for color changes.  **initial_delay** defines the amount of time before the cycle starts when first holding the button.  **change_delay** defines the amount of time between each color change once the cycle starts.

**Stay Switched**

By default, this yaml file will change the relay configuration to always on at boot.  This is because the yaml file is designed to control smart lights that always stay powered.  Setting the stay_switched substitution to true will keep the relay configured as switched at boot.

**color_0 . . . color_11**

Defines 12 colors that the RGB Switch will cycle through when the button is held.  By default, simply moves around the color wheel like the hours on a clock.  Colors can be customized by redefining the substitutions.  Values should be three comma-separated integers 'r,g,b'.

**Brightness Settings**

The main idea behind the brightness and color settings is first to allow the light to toggle to white instead of only the cycle-selected color.  The point of having entity/attribute options instead of just a fixed value is to allow the lights to initially turn on to a value from an adaptive/circadian light integration instead of turning on first to a fixed value and then to the adaptive/circadian value via the integration.  Creates a cleaner fade to on.  The defined values are passed to [Home Assistant's light.turn_on service](https://www.home-assistant.io/integrations/light/#service-lightturn_on), so the documentation there should be followed as far as acceptable values.

The brightness substitutions control the brightness of the synced light entity when toggled via pressing the RGB Switch's button.  By default, the light entity is toggled without any indication of brightness, which keeps the most recent brightness.  The following options are available, and only one should be used.

- **hass_brightness_entity** - defines a sensor entity in Home Assistant with an integer value between 0 and 255 that will be used for the brightness
- **hass_brightness_entity** and **hass_brightness_attribute** - defines an entity attribute in Home Assistant with an integer value between 0 and 255 that will be used for the brightness.
- **hass_brightness_fixed** - defines a fixed integer value between 0 and 255 that will be used for the brightness.
- **hass_brightness_pct_entity** - defines a sensor entity in Home Assistant with a value between 0 and 100 that will be used for the brightness
- **hass_brightness_pct_entity** and **hass_brightness_pct_attribute** - defines an entity attribute in Home Assistant with a value between 0 and 100 that will be used for the brightness.
- **hass_brightness_pct_fixed** - defines a fixed value between 0 and 100 that will be used for the brightness.

**Color Settings**

Substitutions control the color of the synced light entity when toggled via pressing the RGB Switch's button.  By default, a fixed value of 250 mireds is used, which is 50/50 warm/cold white for the KAUF bulbs.

Any defined value will overwrite the default 250 mired option.  hass_ct_mireds_fixed can be redefined to "none" to turn on the light entity without any indication of color and thereby toggle the light entity to the color-cycle selected color.

The following options are available, and only one should be used.

- **hass_rgb_entity** - defines a Home Assistant entity with an RGB value that will be used as the turn-on color.
- **hass_rgb_entity** and **hass_rgb_attribute** - defines a Home Assistant entity attribute with an RGB value that will be used as the turn-on color.
- **hass_rgb_fixed** - defines a fixed rgb value that will be used as the turn-on color.
- **hass_ct_kelvin_entity** - defines a Home Assistant sensor entity that will be used as the turn-on color temperature with units being Kelvin.
- **hass_ct_kelvin_entity** and **hass_ct_kelvin_attribute** - defines a Home Assistant entity attribute that will be used as the turn-on color temperature with the units being Kelvin.
- **hass_ct_kelvin_fixed** - defines a fixed Kelvin value for the turn-on color temperature.
- **hass_ct_mireds_entity** - defines a Home Assistant sensor entity that will be used as the turn-on color temperature with units being mireds.
- **hass_ct_mireds_entity** and **hass_ct_mireds_attribute** - defines a Home Assistant entity attribute that will be used as the turn-on color temperature with the units being mireds.
- **hass_ct_mireds_fixed** - defines a fixed mired value for the turn-on color temperature.

## hold-for-fan-mode.yaml

Hold the switch's button to put the switch into fan mode, wherein pressing the switch rotates through fan speeds instead of toggling a light on and off.  The fan mode times out after 5 seconds by default.

### Installation
Include this yaml file as a package along with kauf-rgbs.yaml.  You should have the following in your local yaml file.  hold-for-fan-mode.yaml must be include before kauf-rgbs.yaml.
```
packages:
  fan_mode: github://bkaufx/kauf-rgb-switch/yaml-features/hold-for-fan-mode.yaml
  Kauf.RGBSw: github://KaufHA/kauf-rgb-switch/kauf-rgbs.yaml
```

### Configuration

**Home Assistant Fan Entity**

The **hass_fan_entity** substitution indicates which fan entity to control.

```
substitions:
  hass_fan_entity: fan.kauf_office
```

**Hold Time**

The **hold_time** substitution defines the hold time required to enter fan mode.  750 ms by default.

**Mode Time**

The **mode_time** substitution defines the amount of time without a press that will cause the switch to exit fan mode.

**Fan Speed Percentages**

The substitutions **speed_0_prcnt, speed_1_prcnt, speed_2_prcnt, and speed_3_prcnt** allow configuration of four different speed percentages for the fan.  By default, the speeds are 0, 33, 67, and 100.

**Fan Speed Colors**

The substitutions **speed_0_color, speed_1_color, speed_2_color, and speed_3_color** allow configuration of four different speed colors for the four fan speeds.  By default, the colors are off, red, yellow, and green.  The colors need to be a string with three comma-separated RGB values, each being an integer from 0-255.  The small light will be changed to the appropriate color when the fan speed changes.

**Mode Color**

The **mode_color** substitution defines a color for the big light when the switch is in fan mode, defaults to red.  Needs to be 3 comma-separated integers between 0-255.

**Home Assistant Light Entity**

If set, the **hass_light_entity** indicates a light entity for the switch to turn on and off when not in fan mode, instead of just turning itself on and off.  All of the brightness and color temp substitutions described above for rotate-colors-sync-to-ha-light.yaml are also valid for this yaml file.
