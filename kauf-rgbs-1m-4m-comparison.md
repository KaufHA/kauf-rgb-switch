Newer models of the RGB Switch now come with 4 MB of memory instead of 1MB.  The two different models require different update files if manually updating or different yaml packages in the ESPHome dashboard.

If you have a ***1MB*** model, ensure you only flash our update files with `-1m` in the filename.  In the ESPHome dashboard, you should only use `kauf-rgbs.yaml`.

If you have a ***4MB*** model, ensure you only flash our update files with `-4m` in the filename.  In the ESPHome dashboard, you should only use `kauf-rgbs-4m.yaml`.

In general, you should not have to worry much about which version of a switch you have.  The proper yaml file will be included by default when adopting a switch in the ESPHome dashboard.  The manual update files will refuse to cross-update between the 1m and 4m versions once you are updated to a version that supports both 1m and 4m.


There are a few different ways to figure out if a given switch has 1MB or 4MB of flash memory.

### Binary Sensor

Each 4MB Switch will come with a binary sensor called `Kauf RGB Sw 4MiB` that indicates whether the running firmware was intended for a 1MB switch or a 4MB switch.  By default, this entity is not enabled in Home Assistant but can be viewed in the switch's web interface.  In the below screenshot, the binary sensor indicates a 4MB device.  If the binary sensor were OFF, a 1MB device would be indicated.

![image](https://github.com/KaufHA/kauf-rgb-switch/assets/89616381/fac0457a-455d-42c7-bd5a-640e5f949d18)

- If this entity does not exist, the running firmware is older from before 4MB devices existed.  A 1MB device can be assumed.
- This entity makes an assumption based on the running firmware, but a device could be running the wrong firmware and therefore have a wrong binary sensor value.

### Update Filename Check

When uploading a binary update file through the web interface, the 4MB devices will refuse to apply an update if `-1m` is found in the filename, and the 1MB devices will refuse to apply an update if `-4m` is found in the filename.

Again, the firmware can only assume that it is running on proper hardware, so this check will not work if the wrong firmware was previously flashed.


### Hardware Differences

If you are unsure whether you previously flashed the wrong firmware and want to confirm whether your device is the 1MB or 4MB hardware version, there are physical hardware differences that can be observed.

The 4MB hardware version indicates (4m) next to the model number on the back of the switch.  This can be unhelpful once the switch has been installed.

You can also pop off the white switch frame that holds the physical button and observe the PCB layout.  The 1MB version has a rectangular button at the bottom while the 4MB version has a circular button.  The below image demonstrates the differences.

![1mb-4mb switch comparison](https://github.com/KaufHA/kauf-rgb-switch/assets/89616381/054b52dd-c1f8-4e55-b94e-0e694b7b6b9f)
