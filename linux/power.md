# Miscelaneous power related controls

### Turbo ON/OFF (for Intel pstate)

_To off_
$ echo 1 | sudo tee /sys/devices/system/cpu/intel_pstate/no_turbo

_To on_
$ echo 0 | sudo tee /sys/devices/system/cpu/intel_pstate/no_turbo
