---
layout: default
title: SSH
description: A short and practical guide to setting up and using SSH to connect to a remote computer.
tags: CLI SSH
---
# SSH

SSH is an incredibly convenient way to securely authenticate yourself and connect to another computer. This post is a quick overview of how to set it up and how it works.

To use SSH you just need an SSH client and a set of keys. While client sounds fancy it really just means being able to run the `ssh` command in your terminal. Clients are already built into the terminals for Mac and Linux systems, and for Windows you can use the shell that comes with Git. 

All you have to do to set it up is generate a public/private key pair and give out the public key to whatever service you want to connect to. If it's something like Github or a VPS service like DigitalOcean then you can usually add them in the settings for your account. 

If you need a CLI option look into the `ssh-copy-id` command (it will end up like this: `ssh-copy-id -i ~/.ssh/id_rsa.pub user@server_ip`).

Below are the commands to generate a new key pair and print out the public key to the terminal window so that you can copy-paste it into whatever you need.

```
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub
```

## How it Works

Now for a quick and high-level overview of how it works, so you can better understand some of what is going on if that's interesting to you.

To start, the `ssh-keygen` command produces two keys: a public key that you give out to anyone you want to use SSH with, and a private key that you always keep secret and only on your machine. 

Next, two important properties of the keys: the public key can be used encrypt messages that can only be decrypted by the private key, and the private key cannot be derived from the public key. 

Just based on that you know you can use them to authenticate, since a remote server can use the public key to test you and only you can pass since only you have the private key.

Once authenticated a similar process occurs where the server and the client that is connecting to it work together to generate a new set of keys where they both know the secret (since the authentication part is one-way and only the server can encrypt things since only the one connecting has the private key). They use that to encrypt the rest of the session. 

If you want to learn more about either of these then google around for "symmetric encryption" (the part where both know the secret to encrypt the whole session) or "asymmetric encryption" (the authentication part where just the one has their private key).