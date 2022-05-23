# YAML features for KAUF RGB Wall Switch

These files can be compiled with the main kauf-rgbs.yaml file to add features.


### kauf-rgbs-rotate-colors.yaml

Include this yaml file as a package along with kauf-rgbs.yaml.  You should have the following in your local yaml file.  kauf-rgbs-rotate-colors.yaml must be include before kauf-rgbs.yaml.
```
packages:
  rotate_colors: github://bkaufx/kauf-rgb-switch/yaml-features/kauf-rgbs-rotate-colors.yaml
  Kauf.RGBSw: github://KaufHA/kauf-rgb-switch/kauf-rgbs.yaml
```

