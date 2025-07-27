---
title: "How To: Flatpak on Arch Linux"
description: 
date: 2025-06-12 11:33:00
layout: post
categories: 
tags: 
comments: false
---
 "[Flatpak](https://flatpak.org/) is a system for building, distributing and running sandboxed desktop applications on Linux."
### Install Flatpak

>- Many Flatpak applications available on flathub are not effectively sandboxed by default . Do not rely on the provided process isolation without first reviewing the related flatpak permission manifest for common sandbox escape issues.
>- Running untrusted code is never safe; sandboxing cannot change this.
{: .prompt-warning }

Flatpak is available on the Arch as part of the `Extra` repository.

To install [Flatpak](https://flatpak.org/) on Arch Linux, we will open a terminal and run:

```bash
sudo pacman -S flatpak
```
### Reboot

Reboot your system.

```bash
flatpak --version
```

---
### Install a Application

Head over to [Flathub](https://flathub.org/) Below is an example.
We are going to install [vscodium](https://flathub.org/apps/com.vscodium.codium).

To Install, open up a terminal and run:
```bash
flatpak install flathub com.vscodium.codium
```

To Run: 
```bash
# Depending on your system, you will probably be able to run as codium
flatpak run com.vscodium.codium
```

### Uninstall

To Remove:
```bash
flatpak uninstall com.vscodium.codium
```

> Be sure to have a look at the [Manifest](https://github.com/flathub/com.vscodium.codium) for VSCodium.
{: .prompt-info }
---
<!---
Reference:
Arch Wiki:
https://wiki.archlinux.org/title/Flatpak
Flatpak:
https://flatpak.org/setup/Arch
Flathub:
https://flathub.org/
https://flathub.org/setup/Arch
--->