+++
title = "Cyberchef for Forensic Investigation and Incident Response"
publishDate = 2025-02-04
draft = false
+++

## What is Cyberchef? {#what-is-cyberchef}

Cyberchef is a tool I learned about toward the end of 2024. Since then I began using it more and more.

> CyberChef was developed by GCHQ and is the Cyber Swiss Army Knife web app for encryption, encoding, compression and data analysis.

In the end it proved to extremely useful for Forensic Analysis and Incident Response investigations. You can use the free version online but if you want to run it in your environment you can. You can leverage containers to do so and I've been using it with Podman. Let's run it in a container!

```sh
podman run \
       -d \
       --name cyberchef \
       -p 8000:8000 \
       mpepping/cyberchef
```


## Foresic Analysis with Cyberchef {#foresic-analysis-with-cyberchef}


### OCR Images {#ocr-images}

Cyberchef has the ability to do Optical Character Recoginition (OCR). This is useful if you need to get text from a screenshot or picture. Say you get a piece of evidence in a `jpg` you can drag and drop the image into Cyberchef and use the "Optical Character Recognition" operation. Your mileage may vary but in a pinch this can be a great tool.

{{< figure src="/images/cyberchefdemo1.gif" caption="<span class=\"figure-number\">Figure 1: </span>Getting a command from a screenshot" >}}


### QR Codes - Decoding Quishing Attacks {#qr-codes-decoding-quishing-attacks}

Did you or a coworker get a possible phishing email that contains a suspicious QR Code? You can avoid using your phone to scan the QR Code to find out if it's contents are malicious simply use "Parse QR Code" operation in Cyberchef! This prevents accidental Quishing attacks and you can now block the IP/URL embedded in the QR Code.

{{< figure src="/images/cyberchefdemo2.gif" caption="<span class=\"figure-number\">Figure 2: </span>Hack the planet!" >}}

Bonus: You can also use the "Defang URL" operation in Cyberchef to safely share the URL!


## Decode Malicious Scripts {#decode-malicious-scripts}


### Deobfuscate Powershell scripts {#deobfuscate-powershell-scripts}

A common tactic advesaries use is to Obsfuscate their Powershell scripts to avoid detection. Cyberchef has a the capability to decode scripts that have been heavily obfuscated. This require some knowledge of Regex and various encoding formats. So it's helpful if you learn that first so you can leverage the tools inside of Cyberchef. Nonetheless, Cyberchef has all the tools you would need to do so. I'll link to a few resources that can give you insights on how to accomplish this.


#### Github - mattnotmax/cyberchef-recipes {#github-mattnotmax-cyberchef-recipes}

<https://github.com/mattnotmax/cyberchef-recipes?tab=readme-ov-file>


#### Tevora - 5 Minute Forensics: Decoding Powershell Payloads {#tevora-5-minute-forensics-decoding-powershell-payloads}

<https://www.tevora.com/threat-blog/5-minute-forensics-decoding-powershell-payloads/>


## Conclusion {#conclusion}

Leveraging Cyberchef with all it's operations is essential for day to day operations. This is something you should definitely consider using in your tool bag.


### Thank You {#thank-you}

> If you enjoyed or found any of the content on my site helpful, you can buy me a cup of coffee or send some bitcoin  ⚡ so I can continue to bring you amazing content for free!


#### You can Buy Me A Coffee {#you-can-buy-me-a-coffee}

{{< figure src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" link="https://www.buymeacoffee.com/eduardorobles" >}}


#### Tip with some Sats {#tip-with-some-sats}

[Tip Some Sats ⚡](https://getalby.com/p/tacosandlinux)


### Setup {#setup}

-   Computer: Framework 13 (Fedora Linux)
