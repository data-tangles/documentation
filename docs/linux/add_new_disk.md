# Add disk to Virtual Machine

Firstly add the new disk via your virtualisation platform and reboot the VM. 

Login to the VM and run `fdisk -l` and note down the new disk name (normally `/dev/sdb` if the VM only has one disk to begin with)

Run `cfdisk /dev/sdb` to create a new partition on the disk

## Create ext4 partition 

Run the `mkfs.ext4` command against the new disk while specifying the partition number 

```
mkfs.ext4 /dev/sdb1
```

## Create mountpoint

Create a new mountpoint directory for the partition in the `/mnt` directory

```
mkdir /mnt/new-volume
```

# Mount volume on mountpoint

```
mount /dev/sdb1 /mnt/new-volume
```

You can now browse your new volume at the created mountpoint 

# Make mountpoint persistent 

To make the new mountpoint persist across reboots, modify the `/etc/fstab` file and add the following entry to the file

```
/dev/sdb1 /mnt/new-volume ext4 defaults 0 0
```