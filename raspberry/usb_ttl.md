# USB to TTL

## Preparation

### `/boot/config.txt`

```
[all]
dtparam=uart0_console
enable_uart=1
```

**Note**: For raspberry pi 5 you need to force the UART console back to GPIO 14/15 with `dtparam=uart0_console`

### Create user

For headless setups precreate the user. Create `userconf.txt` in the boot partition with this format:

    username:password

Password should be the output of:

    echo 'mypassword' | openssl passwd -6 -stdin


### Connect Pins

Outside GPIO pin row:

```
3 -> Black
4 -> White
5 -> Green
```

## Connect to console

    sudo screen /dev/ttyUSB0 115200


