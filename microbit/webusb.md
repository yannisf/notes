# Microbit

I had trouble connecting to Microbit from Linux. I found out that in order to successfully pair with a Microbit using a cable you need to:

* Use a data cable instead of a simple charging cable
* Have to use a browser that supports WebUSB (Chrome and Edge above a version)
* The browser must not be sandboxed (snap/flatpak), else you need to provide further permissions
* User must have the appropriate permissions on the device

## Permissions in device

The device is mounted on modern linux systems through the `udev` device manager. In order to ensure appropriate permissions add the following rule in `/etc/udev/rules.d/` directory in a file named `microbit.rules`

```
SUBSYSTEM=="usb", ATTR{idVendor}=="0d28", ATTR{idProduct}=="0204", MODE="0666"
```

Then reload the rules, restart browser and enjoy.

```
sudo udevadm control --reload-rules
```

 Alternatively, restart Linux.

 Information found [here](https://mattoppenheim.com/2018/06/24/using-udev-to-remove-the-need-for-sudo-with-the-bbc-microbit/).

 