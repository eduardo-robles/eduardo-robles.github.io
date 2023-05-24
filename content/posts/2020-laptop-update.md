+++
title = "My 2020 Ubuntu Laptop Setup"
publishDate = 2020-01-24
draft = false
+++

New Year, New Setup

Ubuntu 19.10 Desktop

I began the new year by buying a new 500GB SSD. My laptop had 2 drives: a 32GB SSD was my /root drive and a 120GB SSD was /home partition. This served me just well but obviously I would run out of space quickly if I was working with virtual machines. With a new drive I had to make the decision to start from scratch or use backups. I decided to start from scratch mainly because I wanted a clean and fast experience.
Operating System

Though I have used Pop!OS in the past this time around I decided to install Ubuntu 19.10. I have Ubuntu 19.10 installed on my desktop and I really enjoyed it’s speed and perfomance. Plus it helps to know that both my laptop and desktop are running the same OS and version. Other distro’s I considered were: Fedora, and Manjaro.
Theme

I recently came acros the Dracula theme for Emacs and I decided I needed this theme everywhere. Luckily you can go to <https://draculatheme.com/> and see all the theme options for a lot of apps.
Apps

This is a list of my go to apps.

> Emacs
> Spotify
> Evolution (Email client)
> Audacity
> Tizonia (Spotify terminal client)
> VLC
> Keybase

Other apps I install depending on the use case:

> VPN
> Audacity
> Open Broadcaster
> GNOME Tweaks
> Syncthing
> Chromium

Configurations

Ok, so let’s talk how I setup my laptop the quickest way possible.
Sign into my Google account in GNOME online accounts.

This is to have Evolution setup as soon as it’s installed and launched.
Run my setup scripts

I came across this great post by software dev Victoria Drake. She wrote a great bash script that she uses to setup her Ubuntu laptop (or even a VM). So I cloned it and modified it for my use. Here are some key take aways.

```sh
# Snap packages

sudo snap install spotify

sudo snap install chromium

sudo snap install tizonia

# GNOME
install gnome-tweaks

# File Backup
install deja-dup
install git
install curl

# add more apps as needed

This is the script that is called to install my apps. This is only an example, in the real world I edited the script to add or remove apps that I wanted installed or removed. Another part of my setup scripts is the desktop.sh script.

# Set GNOME Settings
gsettings set org.gnome.desktop.wm.preferences titlebar-font 'IBM Plex Sans Bold 11'
gsettings set org.gnome.desktop.interface monospace-font-name 'IBM Plex Mono 13'
gsettings set org.gnome.desktop.interface document-font-name 'IBM Plex Sans Medium 11'
gsettings set org.gnome.desktop.interface font-name 'IBM Plex Sans 11'
```

Ubuntu 19.10 Terminal Dracula Theme

I use this script to setup my fonts. It downloads IBM Plex font and installs it on my system. I love this font and thus I use it everywhere. My setup scripts do other things depending on what I want to do, like setup some PPA’s or change other GNOME settings.

One thing that I found after I setup my laptop was this great script to change the terminal theme. It’s called Gogh and you can find it here <https://github.com/Mayccoll/Gogh>.
GPG, Git, and Emacs setup

I do the basic GPG configurations, like download my GPG keys and setup my SSH keys. I also setup Git by adding SSH login, user name and email. Then I setup Emacs by downloading my configuration from my private repo. I set Emacs to run in daemon mode cause it’s faster than lighting this way :smile:. To run Emacs in daemon mode I simply run systemctl --user enable emacs.service and systemctl start emacs.service.

Emacs 26.3
And that’s it

The setup scripts do most of the grunt work. So I simply run them and a few minutes later all my apps and laptop is setup. After I do some post installation tweaks I’m ready to get to work in about 15 minutes. So I hope you all found this post insightful and useful. Some things that I didn’t discuss here but I did do were: I encrypted my drive on initial installation and I downloaded updates while I installed Ubuntu.

If you enjoyed or found any of the content on my site helpful, you can buy me a cup of coffee so I can continue to bring you amazing content for free!


## Thank You {#support}

{{< figure src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" link="https://www.buymeacoffee.com/eduardorobles" >}}
