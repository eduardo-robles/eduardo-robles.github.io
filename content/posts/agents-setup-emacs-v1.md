+++
title = "My AI Agent Setup v1"
publishDate = 2026-05-23
draft = false
+++

## How I started - Opencode with Gemini {#how-i-started-opencode-with-gemini}

I had learned about AI agents early on but dismissed it since I had no real use for them at the time. This all changed with the introduction of Opencode and REMnux MCP in the last version of REMnux. I have been a user of REMnux and talked about it several times on this blog. Luckily, the learning curve of Opencode and REMnux MCP was steep enough to keep me interested in the technlogy.

I played around with using Gemini in my REMnux sandbox. I noticed it didn't make my analysis faster but it did save time. Especially, if I used it in parallel to run several jobs. That's when it clicked, I can use these agents to perform a task that a script or automation simply cannot give me. A script will simply give me data but what I was really looking for was a _structured response_. For a month I experimented with Opencode and REMnux and it was a great starting point.


## What I use now {#what-i-use-now}

Opencode was a great starting point and I highly recommend everyone give it a try. But the Youtube algorithm hype cycle decided I needed to learn about [Pi.Dev](https://pi.dev). Opencode is closely aligned in strategy and design to Claude Code. Which is fine but I have only used a few of the features and never fully explored everything in Opencode. Pi is different, it's barebones, stragely that's what makes it so effective. One of Pi's strongest features is the ability to extend itself. You can simply imagine how you want Pi to function and it can implement it.

With the recent release of Fedora 44 I decided I wanted to tweak my setup. I started placing the majority of my daily applications and tools in containers. Fedora luckily makes this extremely easy with the integration of Toolbox and Podman. I took the same idea I had for my [security containers](https://www.eduardorobles.com/posts/tool-stack-containers/) and resued it for Pi. Pi and several dev tools lived in a container which provided some level of sandboxing.

My workflow was straightforward. I would open the toolbox container, start up Pi and get to working. My work early on was to simply recreate some of my daily tasks. What I found the most interesting and frustrating is the ability for the LLM to **completely** missintrepret my intent. I was a pretty decent prompter before but working with agent tested my patience. So to fix this problem I went back to Youtube University and started learning everything I could about agents.

Soon I learned about "Spec Driven" development, "CONTEXT.md", model abilities like reasoning/thinking plus much more. This helped me wrap my brain around several concepts which improved my agent building.

1.  LLMs do not inherently understand Human idiosyncrasies.
2.  LLMs love to finish tasks, even if they are incorrect.
3.  LLMs have terrible memory.
4.  Iteration must be closely observed and thoughtful.

Funny enough I relied on my Anthropology degree to diagnose most of the problems with my agents. We believe this technology (LLMs) "thinks" but _thinking_ is a Human trait. In order to instill proper task or project completions you have to inject Human motivation and idiosyncratic understanding. Basically, "hey bot _this is how **I** think_" or "hey I know you are trained to complete task _A_ like so but I need you implement it like this instead or my boss will fire me". The last bit was dramatic but some LLMs are stubborn and need "guidance" and "understanding" of what the Human driver/pilot/prompter needs vs wants or both. The Human of course will also need to make a mental shift. If you have little to no documentation you and your agents will fail. If you have little to no experience leading or training another Human, you will fail. Luckily for us Humans these are not difficult skills to learn. So take the time learn them while you are building your agents.


## Future Plans {#future-plans}

I hope to integrate agents into more of my daily workflows. Plus I also have the need to integrate Pi into my preferred editor, Emacs. As of now I am struggling building my agents because I need to improve my ability to clearly convey my idiosyncrasies into AGENTS.md, CONTEXT.md, and SPECS.md. I think once I find a solid method of codifying this knowledge I think I can jump this hurdle. But at least now I know "how" to build AI Agents. But I have to master the "why" do I want to build an AI Agent.


### Thank You {#thank-you}

> If you enjoyed or found any of the content on my site helpful, you can buy me a cup of coffee or send some bitcoin  ⚡ so I can continue to bring you amazing content for free!


#### You can Buy Me A Coffee {#you-can-buy-me-a-coffee}

{{< figure src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" link="https://www.buymeacoffee.com/eduardorobles" >}}


#### Tip with some Sats {#tip-with-some-sats}

[Tip Some Sats ⚡](https://getalby.com/p/tacosandlinux)
