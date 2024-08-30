# Microbit

I had trouble connecting to Microbit from Linux. I found out that in order to successfully pair with a Microbit using a cable you need to:

* Use a data cable instead of a simple charging cable
* Have to use a browser that supports WebUSB (Chrome and Edge above a version)
* The browser must not be sandboxed (snap/flatpak), else you need to provide further permissions
* User must have the appropriate permissions on the device

## Permissions in device

### Group membership

Typically, microbit is loaded as a device of the form `/dev/ttyACM0` or `/dev/ttyUSB0`. These devices are typically created with `660` permissions, so a simple user might not have access to them. Nevertheless, if you can get membership on the group you will be able to work with the device, as the middle `6` denotes that users of the group have full access. The group is usually the `dialout` or the `tty`. You can just `ls -l /dev/ttyACM0` to find out the actual one. Then just `sudo usermod -aG dialout your_username`, and log out and in again (or better restart).

### udev rule

If the above does not work you might want to bring out the big guns and intevene on the udev subsystem using a rule. 
The device is mounted on modern linux systems through the `udev` device manager. In order to ensure appropriate permissions add the following rule in `/etc/udev/rules.d/` directory in a file named `microbit.rules`

```simple
SUBSYSTEM=="usb", ATTR{idVendor}=="0d28", ATTR{idProduct}=="0204", MODE="0666"
```

Then reload the rules, restart browser and enjoy.

```sudo
sudo udevadm control --reload-rules
```

 Alternatively, restart Linux.

 Information found [here](https://mattoppenheim.com/2018/06/24/using-udev-to-remove-the-need-for-sudo-with-the-bbc-microbit/).

## Serial console

Find which device node the micro:bit was assigned to with the command 

```command
ls /dev/ttyACM*
```

If it was `/dev/ttyACM0`, type the command

```command
screen /dev/ttyACM0 115200
```

While attached to the serial console you might not be able to upload a new program in the microbit. Kill the screen session by `ctrl`+`a`,`k`.

