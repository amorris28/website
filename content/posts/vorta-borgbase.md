---
title: "How to setup Vorta and BorgBase for automatic, encrypted backups"
date: 2025-12-09
author: "Andrew"
description: "How to setup Vorta and BorgBase for automatic, encrypted backups"
tags: ["technical-guides"]
draft: true
---

[Vorta](https://vorta.borgbase.com/) is a GUI front-end for linux and macOS for the [Borg](https://borgbackup.readthedocs.io/) backend for doing "encrypted, deduplicated and compressed backups." And [BorgBase](https://www.borgbase.com/) is a cloud service for off-site backups of Borg repos. Obviously, you could setup your own off-site server with SSH if you had the knowledge and interest or an on-site backup on an external hard drive. However, Vorta + BorgBase makes off-site storage super easy. This likely wouldn't work with typical cloud storage solutions like Google Drive and Dropbox since you need SSH access in order to access your Borg repo with Vorta.

In this guide, I will walk through a simple setup with Vorta + BorgBase with sensible defaults. It can be slightly intimidating, but the [docs](https://vorta.borgbase.com/usage/) are actually quite good so I will mostly just refer to those unless I have something to add. For this you will need to get a BorgBase account, install Vorta, create a repo on BorgBase, connect via SSH, and then configure your backup schedule.

## Step 1. Local Setup: Install Vorta and create an SSH key

Official instructions for installing Vorta can be found [here](https://vorta.borgbase.com/install/). Easiest way on macOS is using [Homebrew Cask](https://brew.sh/). On linux, I recommend the [Flatpak](https://flathub.org/en/apps/com.borgbase.Vorta). There are [distro-specific binaries](https://vorta.borgbase.com/install/linux/#distribution-packages) if you prefer, but the Flatpak is easy and can sometimes be more up-to-date than the distro repos. (I'm looking at you, Debian.)

To [create your SSH key](https://docs.borgbase.com/faq/ssh), it is recommended to use ed25519.  First, check to make sure you have OpenSSH installed:

```bash
ssh-keygen
```

If not, install it for your system.

Then, create a keypair:

```bash
ssh-keygen -t ed25519
```

This will create a public/private keypair at `~/.ssh/id_ed25519.pub` for your public key and  `~/.ssh/id_ed25519` for your private key.

## Step 2. Remote Setup: Register for BorgBase and create your remote repo

You can register for BorgBase [here](https://www.borgbase.com/register). They have a free 10 GB/2 repo plan to start with and then you can upgrade to their 250 Gb plan for $24/year if you decide you need more space. Once you are in, navigate to [SSH KEYS](https://www.borgbase.com/ssh). Click "+ ADD KEY." Back on your PC, run this command:

```bash
cat ~/.ssh/id_ed25519.pub
```

And copy and paste the entire output into the Public Key box on BorgBase. You can leave the keyname blank and it will be automatically named with your PC's hostname, unless you prefer to name it something specific. Then click ADD KEY.

Now, click on [REPOSITORIES](https://www.borgbase.com/repositories). Click + NEW REPO. Name it whatever you like ("backup" or "Macbook Pro" or whatever). Choose your closest region and leave the rest of the settings on default. Go to Access and click the drop down arrow next to Full Access and add the SSH key that you just created. Finally, click ADD REPOSITORY.

## Vorta setup

Now, run Vorta and on the Default profile under the Repository tab, click the "+" dropdown and select New Repository... On your BorgBase repo page, click the copy icon on the far left to copy the repo URL and paste that into the "Repository URL" box in Vorta. Enter a passphrase and save the passphrase to your Password Manager. You shouldn't have to do anything else, it should automatically.

<!-- TODO Figure out how repo keys and passhprases work -->





