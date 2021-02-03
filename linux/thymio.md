# Thymio on Ubuntu

1. Install flatpak

    sudo apt install flatpak
2. Install the Software Flatpak plugin

    sudo apt install gnome-software-plugin-flatpak
3. Add the Flathub repository

    flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
4. Restart
5. To allow access to Thymio from your computer add the file:

    vi /etc/udev/rules.d/99-mobsya.rule
6. Add the following lines

    SUBSYSTEM=="usb", ATTRS{idVendor}=="0617", ATTRS{idProduct}=="000a", MODE="0666"
    SUBSYSTEM=="usb", ATTRS{idVendor}=="0617", ATTRS{idProduct}=="000c", MODE="0666"
7. Execute

    sudo udevadm control --reload-rules
8. Download and install Thymio Suite 2.0 for Linux from https://flathub.org/repo/appstream/org.mobsya.ThymioSuite.flatpakref
9. Download simulator maps from https://www.thymio.org/thymio-simulator/

## Resources
* https://flatpak.org/setup/Ubuntu/