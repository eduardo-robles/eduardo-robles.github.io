+++
title = "My AI Agent Setup v2"
publishDate = 2026-05-24
draft = false
+++

## Pi + Emacs = 🤝 {#pi-plus-emacs}

In my last blog post I wrote about the need to integrate Pi into Emacs. Normally, this would be a trivial task since most AI Coding Agents have some straightforward integrations with Emacs. The issue I ran into was the majority of my daily applications and tools are in containers. I took the same idea I had for my [security containers](https://www.eduardorobles.com/posts/tool-stack-containers/) and resued it for Pi. Emacs TRAMP mode is extremely helpful when working inside of containers. But I needed to drive the coding assistant and for that I needed Emacs to understand where and how to access Pi inside a container.

Emacs is my editor of choice and luckily there are many great packages which allow Emacs to communicate and control various LLM tools. I started with GPTEL some time ago and used it for some time. Though an amazing package called [agent-shell](https://github.com/xenodium/agent-shell) popped up and I decided to give it a try. I needed an NPM package "pi-acp" to connect Pi to Agent Shell but that was relatively straighforward. The only confusing part of calling Pi in the container. In the end with the help of Gemini I was able to get it working by simply adding this to the Agent Shell configuration.

```emacs-lisp
(use-package agent-shell
   :ensure t)

 (setq agent-shell-command-prefix '("toolbox" "run" "--container" "dev-tools"))
 (setq agent-shell-pi-acp-command '("pi-acp"))
```

This uses the function `agent-shell-command-prefix` to call the container and communicate with the ACP (Agent Client Protocol) API. It worked and I was able to easily use Pi within Emacs.
![](/images/pi-emacs.png)

From here it functions just like any other coding agent. I can prompt it to work on some code or build specifications. Or I can simply call some of the workflows/playbooks that I am developing.


## Next steps {#next-steps}

This technology space is going through a rapid phase of experimentation and iteration. In 6 months AI Coding Agents might be replaced by some other tool. In the mean time this is a good setup for me. I will continue to experiment and play with these tools. Though I hope to in the future to rely on Emacs to be the main interface for these technologies. What I hope to build or use is a system which I open up Emacs and I can use/configure/orchestrate LLMs from Emacs. There's the funny joke that Emacs is a great operating system with no good text editor. There's no reason why I can't have Emacs be the operating system which the LLMs operate from and the editor be an interface to the LLMs or AI agents. This my ultimate backburner project and hopefully I can find/build a good setup.


### Thank You {#thank-you}

> If you enjoyed or found any of the content on my site helpful, you can buy me a cup of coffee or send some bitcoin  ⚡ so I can continue to bring you amazing content for free!


#### You can Buy Me A Coffee {#you-can-buy-me-a-coffee}

{{< figure src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" link="https://www.buymeacoffee.com/eduardorobles" >}}


#### Tip with some Sats {#tip-with-some-sats}

[Tip Some Sats ⚡](https://getalby.com/p/tacosandlinux)
