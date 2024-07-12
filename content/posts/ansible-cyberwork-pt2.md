+++
title = "Ansible for Cybersecurity Work - Part 2"
publishDate = 2024-07-12
draft = false
+++

## Learning to Authenticate {#learning-to-authenticate}

WinRM has 2 componets: Communication and Authentication. Like with SSH, you establish a connection then you authenticate on the endpoint. In the previous post I wrote about setting up WinRM Listener over HTTPS. Now we have to setup Authentication luckily Windows offers serveral options for Authentication. But keep in mind not all are secure nor are supported with the type of account you would like to use. In other words if you want to authenticate with Kerberos forget about using a Local Account.


## WinRM Authentication {#winrm-authentication}

WinRM authentication is the method used when authenticating against a Windows endpoint. Bascially, how you will logging into the computer remotely? WinRM offers several methods here is a break down from the Ansible documentation.

<https://docs.ansible.com/ansible/latest/os_guide/windows_winrm.html#id3>

| Option      | Local Account | AD Account | Credential Delegation | HTTP Encryption |
|-------------|---------------|------------|-----------------------|-----------------|
| Basic       | Y             | N          | N                     | N               |
| Certificate | Y             | N          | N                     | N               |
| Kerberos    | N             | Y          | Y                     | Y               |
| NTLM        | Y             | Y          | N                     | Y               |
| CredSSP     | Y             | Y          | Y                     | Y               |


### Using Certificate Authentication {#using-certificate-authentication}

Setting the following variable will let Ansible know which authentication method to use.
`ansibible_winrm_transport:` ex. `ansible_winrm_transport: certificate`


#### Generating a certificate with ADCS {#generating-a-certificate-with-adcs}

This is just for a quick demonstration on how on a local machine you can request a certificate from ADCS and the use it with Ansible.

<!--list-separator-->

-  Certlm

    Personal &gt; Certificates &gt; Request New Certificate - Chose a certifcate


#### Mapping Cert to User {#mapping-cert-to-user}

```powershell
New-Item -Path WSMan:\localhost\ClientCertificate `
    -Subject "$username@localhost" `
    -URI * `
    -Issuer $thumbprint `
    -Credential $credential `
    -Force
```


#### Exporting the Certificate for the Ansible Control Node {#exporting-the-certificate-for-the-ansible-control-node}

<!--list-separator-->

-  Place the CA Certificate and Client key/cert on the Ansible Control Node

    ```bash
    openssl pkcs12 -in windows-host-cert.pfx -clcerts -nokeys -out client-cert.pem
    openssl pkcs12 -in windows-host-cert.pfx -nocerts -nodes -out client-key.pem
    ```


### Using Kerberos Authentication {#using-kerberos-authentication}

Setting the following variable will let Ansible know which authentication method to use.
`ansible_winrm_transport:` ex. `ansible_wirm_transport: kerberos`


#### Install Kerberos on Ubuntu {#install-kerberos-on-ubuntu}

```sh
sudo apt-get install python3-dev libkrb5-dev krb5-user
```

You will need a few configurations for the local Kerberos install. Luckily they are not too crazy, but highly important.
`sudo emacs -nw /etc/krb5.conf` or `sudo nano /etc/krb5.conf`


#### Kerberos Default Configuration {#kerberos-default-configuration}

Below is an example configuration for Kerberos. It's super complicated but essentially you need to set your realms to match the AD environment. Pro tip: be sure DNS is configured and working correctly in your environment or else Kerberos becomes a nightmare.

```text
[libdefaults]
        default_realm = myorg.LOCAL

# The following krb5.conf variables are only for MIT Kerberos.
        kdc_timesync = 1
        ccache_type = 4
        forwardable = true
        proxiable = true

# The following encryption type specification will be used by MIT Kerberos
# if uncommented.  In general, the defaults in the MIT Kerberos code are
# correct and overriding these specifications only serves to disable new
# encryption types as they are added, creating interoperability problems.
#
# The only time when you might need to uncomment these lines and change
# the enctypes is if you have local software that will break on ticket
# caches containing ticket encryption types it doesn't know about (such as
# old versions of Sun Java).

#       default_tgs_enctypes = des3-hmac-sha1
#       default_tkt_enctypes = des3-hmac-sha1
#       permitted_enctypes = des3-hmac-sha1

# The following libdefaults parameters are only for Heimdal Kerberos.
        fcc-mit-ticketflags = true

[realms]
        ATHENA.MIT.EDU = {
                kdc = kerberos.mit.edu
                kdc = kerberos-1.mit.edu
                kdc = kerberos-2.mit.edu:88
                admin_server = kerberos.mit.edu
                default_domain = mit.edu
        }
        ZONE.MIT.EDU = {
                kdc = casio.mit.edu
                kdc = seiko.mit.edu
                admin_server = casio.mit.edu
        }
        CSAIL.MIT.EDU = {
                admin_server = kerberos.csail.mit.edu
                default_domain = csail.mit.edu
        }
        IHTFP.ORG = {
                kdc = kerberos.ihtfp.org
                admin_server = kerberos.ihtfp.org
        }
        1TS.ORG = {
                kdc = kerberos.1ts.org
                admin_server = kerberos.1ts.org
        }
        ANDREW.CMU.EDU = {
                admin_server = kerberos.andrew.cmu.edu
                default_domain = andrew.cmu.edu
        }
        CS.CMU.EDU = {
                kdc = kerberos-1.srv.cs.cmu.edu
                kdc = kerberos-2.srv.cs.cmu.edu
                kdc = kerberos-3.srv.cs.cmu.edu
                admin_server = kerberos.cs.cmu.edu
        }
        DEMENTIA.ORG = {
                kdc = kerberos.dementix.org
                kdc = kerberos2.dementix.org
                admin_server = kerberos.dementix.org
        }
        stanford.edu = {
                kdc = krb5auth1.stanford.edu
                kdc = krb5auth2.stanford.edu
                kdc = krb5auth3.stanford.edu
                master_kdc = krb5auth1.stanford.edu
                admin_server = krb5-admin.stanford.edu
                default_domain = stanford.edu
        }
        UTORONTO.CA = {
                kdc = kerberos1.utoronto.ca
                kdc = kerberos2.utoronto.ca
                kdc = kerberos3.utoronto.ca
                admin_server = kerberos1.utoronto.ca
                default_domain = utoronto.ca
        }

[domain_realm]
        .mit.edu = ATHENA.MIT.EDU
        mit.edu = ATHENA.MIT.EDU
        .media.mit.edu = MEDIA-LAB.MIT.EDU
        media.mit.edu = MEDIA-LAB.MIT.EDU
        .csail.mit.edu = CSAIL.MIT.EDU
        csail.mit.edu = CSAIL.MIT.EDU
        .whoi.edu = ATHENA.MIT.EDU
        whoi.edu = ATHENA.MIT.EDU
        .stanford.edu = stanford.edu
        .slac.stanford.edu = SLAC.STANFORD.EDU
        .toronto.edu = UTORONTO.CA
        .utoronto.ca = UTORONTO.CA
```

You can get a Kerberos ticket by running to following command.
`kinit myuseraccnt.local`
You can view a list of the current tickets issued by kerberos with this command.
`klist`


## Conclusion {#conclusion}

When chosing an Authentication method for WinRM I would recommend Kerberos. Kerberos is by far a better and more secure option than `Basic` or `NTLM` authentication. Luckily, you can use CredSSP or Certificates if you are hestitant to use Kerberos. Overall the Authentication part of WinRM is much easier even if you chose Kerberos. But if you do not configure it correctly, you will never be able to login into a workstation remotely with Ansible. Moreover if you chose a less secure method do not use it in production.


#### Thank You {#thank-you}

> If you enjoyed or found any of the content on my site helpful, you can buy me a cup of coffee or send some bitcoin  ⚡ so I can continue to bring you amazing content for free!


#### You can Buy Me A Coffee {#you-can-buy-me-a-coffee}

{{< figure src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" link="https://www.buymeacoffee.com/eduardorobles" >}}


#### Tip with some Sats {#tip-with-some-sats}

[Tip Some Sats ⚡](https://getalby.com/p/tacosandlinux)


### Setup {#setup}

-   Keyboard: Keyboardio Atreus (JWICk Ultimate Black Linear)
-   Mouse: MX Master (Original)
-   Emacs (WSL term)
