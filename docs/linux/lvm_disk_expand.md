# LVM Disk Expansion

Expand the required disk within your virtualisation platform 

## Run disk rescan

!!! warning 

    Replace `sda` with the physical identifier of the disk

```
echo 1>/sys/class/block/sda/device/rescan
```
Run `fdisk -l` to confirm the partition is at the required size, if not reboot the host

## Expand disk

Expand the disk within `cfdisk`

## Expand the LVM volume

!!! warning 

    Replace `sda1` with the physical identifier of the partition

```
sudo pvresize /dev/sda1
```

```
lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

```
resize2fs -p /dev/ubuntu-vg/ubuntu-lv
```

Confirm the partition has been successfully resized using `df -h`