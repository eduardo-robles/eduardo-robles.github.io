+++
title = "Local LLM Labs"
publishDate = 2025-05-21
draft = false
+++

## Local LLMs In Your Homelab {#local-llms-in-your-homelab}

Why experiment with LLM technology in the first place? Well simple because it's everywhere and huge tech companies will shove it our faces every chance they get! In all seriousness, it's actually never been easier to experiment with these Models even on low end hardware. Yes, you can experiment with LLMs by running them on a single CPU and decent RAM. Let me show you how I did it.


## Ollama and Podman {#ollama-and-podman}

To get started you need to understand a Large Language Model "LLM" is basically just a bunch files. These files are collection of the models training data, instructions, and prompts. There's a lot to what makes up an LLM that I will not cover in this post. But feel free to ask your favorite LLM about how the sausage is made.

Since an LLM is just a bunch of files it needs what is called an "Inference Server". This specialized piece of software basically takes all the files in an LLM an exposes them as an API that you can interact with. The Inference Server we will using is called [Ollama](https://ollama.com). Ollama is a great tool to start using LLMs locally since it does the heavylifting of downloading and running your LLMs. Let's get Ollama running on a cheap small form factor PC I have running in my office closet.


### First, get Podman installed on Debian(or whatever Linux you use) {#first-get-podman-installed-on-debian--or-whatever-linux-you-use}

Podman is a container that is drop-in replacement for Docker. If you want to use Docker, feel free.

```shell
sudo apt-get -y install podman
```


### Second, get Ollama running {#second-get-ollama-running}

We will run Ollama with Podman and kick it to the background with daemon flag enabled.

```shell
podman run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama docker.io./ollama/ollama
```


### Third, in the container run an LLM {#third-in-the-container-run-an-llm}

The previous command started the Ollama inference server for us and exposed an API on port `11434`. But now we need to actually have a model to use. We can get one by executing the `ollama run` command in the container like so:

```shell
podman exec -it ollama ollama run granite3.3:8b
```

In this example I am running the [IBM Granite 3.3](https://www.ibm.com/granite) 8 Billion Parameter model. Simply looks at the "Models" page on Ollama to see what is supported.


### Fourth, start using the LLM {#fourth-start-using-the-llm}

Now you have Ollama running an LLM for you, success! How you interact with the model is your choice. There are a million frontends you can download and use to interact with you local LLMs. But here's a quick example using Curl.

```shell
curl --location 'http://sff-in-closet:11434/v1/chat/completions' \
--header 'Content-Type: application/json' \
--data '{
  "messages": [
    {
      "content": "You are a helpful assistant.",
      "role": "system"
    },
    {
      "content": "What is the capital of France?",
      "role": "user"
    }
  ]
}'
```


## Ok, there is a catch.... {#ok-there-is-a-catch-dot-dot-dot-dot}

So if you followed along you will find that if the container stops so does your inference server. Your new AI best friend disappears without saying goodbye...But we can fix that!


### Systemd to the rescue! {#systemd-to-the-rescue}

Create a `systemd` service so you can have your Ollama container running without having to invoke Podman yourself.

```shell
podman generate systemd --new --name ollama ~/.config/systemd/user/ollama.service
```

If you get an error that the file cannot be created by sure to create all necessary folders in the path listed. Next, enable the systemd service and start it.

```shell
systemctl --user daemon-reload
systemctl --user enable ollama.service
systemctl --user start ollama.service
```


### Hey why am I low on disk space all of sudden?! {#hey-why-am-i-low-on-disk-space-all-of-sudden}

Yeah, the name "Large Language Model" is not a typo. These models can range between 4GB and up! So be sure to keep an eye on those model sizes and delete those you don't want anymore. This is why I chose to run my models on a seperate machine other than on my laptop. Here's the specs of the machine I run my LLMs on.

```shell
user@sff-in-closet:~$ screenfetch
         _,met$$$$$gg.           user@sff-in-closet
      ,g$$$$$$$$$$$$$$$P.        OS: Debian 12 bookworm
    ,g$$P""       """Y$$.".      Kernel: x86_64 Linux 6.1.0-33-amd64
   ,$$P'              `$$$.      Uptime: 12d 15h 32m
  ',$$P       ,ggs.     `$$b:    Packages: 492
  `d$$'     ,$P"'   .    $$$     Shell: bash 5.2.15
   $$P      d$'     ,    $$P     Disk: 22G / 469G (5%)
   $$:      $$.   -    ,d$$'     CPU: Intel Core i7-6700T @ 8x 3.6GHz [36.0°C]
   $$\;      Y$b._   _,d$P'      RAM: 846MiB / 23788MiB
   Y$$.    `.`"Y$$$$P"'
   `$$b      "-.__
    `Y$$
     `Y$$.
       `$$b.
         `Y$$b.
            `"Y$b._
                `""""
```

BTW, this is what the specs are at while running Ollama with 2 LLMs.


## Conclusion {#conclusion}

This was another awesome rabbit hole! This is awesome technology to play around with in my homelab. I honestly, don't even care that it's "slow". **It's enough for me**. Plus the benefits out weigh all the hardware limitations. I ensure that I am private and offline with my prompts.

Online LLM services will still continue to exist and I can still access them (aka duck.ai, _your welcome_). But I learned a bunch of new skills and I can even build on them with the technology I deployed.


### Thank You {#thank-you}

> If you enjoyed or found any of the content on my site helpful, you can buy me a cup of coffee or send some bitcoin  ⚡ so I can continue to bring you amazing content for free!


#### You can Buy Me A Coffee {#you-can-buy-me-a-coffee}

{{< figure src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" link="https://www.buymeacoffee.com/eduardorobles" >}}


#### Tip with some Sats {#tip-with-some-sats}

[Tip Some Sats ⚡](https://getalby.com/p/tacosandlinux)
