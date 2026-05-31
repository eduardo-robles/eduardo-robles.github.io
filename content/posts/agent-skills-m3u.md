+++
title = "Building Agent Skills - M3U Playlist Generator"
publishDate = 2026-05-30
draft = false
+++

## The need for a M3U Generator {#the-need-for-a-m3u-generator}

I wanted to demonstrate a situation in which an AI Coding Agent helped me solve a specific problem. I have a unique music playback setup with Emacs and MPV. I use a package called EMMS in Emacs to act as the frontend client I wrote about it [here](https://www.eduardorobles.com/posts/streaming-music-in-emacs/). Recently I have been trying build simple playlists to match certain moods and situations while I'm working.

With EMMS in Emacs you can have a playlist in an M3U file. The M3U file is a relatively straightforward format but I was writing them by hand. So it occured to me, why not have an AI Coding Agent create something for me?


## Building an M3U Generator {#building-an-m3u-generator}

I started by planning my idea of an M3U Generator. I began by writing down (yes by hand on a sheet of paper 🤓) what I envisioned and some simple logic.

{{< figure src="/images/m3u-notes1.png" >}}

In case you can't read my handwritting, I am basically stating what I envision the workflow for generating the M3U playlists file would look like. I then continue on and write down the basic logic which the AI Coding Agent will build.

{{< figure src="/images/m3u-notes2.png" >}}

I outline the logic like so:

-   Find songs in a text file
-   Take songs and add M3U info
    -   set 0 to song length - easy cheat to not guess songs length
-   Name M3U file based on 1st line of text file
-   Save M3U file to root of playlist directory
-   If cannot save to remote directory save to local "music" directory

With this simple logic I wrote a prompt and asked Pi create a _Skill_.

{{< figure src="/images/m3u-notes3.png" >}}

I loaded another _Skill_ which assisted in creating the new _Skill_ I wanted. In my prompt I also asked the coding agent to first research and learn about the M3U format from Wikipedia. Then I gave it the simple logic I had written. In this example I updated the logic to work with _org-mode_ files instead of plaintext.

{{< figure src="/images/m3u-notes4.png" >}}

Pi wrote the skill and some python code to handle the heavy lifting. It even decided it needed to run a test for me. I tested the _Skill_ myself and it worked as expected.


## Learning from this experience {#learning-from-this-experience}

The _Skill_ worked but I still failed. Even with an extremely simple example the LLM failed to understand my **intent**. The outcome may have been sucessful but the "process" was not as I intended it. I never wanted Pi to write more files on disk that it needed to. In order for Pi to "successfully" complete the task it wrote new org-mode files and simply left them for me to clean up.

This taught me one thing Specs and Planning are crucial. Other AI Coding Agents solve this by baking in a "Plan" mode which may have caught this. But that is still leaving to much for the LLM to try and understand. To solve the issue from before I simply added extra instructions in my AGENT.md file.


## Conclusion {#conclusion}

In summary this was a great learning experience. It helped reinforce some of the ideas and practices that I have previously experienced. In order to progress from here I have to sharpen my teaching and planning abilities.


### Thank You {#thank-you}

> If you enjoyed or found any of the content on my site helpful, you can buy me a cup of coffee or send some bitcoin ⚡ so I can continue to bring you amazing content for free!


#### You can Buy Me A Coffee {#you-can-buy-me-a-coffee}

{{< figure src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" link="https://www.buymeacoffee.com/eduardorobles" >}}


#### Tip with some Sats {#tip-with-some-sats}

[Tip Some Sats ⚡](https://getalby.com/p/tacosandlinux)
