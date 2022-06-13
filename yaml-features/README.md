# YAML features for KAUF RGB Wall Switch

These files can be compiled with the main kauf-rgbs.yaml file to add features.

## rotate-colors-sync-to-ha-light.yaml

- **Rotate through colors by holding button** - switch and home assistant light entity both change color together.
- **Sync a Home Assistant light entity** - the smart switch will automatically turn the bulb on and off via home assistant when the button is pressed, as well as cycling the light entity through the color wheel along with the RGB Switch's big light when the button is held.

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

**Still Toggle**

This yaml file disables the default button press actions in order to toggle on release instead of press.  This is done by turning on the disable_button configuration switch.  If you already had the disable_button switch turned on because you don't want the RGB Switch button to toggle the switch, then this can be changed to 'false' and pressing the button will not toggle the switch.  The way this works will probably be changed in an update soon to make this substitution no longer necessary.

