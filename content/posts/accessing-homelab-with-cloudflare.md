+++
title = "Accessing my homelab with Cloudflare"
publishDate = 2021-07-18
draft = false
+++

Cloudflare Access for my Homelab

I decided to use Cloudflare to setup a Secure Web Gateway and establish some Zero Trust access for my homelab services. Cloudflare offers a great service called “Cloudflare Access”. Basically it leverages Cloudflare’s edge network to create secure web routing. Setting up this service is just a matter of running a simple daemon. Once configured you setup Cloudflare DNS to route traffic. Let’s discuss how I setup Cloudflare Access.
Create an SSH Bastion with Cloudflared
Setup a Raspberry Pi with Raspberry Pi OS or Ubuntu

Install Cloudflared
    Ubuntu/Debian install

```sh
wget -q https://bin.equinox.io/c/VdrWdbjqyF/cloudflared-stable-linux-amd64.deb
dpkg -i cloudflared-stable-linux-amd64.deb
```

Raspberry Pi

```sh
wget -q https://bin.equinox.io/c/VdrWdbjqyF/cloudflared-stable-linux-arm.tgz
tar -xyzf cloudflared-stable-linux-arm.tgz
sudo cp ./cloudflared /usr/local/bin
sudo chmod +x /usr/local/bin/cloudflared
cloudflared -v
```

Create a tunnel with Cloudflared

cloudflared tunnel login A browser window will open asking for authentication from Cloudflare.
Setup a “Self-hosted App” on Cloudflare Teams.

See this <https://developers.cloudflare.com/cloudflare-one/applications/configure-apps/self-hosted-apps>
Configure tunnel on Raspberry Pi (or jump host)
    Find tunnel Id

```sh
cloudflared tunnel list
```

Create/Edit Cloudflared Configurations
    location: `/home/pi/.cloudflared/config.yml`
tunnel: TUNNEL\_ID\_GOES\_HERE
credentials-file: `/home/pi/.cloudflared/TUNNEL_ID.json`

```text
ingress:
  - hostname: rterm.eduardorobles.com
    service: ssh://localhost:22
  - service: http_status:404
```

Execute the tunnel

```sh
cloudflared tunnel run TUNNEL_NAME
```

Route DNS for tunnel

    cloudflared tunnel route dns TUNNEL\_ID rterm.eduardorobles.com
Access Raspberry Pi (or jump host)
    In browser go to <https://rterm.eduardorobles.com>
    Go through the login steps and you should be able to login to your jump host
Connect from a client machine
    Install Cloudflared
    Configure SSH Config

```sh
Host rterm.eduardorobles.com
  ProxyCommand /usr/local/bin/cloudflared access ssh --hostname %h
```

Adding another service
    Add settings to config.yml file
	Delete old config file /etc/cloudflared/config.yml
	    Install service again

Accessing All of my Services

If you followed along you can see that in the last step we can add multiple ingress rules. For each service you want to router traffic to simply add it your configurations. In the example above I setup SSH access to my Raspberry Pi. Cloudflare can even render the SSH session in the browser for you.

rendering an SSH session in the browser

You can setup another machine with SSH to proxy your connection. But adding multiple ingress points allows you to access any and all of your services. Since you are using a Secure Web Gateway, your services are not automatically open on the internet.

I also a Zero Trust Policy was setup which allows for very locked down sites. I setup 2 Factor Authentication for my Web Gateway. In the end I feel happy with the results and recommend anyone try Cloudflare Access.
