# Disable radios

You might want to disable totally the radios from your raspeberry (wifi, bluetooth), to reduce consumption and possible interference.

## Using rfkill

    sudo rfkill block wifi

## Modifying boot

Edit `/boot/config.txt`, and in `[all]' add the following:

    dtoverlay=disable-wifi
    dtoverlay=disable-bt

## Resource

* https://pimylifeup.com/raspberry-pi-disable-wifi/