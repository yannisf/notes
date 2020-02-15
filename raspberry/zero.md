# Raspberry PI Zero (W)

This small Raspberry has a couple of distinctive features. It requires a mini HDMI cable, and has only USB OTG. Provided
that these two components might be something one does not have readily available, accessing the Raspeberry for the first
time might be a challenge. To work around this, the suggested option is to preconfigure the micro SD card to connect to
your wireless network and enable SSH.

### Configuring WIFI

Mount the microSD card on a PC, and create in the boot partition a file named `wpa_supplicant.conf`. Alternatively, edit `/etc/wpa_supplicant/wpa_supplicant.conf`.

The file should have the following content (update ssid and psk accordingly). To add network info you need to create a second text file called wpa_supplicant.conf and place that in the root of the boot SD too.

```hocoon
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="NETWORK-NAME"
    psk="NETWORK-PASSWORD"
}
```

To setup multiple networks:

```hocoon
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="network1"
    psk="network1_passwd"
    id_str="network1"
}

network={
    ssid="network2"
    psk="network2_passwd"
    id_str="network2"
}
```

Further information [here](https://raspberrypi.stackexchange.com/questions/11631/how-to-setup-multiple-wifi-networks).

### Enabling SSH

Mount the microSD card on a PC and create an empty file in the **boot** directory called `ssh`.

## Useful apt packages

ipython3
python3-pip
python3-picamera
sense-hat
rpi.gpio

## Reducing power consumption

### Disabling LED

You might want to disable the ON led from your RPI Zero to reduce power consumption even more. To this end add (or change)
the following in your `config.txt`.

 ```ini
dtparam=act_led_trigger=none
dtparam=act_led_activelow=on
```

**Source**: https://www.jeffgeerling.com/blogs/jeff-geerling/controlling-pwr-act-leds-raspberry-pi

### Disabling HDMI

Disabling HDMI reduces power consumption. If you don't needed add the following in your `config.txt`:

```ini
hdmi_blanking=2
```

**Source**: https://raspberrypi.stackexchange.com/questions/79728/keep-hdmi-off-on-boot*

### Underclock CPU

The default frequency for Raspberry Zero (at least for Zero W as of 2019) is 1000MHz, but
can be reduced to 700MHz which is the lowest possible setting. Edit `config.txt` and add
or change the following to:

```ini
arm_freq=700
```

The setting can be checked during normal operation with:

```sh
$ sudo cat /sys/devices/system/cpu/cpu0/cpufreq/cpuinfo_cur_freq
700000
```

## Installing Java

Java is not available in the latest Raspian repositories for the Raspberry Pi Zero. To this end a different distribution has to be downloaded. Since Oracle and AdoptOpenJDK do not provide such a build, Zulu is the next best choice. Download it from https://www.azul.com/downloads/zulu-community/?&version=java-8-lts&architecture=arm-32-bit-hf&package=jdk.

## Sources

- [Official documentation](https://github.com/raspberrypi/documentation/tree/master/configuration)
