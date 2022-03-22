+++
title = "Adding SSH Key To Agent"
date = 2022-03-22T08:11:00-05:00
draft = false
+++

## Check if SSH Agent is running {#check-if-ssh-agent-is-running}

This is to add the keys to the SSH Agent

```sh
eval "$(ssh-agent -s)"
```


## Add the Keys to SSH Agent {#add-the-keys-to-ssh-agent}

```sh
ssh-add ~/.ssh/nameofkey
```


## Verify Keys Added to SSH Agent {#verify-keys-added-to-ssh-agent}

```sh
ssh-add -l
```


## Copy Key to Remote Server {#copy-key-to-remote-server}

```sh
ssh-copy-id user@remote.server.location
```


## Copy Server Key to Host {#copy-server-key-to-host}

```sh
ssh-copy-id user@host.local
```
