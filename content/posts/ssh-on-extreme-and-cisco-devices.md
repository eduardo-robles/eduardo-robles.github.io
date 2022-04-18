+++
title = "SSH on Extreme and Cisco Devices"
date = 2022-04-18T06:58:00-05:00
draft = false
+++
# Enable SSH on Extreme Devices

A key will be generated. To upload a public key simply use `sftp` or
`scp` to upload key. But be sure to change the extension to `.ssh` for
example `id_rsa.pub` will be `id_rsa.ssh` on the switch. Also you can
assign a key to a user by simply appending the username to the key file
for example `admin.id_rsa.pub`.

Extreme switches have a limitation of only being able to use RSA or DSA
keys. Recommend to use RSA 2048

## Enabling SSH on an Extreme Switch

`enable ssh2`

## Chaning SSH port

`enable ssh2 port tcp 766`

## Enable SSH on VR-Mgmt Only

`enable ssh2 vr VR-Mgmt`

# Enable SSH On Cisco Devices

## Add hostname to the device

`ip domain-name ex.cisco.com`

## Generate SSH Key for device

`crypto key generate rsa`

## Chose SSH Key size

Default is `512` but `1024` is better

## Change SSH version

`ssh version 2`

## Add a username and password for SSH access

`username admin secret admin123`

## Configure the lines which will have SSH access

`line vty 0 15` or `line vty 0 2`

## Enable SSH on enable lines

`transport input ssh`

## Keep SSH to local logins

`login local`

## Save config

`copy run start`
