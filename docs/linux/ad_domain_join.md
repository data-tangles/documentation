# Active Directory Domain Join

This guide assumes you have an existing Active Directory domain. This guide also assumes Debian based distributions.

## Install required packages 

```
sudo apt install sssd-ad sssd-tools realmd adcli
```

## Confirm domain discovery via DNS 

```
sudo realm -v discover ad1.example.com
```

You should see a result similar to the following if successful

```
 * Resolving: _ldap._tcp.ad1.example.com
 * Performing LDAP DSE lookup on: 10.51.0.5
 * Successfully discovered: ad1.example.com
ad1.example.com
  type: kerberos
  realm-name: AD1.EXAMPLE.COM
  domain-name: ad1.example.com
  configured: no
  server-software: active-directory
  client-software: sssd
  required-package: sssd-tools
  required-package: sssd
  required-package: libnss-sss
  required-package: libpam-sss
  required-package: adcli
  required-package: samba-common-bin
```

## Join device to domain

!!! note

    This `adcli` command is being used for domain join. This is to combat issues with Server 2025 Domain Controllers as per: https://gitlab.freedesktop.org/realmd/adcli/-/issues/40

```
adcli join -U yout_user@YOUR.REALM --domain-controller=your.dc.fqdn --verbose --ldap-passwd
```

## Configure SSSD

If the domain join operation was successful create a default SSSD configuration file at `/etc/sssd/sssd.conf` and make sure to `chmod 600` on the file once configured.

```
[sssd]
domains = ad1.example.com
config_file_version = 2
services = nss, pam
	
[domain/ad1.example.com]
default_shell = /bin/bash
krb5_store_password_if_offline = True
cache_credentials = True
krb5_realm = AD1.EXAMPLE.COM
realmd_tags = manages-system joined-with-adcli 
id_provider = ad
fallback_homedir = /home/%u@%d
ad_domain = ad1.example.com
use_fully_qualified_names = True
ldap_id_mapping = True
access_provider = ad
```

Restart the SSSD service for configuration settings to be applied.

```
sudo systemctl restart sssd
```

## Allow user home directory creation

This will allow any AD users to automatically create a new home directory upon logon.

```
sudo pam-auth-update --enable mkhomedir
```

## Testing setup

### Fetch AD User information

```
$ getent passwd john@ad1.example.com
john@ad1.example.com:*:1725801106:1725800513:John Smith:/home/john@ad1.example.com:/bin/bash
```

### Fetch AD Group membership information

```
$ groups john@ad1.example.com
john@ad1.example.com : domain users@ad1.example.com engineering@ad1.example.com
```
