+++
title = "Create SSH Key"
date = 2022-03-08T18:38:00-06:00
draft = false
+++

Creating an SSH key is very easy. Below is an example of how to generate an SSH key with the `ssh-keygen` command.


## Create SSH Key {#create-ssh-key}

```bash
ssh-keygen -t ed25519 -f ~/.ssh/nameofkey -N '' -C "comment goes here"
```

```bash
ssh-keygen -t rsa -f ~/.ssh/nameofkey -N '' -C "comment goes here"
```


### -t option is for the type of keys to be created (ex. ed25519) {#t-option-is-for-the-type-of-keys-to-be-created--ex-dot-ed25519}


### -f option is the filename and location of the keys (ex. `/path/to/file`) {#f-option-is-the-filename-and-location-of-the-keys--ex-dot-path-to-file}


### -N is the passphrase to be given, leave blank for no passphrase {#n-is-the-passphrase-to-be-given-leave-blank-for-no-passphrase}


### -C enter a comment to best find keys later (ex. "github key") {#c-enter-a-comment-to-best-find-keys-later--ex-dot-github-key}

If you enjoyed or found any of the content on my site helpful, you can buy me a cup of coffee so I can continue to bring you amazing content for free!


### Thank You {#support}

{{< figure src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" link="https://www.buymeacoffee.com/eduardorobles" >}}
