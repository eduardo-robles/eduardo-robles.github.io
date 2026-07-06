+++
title = "How to use REMnux MCP"
date = 2026-07-06T05:31:00-05:00
draft = false
+++

## REMnux MCP {#remnux-mcp}

The REMnux MCP is a method to connect an AI coding tool to various REMnux tools. This is the latest work from the REMnux project and it allows for AI assisted analysis. In this post were going to learn how to SSH into REMnux and have our local AI Agent use REMnux for Analysis. In this setup your local AI Agent and REMnux MCP stay local to your machine and REMnux is remotely accessed. This allows for some isolation when analyzing the malicious samples.


## Method 1: SSH into REMnux VM {#method-1-ssh-into-remnux-vm}

First you need some prerequisites to ensure you can get your local AI Agent to communicate via SSH.

1.  Install the REMnux MCP server ex. `npx @remnux/mcp-server`
2.  Configure your AI Agent to accept the MCP server ex. `claude mcp add remnux -- npx @remnux/mcp-server --mode=ssh --host=VM_IP --user=remnux --password=YOUR_PASSWORD`
    -   I included a copy of my Pi MCP config for reference, see below.
3.  Ensure the REMnux VM is accessible via SSH ex. `sshd start`

Once all that is configured simply open your AI Agent and connect to the MCP server. Your AI Agent should immediately see all the available tools from REMnux. From here you can begin your analysis. Simply tell the agent what you want to do and it should be able to execute the analysis from the REMnux VM. It can even assist with moving via SSH any samples you have locally.


## Method 2: SSH into REMnux Container {#method-2-ssh-into-remnux-container}

I use Podman instead of Docker but most of these commands work the same.

1.  Create a REMnux Container ex.
    `podman run --rm -d -p 2222:22 remnux/remnux-distro:noble`
2.  Connect to the running REMnux container
    `ssh remnux@localhost -p 2222`

The key part in this method is binding a higher port for SSH (2222). You can chose whatever port because 22 is a privileged port. Using 22 will require running your container with higher privileges which is not necessary. Use a transient contianer because we do not want to leave a possible malware sample unattended.


## Tips {#tips}

In REMnux the default username and password are listed in the documentation. It's very similar to the way Kali Linux does their username and password scheme. In either of these methods, be sure to have the Virtual Machine or Container running before trying to connect.

-   REMnux MCP Server docs
    <https://github.com/REMnux/remnux-mcp-server>
-   REMnux AI Documentation
    <https://docs.remnux.org/tips/using-ai>


## Example MCP Config {#example-mcp-config}

Below you will see that in my MCP config I have both connections to the REMnux Virtual Machine and Container. You can expand it even further by having maybe 2 different Virtual Machine configs. Say 1 VM has a slightly different configuration (iNet Sim). You can configure both in you AI Agent settings and allow your AI agent to work on either. This is convient and flexible when working with various setups.

```json
{
  "mcpServers": {
    "remnux-vm": {
      "command": "npx",
      "args": [
        "@remnux/mcp-server",
        "--mode=ssh",
        "--host=VM_IP",
        "--user=remnux",
        "--password=YOUR_PASSWORD"
      ],
      "directTools": false
    },
    "remnux-container": {
      "command": "npx",
      "args": [
        "@remnux/mcp-server",
        "--mode=ssh",
        "--host=localhost",
        "--port=2222",
        "--user=remnux",
        "--password=YOUR_PASSWORD"
      ],
      "directTools": true
    }
  }
}
```


## Conclusion {#conclusion}

I have shown how you can setup the REMnux MCP Server to allow your AI Agent to analyze malicious samples. As always use abudant precaution when working with malware samples. AI Agents do not understand intent only instructions. So be as explicit as possible. You may even want to clearly define in your `AGENTS.md` to have the AI Agent only follow this method.


### Thank You {#thank-you}

> If you enjoyed or found any of the content on my site helpful, you can buy me a cup of coffee or send some bitcoin  ⚡ so I can continue to bring you amazing content for free!


#### You can Buy Me A Coffee {#you-can-buy-me-a-coffee}

{{< figure src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" link="https://www.buymeacoffee.com/eduardorobles" >}}


#### Tip with some Sats {#tip-with-some-sats}

[Tip Some Sats ⚡](https://getalby.com/p/tacosandlinux)
