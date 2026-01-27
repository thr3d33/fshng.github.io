---
title: "How To: LXC/LXD on Arch Linux"
description: "Cans of code."
date: 2026-01-26 00:00:00
layout: post
categories: [containers, lxd, lxc]
tags: [linux, how-to, archlinux]
comments: false
---
### Install

Update your system and Install the LXC and LXD packages:

Make sure everything is up-to-date with an system `update`
```bash
sudo pacman -Syu
```

Install the [lxd]() package using `pacman`
```bash
sudo pacman -S lxd lxc
```

### Configure

```bash
sudo usermod -v 1000000-1000999999 -w 1000000-1000999999 root
``` 

Add user to the LXD group:

> Adding any user to the `lxd` group will be granting them elevated privileges (root). Use with caution.
{: .prompt-warning }
 
```bash
sudo usermod -aG lxd $USER
```

Before we logout to lets `enable` and `start` the `lxc.service`

```bash
sudo systemctl enable lxc.service
sudo systemctl start lxc.service
sudo systemctl status lxc.service
```

Logout

---
### Usage

Once you have logged back in.

Initialise `lxd`. This requires elevated privileges, if your user was added to the group correctly, it will initialise and prompt you with a series of configurations questions:

```bash
lxd init
```

We can list the remote repositories we have access to by default.

```bash
lxc list remote
```

We can also list the images that are present on the repository.

```bash
lxc image list images:
```

In the below example we launch an `archlinux/amd64` Linux container from the `images` repository.

```bash
lxc launch images:archlinux/amd64 <container-name> # <container-name> is optional
```

### Using Snap



---
**References:**
- <https://wiki.archlinux.org/title/LXD> - Arch Wiki - LXD
- 

