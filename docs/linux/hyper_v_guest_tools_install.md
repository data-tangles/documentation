# Hyper-V Guest Tools install

In order for Linux Guest VM's to correctly integrate with Hyper-V the Guest tools should be installed. Generally this is installed by default if the VM was originally proviosioned
on Hyper-V.

If the VM was migrated over from another virtualization platform (eg: ESXI, Xen, QEMU etc) or a Physical Machine then the integration tools would need to be installed.

In order to install the Guest tools, login to the Linux VM and run the following code applicable to the Linux distribution:

``` title="Ubuntu"
sudo apt-get update
sudo apt-get install linux-image-virtual linux-tools-virtual linux-cloud-tools-virtual
```

``` title="Debian"
sudo apt-get install hyperv-daemons
```

For Centos, there are a few extra packages that can be installed for optimisation between the guest and host. These include the VSS daemon and File copy daemon. This can be installed by running the following code:

``` title="CentOS"
yum install hyperv-daemons
yum list installed | grep hyperv
```

You will then need to start the appropritate services:

```
systemctl enable hypervkvpd hypervvssd
systemctl start hypervkvpd hypervvssd
```
