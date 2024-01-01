# Postfix Install 

The below steps will setup Postfix with Gmail SMTP to send emails. Gmail can be swapped out for your email provider of choice. In this tutorial we are using Ubuntu for the setup.

Firstly, update the system packages

## Update packages

```
sudo apt-get update && sudo apt-get upgrade 
```

## Install Postfix package 

```
sudo apt-get install postfix 
```

Select the `Internet Site` option 

## Generate Gmail App Password

Follow the guide located [here](https://support.google.com/mail/answer/185833?hl=en) to setup an App Password for your Gmail account

## Setup SASL

Create a SASL Auth file for Postfix 

```
vi /etc/postfix/sasl/sasl_credentials   
```
Add the Gmail app password to the file 

```
[smtp.gmail.com]:587 exampleuser@gmail.com:aodrkojoafdnyqyl 
```

## Create Hash Database file 

Run the following to create the file 

```
sudo postmap /etc/postfix/sasl/sasl_credentials
```

This will create a new file `/etc/postfix/sasl/sasl_credentials.db`

## Change permissions on database file

In order to protect the credentials of the file run the following

```
sudo chown root:root /etc/postfix/sasl/sasl_credentials /etc/postfix/sasl/sasl_credentials.db 
sudo chmod 0600 /etc/postfix/sasl/sasl_credentials /etc/postfix/sasl/sasl_credentials.db 
```

## Set postfix relay & SASL config

Modify the relay config in the `/etc/postfix/main.cf`

```
mydestination = $myhostname, localhost, localhost.localdomain 
relayhost = [smtp.gmail.com]:587 
mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 
```

Add the following to the bottom of `/etc/postfix/main.cf`

```
smtp_sasl_auth_enable = yes 
smtp_sasl_security_options = noanonymous 
smtp_sasl_password_maps = hash:/etc/postfix/sasl/sasl_credentials
smtp_tls_security_level = encrypt 
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt 
```

## Restart postfix 

`sudo systemctl restart postfix`

## Test email 

To test the postfix config using `sendmail` do the following

```
sendmail you@youremailaddress.com 
To: you@youremailaddress.com
Subject: Test Postfix Email
This is a test mail to test Postfix
```