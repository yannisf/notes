# Kodi

Kodi is a media player that can be used with RaspberryPi's.
There are dedicated distributions, but can also be installed as an optional package on top of Debian.
When using a dedicated distribution, Kodi starts on boot while in the latter case it has to be started interactively.

I do not want to have a dedicated distribution  for Kodi, since this underutilizes the RaspberryPi resources.
Since I will have a device always on, it might as well do something useful in the background, such as: run a mumble server, a minecraft server, a Kafka service, a wireguard VPN endpoint etc.

So I chose to run Kodi on Debian. This entails of course a couple of more things one has to address.

## Install

This is the easiest part. Just run the following:

    $ sudo apt install kodi


## Autostart

First create an unprivileged user to run kodi with:

    $ sudo useradd -m -s /bin/false kodi

Then, add the user to the `video` group:

    $ sudo usermod -a -G video kodi

Schedule Kodi to autorun on startup using `cron`:

    $ sudo crontab -e -u kodi

Add the following line:

    @reboot kodi --standalone

## Considerations

Having a lot of services running on the same host,
significantly increases the attack vector.
For Kodi this was mitigated using a dedicated, unprivileged
user. Ensure that all services running on the system follow security best practices, install security updates as soon as possibe and setup a firewall.
