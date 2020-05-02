# LUKS

LUKS is a full disk encryption solution for Linux. The conceptual model of using an encrypted disk, is:

- Format a device to use LUKS
- Unlock the device
- Create a filesystem on device (if one does not already exist) 
- Mount and use the encrypted filesystem

### Install 

    sudo apt install cryptsetup

### Initialize

    cryptsetup -y -v luksFormat /dev/sda1

### Open encrypted partition

    cryptsetup luksOpen /dev/sda1 usb_32_samsung

### Status 

    cryptsetup -v status usb_32_samsung

### Dump (device info)

    cryptsetup luksDump /dev/sda1

### Create filesystem

    mkfs.ext4 /dev/mapper/usb_32_samsung 

### Mount filesystem

    mount /dev/mapper/usb_32_samsung /mnt/

### Close encrypted partition

    cryptsetup luksClose usb_32_samsung

### Add password

    cryptsetup luksAddKey /dev/sda1

### Decrypt

    cryptsetup-reencrypt --decrypt /dev/sda1

