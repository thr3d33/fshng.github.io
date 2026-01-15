---
title: "How To: Snap on Arch"
description:
date: 2025-09-07 00:00:00
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
### Installation


Using AUR:
Your AUR helper of choice, below we will be using [yay]()

```bash
yay install snap
```

You are always able to build it yourself.

Build requirements:
```console
 sudo pacman -S base-devel 
```

```bash
sudo pacman -S squashfs-tools autoconf-archive go go-tools python-docutils
```

```bash
sudo modprobe squashfs
```

Log out and back in again, or restart your system
```bash
reboot
```



```bash
git clone https://aur.archlinux.org/snapd.git
cd snapd
makepkg -si
```

```bash
sudo systemctl enable --now snapd.socket
```

```bash
sudo systemctl enable --now snapd.apparmor.service
```

```bash
systemctl status apparmor.service
```

```bash
sudo ln -s /var/lib/snapd/snap /snap
```

Log out and back in again, or restart your system
```bash
reboot
```

---

**References:**

- https://wiki.archlinux.org/title/Snap - Arch Wiki entry for Snap
- https://snapcraft.io/ - Homepage for Snapcraft

