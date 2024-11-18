+++
title = "Easy DFIR Tools and Methods"
publishDate = 2024-11-17
draft = false
+++

## Phishing Email Analysis {#phishing-email-analysis}


### ClamAV {#clamav}

ClamAV is great to scan for malware but also can scan `eml` files including email attachments. Use the `--debug` flag for more info on the scan.

```sh
clamscan sample.eml
```


### Continued {#continued}

You can also use ClamAV to scan any suspicious file.

```sh
clamscan sample.zip
```


## Investigating a malicious link {#investigating-a-malicious-link}

To investigate a link I use a REMnux container which offers so many awesome tools. I will cover THUG and Automater.


### THUG {#thug}

THUG is a “honeyclient”. A honeyclient is a tool that mimicks the behavior of a web browser. Useful for analyzing what a link does when a user clicks on it.

```sh
thug -u win7chrome49 "https://eduardorobles.com"
```


### Continued... {#continued-dot-dot-dot}

Once it begins to “load” the suspicious site it executes any code that may be on the site. Once it is done running/loading the page it dumps a report. The report contains a summary of what occured plus you get any malicious artifacts that the page may have downloaded.

In one exercise a suspicous page downloaded an executable and I was able to analyze the executable from the container and it was indeed a malicous executable. Yikes!


### Automater {#automater}

> Automater is a URL/Domain, IP Address, and Md5 Hash OSINT tool aimed at making the analysis process easier for intrusion Analysts. Given a target (URL, IP, or HASH) or a file full of targets Automater will return relevant results from sources like the following: IPvoid.com, Robtex.com, Fortiguard.com, unshorten.me, Urlvoid.com, Labs.alienvault.com, ThreatExpert, VxVault, and VirusTotal.


### Continued... {#continued-dot-dot-dot}

Automater is a python tool found in `/usr/local/automater`

```sh
./Automater.py https://eduardorobles.com
```


## Investigating a suspicious PDF {#investigating-a-suspicious-pdf}

Malicous content will be embedded in a PDF. This is not immediately visible to an end user. It's best to extract the content in order to inspect it.


### Strings {#strings}

You can use the command `strings` to view all the different system call a file contains.

```nil
strings sus_invoice.pdf | grep http
```

You can also pipe grep to single out things like `http` links or hashes.


### Magika {#magika}

Magika is a tool release by Google. It's intended purpose is to accurately clasify a file. Sometime you stumble onto something and you can't figure out what this filetype is. Magika can help with this type of analysis.

```nil
pip install magika
```


### Thank You {#thank-you}

> If you enjoyed or found any of the content on my site helpful, you can buy me a cup of coffee or send some bitcoin  ⚡ so I can continue to bring you amazing content for free!


#### You can Buy Me A Coffee {#you-can-buy-me-a-coffee}

{{< figure src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" link="https://www.buymeacoffee.com/eduardorobles" >}}


#### Tip with some Sats {#tip-with-some-sats}

[Tip Some Sats ⚡](https://getalby.com/p/tacosandlinux)


### Setup {#setup}

-   Keyboard: Launch Keyboard with JWICK Utlimate Black Linear switches
-   Mouse: MX Master (Original)
-   Computer: Framework 13 (Fedora Linux)
