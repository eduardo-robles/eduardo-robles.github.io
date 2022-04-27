+++
title = "SSH Config File - Make SSH Easier"
date = 2022-04-27T07:30:00-05:00
draft = false
+++

How do you stay organized with SSH connections? Most of use simply search our terminals history to find SSH connections. You may do `history | grep "ssh"` or even `Ctrl+R` and search SSH. While that may work for a few connections, there is a better way: SSH Config file. An SSH Config file simply tells OpenSSH how to open up connections. The benefit is that you can keep all your connections all in one place. Plus you can use things like Jumphosts and Public Keys to make connections easier. Let's look at a typical SSH command.

`ssh erobles@10.0.3.11 -p 2300 -i ~/.ssh/mykeys`


## `erobles@10.0.3.11` this states our username on the server and the IP/Hostname of the server {#erobles-10-dot-0-dot-3-dot-11-this-states-our-username-on-the-server-and-the-ip-hostname-of-the-server}


## `-p 2300` the port we are connecting to on the server {#p-2300-the-port-we-are-connecting-to-on-the-server}


## `-i ~/.ssh/mykeys` the Public/Private keys used in the SSH connection {#i-dot-ssh-mykeys-the-public-private-keys-used-in-the-ssh-connection}

While this is fine, it can be time consuming and easily forgotten. So let's see how this commands translates to in an SSH Config file.

```bash
HOST myserver
  HostName 10.0.3.11
  User erobles
  Port 2300
  IdentityFile ~/.ssh/mykeys
```

The example above achieves the same as the long SSH command in the previous example. You can save this file in `~/.ssh./` directory with the filename `ssh_config`. Once the file is saved you can simple type `ssh myserver`, OpenSSH will check the SSH config file for an entry anme `myserver` and execute an SSH connections with the options you specify. As you have more servers/machines you have to SSH into you simply add those the SSH config file. You can have 20, 40, or 100 connections all in one file! Working with an SSH Config file makes your SSH workflow much easier. It can also be helpful to keep track of SSH connectitons.

> If you enjoyed or found any of the content on my site helpful, you can buy me a cup of coffee so I can continue to bring you amazing content for free!
>
> [Buy Me a Coffee](https://www.buymeacoffee.com/eduardorobles)
