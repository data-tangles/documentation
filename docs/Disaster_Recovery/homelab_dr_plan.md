# Homelab DR Plan

!!! warning 

    The following Disaster Recovery plan is specific to my homelab environment. These steps should not be used for your environment however you can extract certain steps from it in order to formulate your own.

## Networking

| Hardware              | Description                          |
| --------------------- | ------------------------------------ |
| Ubiquiti EdgeRouter X | Firewall                             |
| Netgear S350 GS308T   | Core Switch                          |
| TP-Link WA801N        | Primary Wireless Access Point        | 
| D-Link DIR-825        | IoT Wireless Access Point            | 

### Restore Steps

#### Ubiquiti EdgeRouter X

1. Connect device to eth0
2. Restore config backup
3. Connect device as per table below

| Port | Device |
| ---- | ------ |
| eth0 | WAN    |                        
| eth1 | N/A    | 
| eth2 | LAN    | 
| eth3 | N/A    |
| eth4 | N/A    |       

#### Netgear S350 GS308T 

1. Connect device to Port 1
2. Restore config backup
3. Connect device as per table below

| Port | Device                 |
| ---- | ---------------------- |
| 1    | ESXI Host 1            |                
| 2    | ESXI Host 2            | 
| 3    | N/A                    | 
| 4    | N/A                    |
| 5    | TP-Link WA801N         | 
| 6    | Main PC                | 
| 7    | D-Link DIR-825         | 
| 8    | Ubiquiti EdgeRouter X  |  

#### TP-Link WA801N

1. Connect device to port
2. Restore config backup

#### D-Link DIR-825

1. Connect device to port
2. Restore config backup
3. Connect device as per table below

| Port | Device                 |
| ---- | ---------------------- |
| WAN  | N/A                    |                
| 1    | Netgear S350 GS308T    | 
| 2    | N/A                    | 
| 3    | N/A                    |
| 4    | N/A                    | 

### Network

``` mermaid
graph TD
 linkStyle default interpolate basis
 wan[<center>Fibre 75/75 Mb<br>]---router{<center>Ubiquiti EdgeRouter X<br>}
 router---|1Gb|switch[<center>Netgear S350 GS308T<br>]
 subgraph switching[" "]
 switch---|Port 1|esxi-host-01(<center>ESXI Host 01<br>)
 switch---|Port 2|esxi-host-02(<center>ESXI Host 02<br>)
 switch---|Port 5|tp-link-wa801n(<center>TP-Link WA801N<br>)
 switch---|Port 6|main-pc(<center>Main PC<br>)
 switch---|Port 7|d-link-dir-825(<center>D-Link DIR-825<br>)
 switch---|Port 8|ubiquiti-edgerouter-x(<center>Ubiquiti EdgeRouter X<br>)
 end
 subgraph wifi-5Ghz[" "]
 d-link-dir-825---vlan-iot-5ghz(<center>VLAN 20 - IoT - 5Ghz<br>)
 end
 subgraph wifi[" "]
 tp-link-wa801n---vlan-lab(<center>VLAN 10 - Lab<br>)
 tp-link-wa801n---vlan-iot(<center>VLAN 20 - IoT<br>)
 tp-link-wa801n---vlan-guest(<center>VLAN 30 - Guest<br>)
 end
```

## Servers

### Hardware

Currently utilising the following hardware for ESXI Hosts: 

| Model| CPU | RAM | Storage | ESXI Version |
| ---- | --- | --- | ------- | ------------ |
| Dell Optiplex 7050 | Intel Core i5 7500 | 32 GB | 3x 240 GB SSD | ESXI 8.0 U2 |                
| HP EliteDesk 800 G1 SFF | Intel Core i7 4770 | 32 GB | 3x 240 GB SSD | ESXI 8.0 U2 |

### Virtual Machines

#### Restore Steps 

##### Backups

Using Veeam Backup & Replication for backup and restore capabilities. The following steps would need to be taken to get Virtual Machines restored

1. Ensure ESXI Hosts have been provisioned
2. Install Veeam Backup & Replication 12 or above on a Windows Client or Server OS
3. Connect backup media to temporary restore machine
4. Utilise Encryption key from BitWarden or KeePass databases to decrypt Veeam Configuration Backup
5. Map restore media to jobs once configuration backup has been restored
6. Initiate Full VM restores of all Virtual Machines to relevant ESXI Host