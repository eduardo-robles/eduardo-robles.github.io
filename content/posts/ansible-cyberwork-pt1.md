+++
title = "Ansible for Cybersecurity Work - Part 1"
publishDate = 2024-07-12
draft = false
+++

## Is it DevSecOp, SecDevOps, OpsSecDev? {#is-it-devsecop-secdevops-opssecdev}

The infosec field is full of buzzword now more so with the explosion of automation and AI. Luckily, I am not easily fooled by the buzzword and look for the real meat and bones. So when I was tasked with automating some tasks at work I jumped into an interesting technology called Ansible. Ansible is a tool for automation that is cross platform. It relies on setting up a secure connection to an endpoint and then Ansible handle executing tasks on the system.

In a Software/Hardware diverse environment a tool like Ansible is refreshing. But why would someone who work in Cybersecurity care about Ansible. Well technologies and environments change and having a tool that is cross-platform helps reduce complexity. I can use Ansible to manage Windows workstation and patch Linux servers. Deploy software to MacOS and manage Firewall settings. I'd like to take you along my journey of learning and implementing concepts like: Infrastructure as Code, DevSecOps, and Orchestration as a Cybersecurity Analyst.


## Learning to Communicate {#learning-to-communicate}

This series will be a quick and dirty view into how Ansible works. Let's start with how Ansible communicates with endpoints. If you're on a Unix based system that's easy you will be using SSH. Simply setup SSH key based authentication and you are good to go. If you are on Windows based systems this gets a bit more interesting (or complicated). Yes, OpenSSH exists on Windows Servers and Workstations but it is not as robust as it is on Unix. Plus Microsoft has other tools for this type of automation. WinRM is the go tool technology on Windows system when using Ansible.

WinRM is not easy to configure or too understand. Most people get WinRM configurations wrong because Microsoft is not very good at explaining this tool. It took me about 3 months to fully understand the basics of WinRM. So I'd like to save you some time and explain the basics of WinRM. Let's start with setting up a "Listener".


## WinRM Communication {#winrm-communication}

WinRM communicates via an HTTP SOAP api. This means we can send WinRM communications via HTTP or HTTPS. Setting this is done with the `winrm` listener commands in Windows. The method of achieving HTTPS communication is multifaceted. You can employ a reverse proxy, use Active Directory Certificate Services to deploy certificates, or use OpenSSL to create certificates. The use of self-signed certificates is also possible.


### Ansbile Variable {#ansbile-variable}

Setting the following variable will let Ansible know which communication method to use.
`ansible_winrm_scheme:` ex. `ansible_winrm_scheme: https`


### Setting Up HTTPS Certificate Validation with OpenSSL {#setting-up-https-certificate-validation-with-openssl}


#### Generate Certificates with OpenSSL {#generate-certificates-with-openssl}

<!--list-separator-->

-  Generate the CA Private Key

    ```bash
    openssl genpkey -algorithm RSA -out ca-key.pem
    ```

<!--list-separator-->

-  Generate the CA certificate

    ```bash
    openssl req -x509 -new -nodes -key ca-key.pem -sha256 3650 -out ca-cert.pem -subj "/C=US/ST=DC/L=Washington/O=ORG/OU=MyORG/CN=CA"
    ```

<!--list-separator-->

-  Generate the private key for the Windows host

    ```bash
    openssl genpkey -algorithm RSA -out windows-host-key.pem
    ```

<!--list-separator-->

-  Create a certificate signing request (CSR)

    ```bash
    openssl req -new -key windows-host-key.pem -out windows-host.csr -subj "/C=US/ST=DC/L=Washington/O=ORG/OU=MyORG/CN=hostname.domain.com"
    ```

<!--list-separator-->

-  Create a configuration file for the certificate exentensions

    ```ini
    authorityKeyIdentifier=keyid,issuer
    basicConstraints=CA:FALSE
    keyUsage = digitalSignature, noRepudiation, keyEncipherment, dataEncipherment
    extendedKeyUsage = serverAuth
    subjectAltName = @alt_names

    [alt_names]
    DNS.1 = hostname.domain.com
    ```

<!--list-separator-->

-  Generate the certificate signed by the CA

    ```bash
    openssl x509 -req -in windows-host.csr -CA ca-cert.pem -CAkey ca-key.pem -CAcreateserial -out windows-host-cert.pem -days 365 -sha256 -extfile windows-host-ext.cnf
    ```

<!--list-separator-->

-  Create the PFX file for Windows Host

    ```bash
    openssl pkcs12 -export -out windows-host-cert.pfx -inkey windows-host-key.pem -in windows-host-cert.pem -certfile ca-cert.pem -password pass:password
    ```


#### Import Certificates into the Windows Machine {#import-certificates-into-the-windows-machine}

<!--list-separator-->

-  Import the PFX Certificate into Windows Host

    Use Powershell as an Administrator

    ```powershell
    $password = ConvertTo-SecureString -String "password" -Force -AsPlainText
    Import-PfxCertificate -FilePath "C:\path\to\windows-cert.pfx" -CertStoreLocation Cert:\LocalMachine\My -Password $password
    ```

<!--list-separator-->

-  Import the CA certificate into Windows Host

    ```powershell
    Import-Certificate -FilePath "C:\path\to\ca-cert.pem" -CertStoreLocation Cert:\LocalMachine\Root
    ```


#### Configure WinRM on the Windows Machine {#configure-winrm-on-the-windows-machine}

<!--list-separator-->

-  Enable WinRM

    ```powershell
    winrm quickconfig -force
    ```

<!--list-separator-->

-  Create an HTTPS listener using the certificate

    ```powershell
    $cert = Get-ChildItem -Path Cert:\LocalMachine\My | Where-Object { $_.Subject -eq "CN=hostname.domain.com"}
    $thumbprint = $cert.Thumbprint
    ```

<!--list-separator-->

-  Create the HTTPS Listener

    ```powershell
    winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname="hostname.domain.com"; CertificateThumbprint="$thumbprint"}
    ```

<!--list-separator-->

-  Enable Certificate Authentication

    ```powershell
    winrm set winrm/config/service/auth @{Certificate="true"}
    ```

<!--list-separator-->

-  Set Trusted Hosts

    This adds extra security by only allowing one control machine(node) to communicate with the endpoints. This is a winrm configuration. The value `ansible-controlnode` is the hostname for whatever you are using as the Ansible control node.

    ```powershell
    Set-Item wsman:\localhost\Client\TrustedHosts -Value "ansible-controlnode"
    ```


#### Configure Ansible to Use the Certificates {#configure-ansible-to-use-the-certificates}

<!--list-separator-->

-  Place the CA Certificate and Client key/cert on the Ansible Control Node

    ```bash
    openssl pkcs12 -in windows-host-cert.pfx -clcerts -nokeys -out client-cert.pem -password pass:password
    openssl pkcs12 -in windows-host-cert.pfx -nocerts -nodes -out client-key.pem -password pass:password
    ```


### Setting Up HTTPS with Self-Signed Certificate with Powershell {#setting-up-https-with-self-signed-certificate-with-powershell}


#### Generate the Self-signed certificate {#generate-the-self-signed-certificate}

In this example `-DnsName` is set the Hostname of the machine (FQDN).

```powershell
New-SelfSignedCertificate -DnsName "MyMachine01.local" -CertStoreLocation Cert:\LocalMachine\My
```


#### Configure WinRM Listener {#configure-winrm-listener}

In this example `Hostname` is the the hostname of the machine, and `CertificateThumbprint` is get the Thumbprint from the Self-signed certificate.

```powershell
winrm create winrm/config/Listener?Address=*+Transport=HTTPS '@{Hostname="MyMachine01.local"; CertificateThumbprint="thumbprintondevice"}'
```


#### Configure Firewall to Allow TCP 5986 aka WinRM over HTTPS {#configure-firewall-to-allow-tcp-5986-aka-winrm-over-https}

```powershell
New-NetFirewallRule -DisplayName "WinRM over HTTPS" -Direction Inbound -Protocol TCP -LocalPort 5986 -Action Allow -Profile Domain
```


#### Configure TrustedHosts for WinRM {#configure-trustedhosts-for-winrm}

In this example I am setting the Trusted Hosts value to `MyNode00.local`. Ideally this will be the ansible control node.

```powershell
Set-Item WSMan:\localhost\Client\TrustedHosts -Value "MyNode00.local"
```

<!--list-separator-->

-  Remove TrustedHosts Value

    If needed you can remove the trusted host value.

    ```powershell
    Remove-Item WSMan:\localhost\Client\Trustedhosts
    ```

<!--list-separator-->

-  Get TrustedHosts Value

    ```powershell
    Get-Item WSMan:\localhost\Client\Trustedhosts
    ```


#### Powershell Script to Enable WinRM Listener and Configure Firewall for HTTPS with Self-signed Certificate communication for WinRM {#powershell-script-to-enable-winrm-listener-and-configure-firewall-for-https-with-self-signed-certificate-communication-for-winrm}

```powershell
# Get Variables
# Hostname
$fqdn = $env:computername +'.'+ $env:userdnsdomain

# Create Variables
# Trusted Host
$trusthost = "MyNode00.local"
# Certificate Store "My"
$mystore = "Cert:\LocalMachine\My"
# Certificate Store "Root"
$rootstore = "Cert:\LocalMachine\root"

# Create new Self-signed certificate
Write-Verbose "Creating Self-Signed Certificate"
New-SelfSignedCertificate -DnsName "$fqdn" -CertStoreLocation Cert:\LocalMachine\My
# Create new Self-signed certificate and expire in 6 months
#New-SelfSignedCertificate -DnsName "$fqdn" -CertStoreLocation Cert:\LocalMachine\My -NotAfter (Get-Date).AddMonths(6)

# Get thumbrprint from Self-signed certificate
$cert = Get-ChildItem -Path Cert:\LocalMachine\My | Where-Object { $_.Subject -eq "CN=$fqdn"}
$thumbprint = $cert.Thumbprint

# Find and start the WinRM service.
Write-Verbose "Verifying WinRM service."
If (!(Get-Service "WinRM")) {
  Write-ProgressLog "Unable to find the WinRM service."
  Throw "Unable to find the WinRM service."
}
ElseIf ((Get-Service "WinRM").Status -ne "Running") {
  Write-Verbose "Setting WinRM service to start automatically on boot."
  Set-Service -Name "WinRM" -StartupType Automatic
  Write-ProgressLog "Set WinRM service to start automatically on boot."
  Write-Verbose "Starting WinRM service."
  Start-Service -Name "WinRM" -ErrorAction Stop
  Write-ProgressLog "Started WinRM service."

}

# Configure WinRM Listener
Write-Verbose "Configure HTTPS Listener"
winrm create winrm/config/Listener?Address*+Transport=HTTPS '@{Hostname="$fqdn";CertificateThumbprint="$thumbprint"}'

#Configure Kerberos authentication for WinRM
Write-Verbose "Configure Kerberos Auth for WinRM"
winrm set WinRM/Config/Client/Auth '@{Basic="false";Digest="false";Kerberos="true";Negotiate="false";Certificate="true";CredSSP="false"}'

# Delete HTTP Listener
Write-Verbose "Deleting HTTP Listner"
winrm delete WinRM/Config/Listener?Address=*+Transport=HTTP

# Configure Firewall Rule
Write-Verbose "Configure Firewall Rule"
New-NetFirewallRule -DisplayName "WinRM over HTTPS" -Direction Inbound -Protocol TCP -LocalPort 5986 -Action Allow -Profile Domain

# Configure TrustedHosts
# This is how you prevent lateral movement!
Write-Verbose "Set WinRM Trusted Hosts"
Set-Item WSMan:\localhost\Client\TrustedHosts -Value "$trusthost"
```


## WinRM is NOT Powershell Remoting {#winrm-is-not-powershell-remoting}

This trips people up, don't make the same mistake.


## Conclusion {#conclusion}

This is months of research and hacking together scripts. I haven't gotten this work to a point were I feel it is production ready. Though I feel confident that is is a great starting point. I learned a lot about Certificate management, Windows Automation, Ansible Configurations, and Powershell. This is a fun project because it really pushed the boundaries of my knowledge of Linux and Windows systems. I am so grateful I have the opportunity to work in this field because I get to work on cool stuff like this.


### Thank You {#thank-you}

> If you enjoyed or found any of the content on my site helpful, you can buy me a cup of coffee or send some bitcoin  ⚡ so I can continue to bring you amazing content for free!


#### You can Buy Me A Coffee {#you-can-buy-me-a-coffee}

{{< figure src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" link="https://www.buymeacoffee.com/eduardorobles" >}}


#### Tip with some Sats {#tip-with-some-sats}

[Tip Some Sats ⚡](https://getalby.com/p/tacosandlinux)


### Setup {#setup}

-   Keyboard: Keyboardio Atreus (JWICk Ultimate Black Linear)
-   Mouse: MX Master (Original)
-   Emacs (WSL term)
