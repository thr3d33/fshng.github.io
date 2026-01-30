---
title: "How To: Snap on Arch"
description:
date: 2025-09-07 00:00:00
modified: 2026-01-30 12:12
layout: post
categories:
  - packages
tags:
  - linux
  - snap
  - how-to
comments: false
image:
  path: assets/img/headers/howto-snap-arch.webp
---
>"Snaps are app packages for desktop, cloud and IoT that are easy to install, secure, cross‐platform and dependency‐free. Snaps are discoverable and installable from the Snap Store, the app store for Linux with an audience of millions." snapcraft.io

### Prerequisites

Give the [snap](https://wiki.archlinux.org/title/Snap) entry on the Arch Linux wiki a read.
Below are some environment variables for export or you can add them to your `.zshrc` or `.zshenv` files.

**[XDG]() USER DIRS:**
```bash
export XDG_CACHE_HOME=$HOME/.cache
export XDG_DATA_HOME=$HOME/.local/share
```
{: file='~/.zshenv'}

**Go:**
```bash
export GOPATH=$XDG_DATA_HOME/go
export GOMODCACHE=$XDG_CACHE_HOME/go/mod
```
{: file='~/.zshenv'}
### Installation

Using [AUR](https://aur.archlinux.org/):
Your AUR helper of choice, below we will be using [yay](). The  [snapd](https://aur.archlinux.org/packages/snapd) package is maintained by [bboozzoo].

```bash
yay install snap
```


### Build That S<span style="color:#d79921">#!</span><span hidden>hi</span>t (BTS)

The guys a [Snapcraft](https://snapcraft.io/docs/installing-snap-on-arch-linux) have instructions on installing the snap daemon on Arch Linux that you can have a look at.
Give the Arch Wiki a read if you like for the [prerequisites](https://wiki.archlinux.org/title/Arch_User_Repository#Prerequisites) but I have also outlined the packages required below.

**Build dependencies:**
```bash
sudo pacman -S base-devel 
sudo pacman -S squashfs-tools autoconf-archive go go-tools python-docutils
```

<!--```bash
sudo modprobe squashfs
```
-->
<!--Log out and back in again, or restart your system
```bash
reboot
``` -->

This is a manual install of the AUR package.
First we clone the repository then we will `cd` into the working directory (snapd) and `makepkg` to build our version of `snapd` .

```bash
git clone https://aur.archlinux.org/snapd.git
cd snapd
makepkg -si
```

![](assets/img/posts/snapd/snapd-gitclone.png)

We need to enable the `snapd.socket` using `systemctl`.
```bash
sudo systemctl enable --now snapd.socket
```

[Snap confinement](https://snapcraft.io/docs/snap-confinement) and application sandboxing is available, it can be enabled by making sure you have [AppArmor](https://wiki.archlinux.org/title/AppArmor#Installation) installed.

We can check to see if he status [apparmor](https://archlinux.org/packages/extra/x86_64/apparmor/) to verify if its installed and enabled. 
```bash
systemctl status apparmor.service
```

![snapd-status](assets/img/posts/snapd/snapd-status.png)

Below we enable `apparmor` for the `snapd` daemon.
```bash
sudo systemctl enable --now snapd.apparmor.service
```

For classic `snap` support, we will need to create a symbolic link between `/var/lib/snapd/snap` and `/snap`.
```bash
sudo ln -s /var/lib/snapd/snap /snap
```

Log out and back in again, or restart your system
```bash
reboot
```

Once you have booted back up we can test if everything is working by installing a `snap` package.
If you already have something you want to install, give it a go, or you can have a look at the [Snap Store](https://snapcraft.io/docs/installing-snap-store-app), otherwise you can install the `hello-world` package to test your configuration is working.
```bash
sudo snap install hello-world
```

Now you can run `hello-world`
```bash
hello-world
```

We can use the same `hello-world` package to test confinement, by running:
```bash
hello-world.evil
```

![hello-world.evil](assets/img/posts/snapd/hello-world-evil.png)

---

**References:**

- <https://wiki.archlinux.org/title/Snap> - Arch Wiki entry for Snap
- <https://snapcraft.io/> - Homepage for Snapcraft

