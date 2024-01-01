# SNMP Setup on Ubuntu

This guide will showcase installing SNMP on an Ubuntu system.

## Update system packages 

`sudo apt-get update && sudo apt-get upgrade` 

## Install SNMP package 

`sudo apt-get install snmpd`

## Configure agent listener 

Open `/etc/snmp/snmpd.conf` in the editor of your choice and edit the `agentAddress` section. Also remove the `#` in front for it to take effect

``` title="Listen on all addresses"
agentAddress udp:161,udp6:[::1]:161
```

``` title="Listen on specific address"
agentAddress udp:192.168.100.2:161
```

## Configure community string

Open `/etc/snmp/snmpd.conf` in the editor of your choice and edit the `rocommunity` section.

set `public` to an alternative community name if needed

## Restart SNMP Service 

```
sudo service snmpd restart
```


