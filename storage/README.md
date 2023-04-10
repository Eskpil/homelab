# Configuring storage

I just use a bunch of SSD drives or HDD drives i have lying around.

## Configuring disks to have unique names

Here is my configuration for giving all my drives unique names. This is
necessary when passing the drives through to a VM.

```
# file is /etc/udev/rules.d/60-my-disks.rules

KERNEL=="sd*", ENV{ID_SERIAL}=="<serial_number>", SYMLINK+="disk/by-serial/<serial_number>"
```

The KERNEL=="sd*" would ensure this only applies to block devices, such
as SSD's and HDD's. We would then replace the <serial_number> with the
actual serial number of the drive to uniquely identify it through each
boot/reboot.

Getting the serial numbers of our drives

```shell
lsblk -o NAME,SERIAL
```

To apply our udev rule simply run

```shell
sudo udevadm control --reload-rules
```

### Example 

This is an example of how I configured my disks which are going to be
used for a NAS. I am running my NAS solution in a virtual machine. The
solution I am using is TrueNAS (Core)

```
# CVTS441502DH240JGN   INTEL SSDSC2BF240A5L (220GB)
KERNEL=="sd*", ENV{ID_SERIAL}=="CVTS441502DH240JGN", SYMLINK+="disk/by-serial/CVTS441502DH240JGN"

# S32LNWAH976090R      Samsung SSD 750 EVO (250GB)
KERNEL=="sd*", ENV{ID_SERIAL}=="S32LNWAH976090R", SYMLINK+="disk/by-serial/S32LNWAH976090R"

# S6PYNL0TB25896L      Samsung SSD 870 EVO (500GB)
KERNEL=="sd*", ENV{ID_SERIAL}=="S6PYNL0TB25896L", SYMLINK+="disk/by-serial/S6PYNL0TB25896L"

# PNY41180029600101FF8 PNY CS900 SSD (120GB)
KERNEL=="sd*", ENV{ID_SERIAL}=="PNY41180029600101FF8", SYMLINK+="disk/by-serial/PNY41180029600101FF8"
```
