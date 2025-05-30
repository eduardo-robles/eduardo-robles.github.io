+++
title = "BSIDES RGV 2025"
publishDate = 2025-05-30
draft = false
+++

## BSIDES RGV 2025 {#bsides-rgv-2025}

<style>.org-center { margin-left: auto; margin-right: auto; text-align: center; }</style>

<div class="org-center">

Quick, Easy, Malware Investigations and Threat Hunting

</div>


## About Me {#about-me}

-   Hi! I'm Eduardo Robles I work for County of Hidalgo IT dept as a Cybersecurity Analyst IV
-   Founder of South Texas Linux Users Group.
-   You can check out my skills on my blog or LinkedIn.


## Agenda {#agenda}

1.  Learn the basics of Malware Analysis
2.  Learn some Threat Hunting skills
3.  Small look into Digital Forensics


## Disclaimer {#disclaimer}

Everything in this talk is my own research and opinion.


## Malware Investigations {#malware-investigations}


### Why do internal malware analysis? {#why-do-internal-malware-analysis}

-   Existing tools Virustotal, JoeSandbox, etc.
-   Protect sensitive information from 3rd parties.
-   Freedom from reliance on one tool or platform. You can still use Virustotal or Any.Run but you can build workflows that don't rely on them.


### Malware is scary and dangerous, put in a box (container). {#malware-is-scary-and-dangerous-put-in-a-box--container--dot}

Malware is scary. Malware is dangerous. So it's best to analyze in a "contained" environment.

-   Virtual Machines
-   Containers (Docker, Podman, etc)


### Working with Malware Samples {#working-with-malware-samples}

Safely moving malware around to later analyze can be daunting. Here are some pointers.


#### Defang {#defang}

Take a normal hyperlink or file extention and defang it so it's not active.

-   Normal

<!--listend-->

```text
https://eduardorobles.com or superbadmalware.exe
```

-   Defanged

<!--listend-->

```text
hxxps://eduardorobles[.]com or superbadmalware.malz
```


#### Encrypted Archive with a Password (7zip) {#encrypted-archive-with-a-password--7zip}

Use 7zip to password encrypt an archive. This add an extra layer of protection by not allowing someone to accidently open the archive.


#### Disable network access {#disable-network-access}

-   You can disable network access to your malware analysis station.
-   This stops malware from communicating to a C2 infrastruture.
-   Or you can also simulate network traffic if you want to analyze what the malware might be trying to communicate with.


### REMnux {#remnux}

If you want easy button for malware analysis use **REMnux** as a VM or a container!
<https://remnux.org>

> "REMnux: A Linux Toolkit for Malware Analysis"


### Setup REMnux in a Container {#setup-remnux-in-a-container}

-   So I can have a container running on a Malware Analysis station and on my workstation. This gives me the flexibility to test on either machine or platform.


#### Run REMnux as a Transient container {#run-remnux-as-a-transient-container}

```sh
podman run \
       --rm \
       -it \
       --name malContainer \
       -v /var/home/core/SAMPLES:/home/remnux/files \
       --privileged \
       --network none \
       remnux/remnux-distro:focal bash
```


## Digital Forensics {#digital-forensics}


### Phishing Email Analysis {#phishing-email-analysis}


#### ClamAV {#clamav}

ClamAV is great to scan for malware but also can scan `eml` files including email attachments. Use the `--debug` flag for more info on the scan.

```sh
clamscan sample.eml
```


#### Continued {#continued}

You can also use ClamAV to scan any suspicious file.

```sh
clamscan sample.zip
```


#### Demo {#demo}

<iframe src="https://archive.org/embed/20250529-email-analysis" width="640" height="480" frameborder="0" webkitallowfullscreen="true" mozallowfullscreen="true" allowfullscreen></iframe>


### Investigating a suspicious PDF {#investigating-a-suspicious-pdf}

Malicious content will be embedded. It's best to extract the content in order to inspect it.


#### Strings {#strings}

You can use the command `strings` to view all the different system call a file contains.

```nil
strings sus_invoice.pdf | grep http
```

You can also pipe grep to single out things like `http` links or hashes.


#### Yara {#yara}

<https://github.com/airbnb/binaryalert/blob/master/rules/public/eicar.yara>

```yara
rule eicar_av_test {
    /*
       Per standard, match only if entire file is EICAR string plus optional trailing whitespace.
       The raw EICAR string to be matched is:
       X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*
    */

    meta:
        description = "This is a standard AV test, intended to verify that BinaryAlert is working correctly."
        author = "Austin Byers | Airbnb CSIRT"
        reference = "http://www.eicar.org/86-0-Intended-use.html"

    strings:
        $eicar_regex = /^X5O!P%@AP\[4\\PZX54\(P\^\)7CC\)7\}\$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!\$H\+H\*\s*$/

    condition:
        all of them
}

rule eicar_substring_test {
    /*
       More generic - match just the embedded EICAR string (e.g. in packed executables, PDFs, etc)
    */

    meta:
        description = "Standard AV test, checking for an EICAR substring"
        author = "Austin Byers | Airbnb CSIRT"

    strings:
        $eicar_substring = "$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!"

    condition:
        all of them
}
```


#### Demo {#demo}

<iframe src="https://archive.org/embed/20250528-file-analysis" width="640" height="480" frameborder="0" webkitallowfullscreen="true" mozallowfullscreen="true" allowfullscreen></iframe>


### Investigating a malicious link {#investigating-a-malicious-link}

To investigate a link REMnux offers so many awesome tools. I will cover THUG and Automater.


#### THUG {#thug}

THUG is a “honeyclient”. A honeyclient is a tool that mimicks the behavior of a web browser. Useful for analyzing what a link does when a user clicks on it.

```sh
thug -u win7chrome49 "https://eduardorobles.com"
```


#### Continued... {#continued-dot-dot-dot}

Once it begins to “load” the suspicious site it executes any code that may be on the site. Once it is done running/loading the page it dumps a report. The report contains a summary of what occured plus you get any malicious artifacts that the page may have downloaded.

In one exercise a suspicous page downloaded an executable and I was able to analyze the executable from the container and it was indeed a malicous executable. Yikes!


#### Demo {#demo}

<iframe src="https://archive.org/embed/20250529-link-analysis" width="640" height="480" frameborder="0" webkitallowfullscreen="true" mozallowfullscreen="true" allowfullscreen></iframe>


## Threat Hunting {#threat-hunting}


### Velociraptor {#velociraptor}

"Velociraptor is an advanced digital forensic and incident response tool that enhances your visibility into your endpoints."
<https://docs.velociraptor.app/>

```text
Velociraptor.exe gui
```


### Tools {#tools}


#### Cyberchef {#cyberchef}

A great tool!

> GCHQ CyberChef in a container. CyberChef is the Cyber Swiss Army Knife web app for encryption, encoding, compression and data analysis.

Let's run it in a container!

```sh
podman run \
       -d \
       --name cyberchef \
       -p 8000:8000 \
       mpepping/cyberchef
```


## Conclusion {#conclusion}

-   Hope you learned some quick tools to add to your daily workflow.
-   Automation?? A.I?? ¯\\_(ツ)\_/¯
-   Analyzing malware can be tricky but it shouldn't be intimidating.
