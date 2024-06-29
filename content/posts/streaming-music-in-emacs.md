+++
title = "Wins and Fails in Streaming Music with Emacs"
date = 2024-06-29T09:30:00-05:00
draft = false
+++

## Emacs Multimedia System (EMMS) {#emacs-multimedia-system--emms}

My preferred media player in Emacs is [EMMS](https://www.gnu.org/software/emms/). Think of EMMS as a stackable playlist media player. EMMS out of the box does not actually play music. It relies on a "external" player like MPV, VLC, or MPLAYER. This is where this journey takes an interesting turn. Under Linux my EMMS setup is pretty straightforward. Below is my config:

```text
(emms-all)
(emms-standard)
(emms-default-players)
(setq emms-player-list '(emms-player-vlc)
      emms-info-functions '(emms-info-native)
      emms-show-format "Playing: %s")
```

I call EMMS, state I want to use EMMS standard list of players and include VLC in the list of players installed on my system. And setup some basic information for the EMMS player. After this I have to setup a method of streaming some music. This is also easy on Linux, it looks like so:

```text
(defun play-defcon-radio ()
   "Play defcon Radio"
   (interactive)
   (emms-play-streamlist "https://somafm.com/defcon.pls"))

(defun play-freecodecamp-radio ()
"Play freecodecamp radio - Low"
 (interactive)
 (emms-play-url "https://coderadio-admin-v2.freecodecamp.org/listen/coderadio/low.mp3"))

(defun play-tilde-radio()
  "Play Tilde Radio"
  (interactive)
  (emms-play-url "https://azuracast.tilderadio.org/radio/8000/radio.ogg"))

(defun play-kmfa-radio()
  "Play KMFA Radio"
  (interactive)
  (emms-play-url "https://kmfa.streamguys1.com/KMFA-mp3"))
```

With a simple Elisp function I wrap the `emms-play-url` function to the online radio station I want to listen. Here is a clip of how it all works

{{< figure src="/images/emms-jamming.gif" caption="<span class=\"figure-number\">Figure 1: </span>Calling Defcon Radio from EMMS." >}}

Again under Linux this works out of the box. But what about other operating systems like Windows?


## EMMS in a Windows World {#emms-in-a-windows-world}

My day to day workstation at work is a Windows 11 destkop. Obviously, I use Emacs because Emacs is my go to tool for my work in Cybersecurity. But Windows is not Linux and Emacs works very differently under Windows. I always find weird little quirks and hacks that I never need to do under Linux. So how did I achieve streaming music glory with EMMS in Windows?

This took forever to simply play correctly. I ran into issues where Emacs was prepending the HOME path to url variables in `emms-play-url`. It was super weird and frustrating issue that I kept running into. I looked at forums, blogs, and even asked multiple LLMs for help but I still came up short. Ultimately, I knew a few things:

1.  This configuration worked on Linux.
2.  VLC acts differently on Windows.

I went down a wild goose chase trying to get an Elisp function to strip down the passed variable to the url alone. You see on Windows my configuration has be setup from the `appdata` folder. In there I set my HOME environment variable. It makes using emacs so much easier.

But for some weird reason (probably because I didn't RTFM), EMMS kept prepending my HOME path to any url. This meant that every time I played a URL it would appear like so

`C:/myhome/https://musicsite.com/song.mp3`

It did not matter if the url was hard-coded or not, it would always be prepended with my HOME path. So a quick hack was to set `emms-source-file-default-directory nil`. This solved the prepend issue but EMMS would throw an error that I still did not know how to play the "track". I had seen this error before but ignored it because I thought it was related to the prepend issue.

I did more searching and found a stackoverflow [post](https://stackoverflow.com/questions/9147823/emms-error-dont-know-how-to-play-track) titled with the error I was getting. I took a look and they basically concluded that EMMS did not know where the player was located. I verified the value via the "Customize Emacs" option for EMMS. It simply showed `vlc` which I knew immediately was wrong. Windows is weird and it needs the full path and file extension. I simply added  `emms-player-vlc-command-name "C:/Program Files/VideoLAN/VLC/vlc.exe"` the default command line options passed are `--intf=rc`. The command line options open a VLC window which shows some basic information. This does not happen in my Linux machine so it seems that this is a Windows quirk. I had to tweak the options that get passed in command line on my Windows Machine. After some tweaking and searching I found the solution to running VLC in the background with no GUI.

`emms-player-vlc-parameters '("-I dummy" "--dummy-quiet")`

This can allow EMMS to control VLC and it runs in the background with no GUI. I also had a hard time getting a url with PLS extension to work. For example the Defcon Radio on [SomaFM](https://somafm.com/support/) is `https://somafm.com/defcon.pls` but adding that to `emms-play-streamlist` would not work. I kept getting an error that the track information could not be found. _Super weird_.

In the end I was lucky that SomaFM provides an icecast server which I could tap into. So I simply added that to my Elisp function and it worked. In the end I learned a few things. It's rare and special when configurations work equally between operating systems.

As you can see my Windows config is finickier. It's not necessarily a bad thing that Windows requires more configuration. I just have to be more disciplined about my config.

```text
(emms-all)
(emms-standard)
(emms-default-players)
(setq emms-player-list '(emms-player-vlc)
      emms-player-vlc-command-name "C:/Program Files/VideoLAN/VLC/vlc.exe"
      emms-player-vlc-parameters '("-I dummy" "--dummy-quiet")
      emms-player-vlc-playlist-command-name "C:/Program Files/VideoLAN/VLC/vlc.exe"
      emms-player-vlc-playlist-parameters '("-I dummy" "--dummy-quiet")
      emms-info-functions '(emms-info-native)
      emms-show-format "Jamming: %s"
      )
```

And finally, steaming joy!

```text
 (defun play-freecodecamp-radio ()
   "Play Freecodecamp Radio"
   (interactive)
   (emms-play-url "https://coderadio-admin-v2.freecodecamp.org/listen/coderadio/low.mp3"))

(defun play-defcon-radio ()
  "Play Defcon Radio"
  (interactive)
  (emms-play-url "https://ice4.somafm.com/defcon-256-mp3"))
```


### Thank You {#thank-you}

> If you enjoyed or found any of the content on my site helpful, you can buy me a cup of coffee or send some bitcoin  ⚡ so I can continue to bring you amazing content for free!


#### You can Buy Me A Coffee {#you-can-buy-me-a-coffee}

{{< figure src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" link="https://www.buymeacoffee.com/eduardorobles" >}}


#### Tip with some Sats {#tip-with-some-sats}

[Tip Some Sats ⚡](https://getalby.com/p/tacosandlinux)


### Setup {#setup}

-   Keyboard: ANNE Pro 2 with Kailh Box White switches
-   Mouse: MX Master (Original)
-   Computer: Framework 13 (Fedora Linux)
