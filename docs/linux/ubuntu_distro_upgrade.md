# Ubuntu Distribution Upgrade

Upgrading Ubuntu from one LTS version to the next is a fairly simple process in principal. 

!!! warning 

    Please make sure to test this on a non-critical system or take backups prior to upgrading your system!

## Update system packages 

```
sudo apt update && sudo apt upgrade 
```

## Install Ubuntu update tool 

```
sudo apt install update-manager-core
```

# Run system upgrade

```
sudo do-release-upgrade
```

# Reboot system 

```
sudo reboot 
```

You can now verify the upgrades and rollback if any issues are encountered.


