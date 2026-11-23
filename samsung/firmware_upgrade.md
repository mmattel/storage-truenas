# Upgrading the Samsung Firmware

Upgrading the firmware for Samsung NVMe drives is straightforward.\
You must boot Debian from eMMC as desribed in [Installing and Managing TrueNAS on Beelink ME Mini](../truenas/truenas.md) to proceed.

**You should not/can't do this from within TrueNAS to avoid any conflicting write access.**\
I have upgraded on my Debian based notebook while the OS was running without issues, but better safe than sorry...

## Updating Samsung 980/990 (PRO) NVMe Using the Linux CLI:

Get the latest firmware for your NVMe: `https://semiconductor.samsung.com/consumer-storage/support/tools/`, replace the image version in the next step accordingly.

1.  Download the image to your server\
    `wget -O /root/samsungFirmware.iso https://download.semiconductor.samsung.com/resources/software-resources/Samsung_SSD_990_PRO_8B2QJXD7.iso`
2.  Make sure all the required dependencies are installed\
    `sudo apt -y install gzip unzip wget cpio`
3.  Mount the iso and unpack initrd\
    `mkdir /mnt/iso`\
    `sudo mount -o loop /root/samsungFirmware.iso /mnt/iso/`\
    `mkdir /tmp/fwupdate`\
    `cd /tmp/fwupdate`\
    `gzip -dc /mnt/iso/initrd | cpio -idv --no-absolute-filenames`
4.  Run the firmware update tool fumagician\
    `cd root/fumagician/`\
    `sudo ./fumagician`\
    You will get asked for any drive to update
5.  Reboot the server to apply the firmware updates (boot Debian)\
    `sudo reboot`
6.  Cleanup\
    `rm -rf /tmp/fwupdate`\
    `rm -f /root/samsungFirmware.iso`
7.  Reboot (boot TrueNAS)\
    `sudo reboot`

Thats it, your Samsung PRO NVMe(s) are now up-to-date!

