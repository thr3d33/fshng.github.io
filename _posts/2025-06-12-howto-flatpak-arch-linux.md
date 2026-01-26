---
title: "How To: Flatpak on Arch Linux"
description: Flatpak is a system for building, distributing and running sandboxed desktop applications on Linux.
date: 2025-06-12 11:33:00
layout: post
categories: [packages]
tags: [linux, flatpak, how-to, archlinux]
comments: false
image: 
  path: assets/img/headers/howto-flatpak-arch.webp
---
> "[Flatpak](https://flatpak.org/) is a system for building, distributing and running sandboxed desktop applications on Linux."

### Install

>- Many Flatpak applications available on flathub are not effectively sandboxed by default . Do not rely on the provided process isolation without first reviewing the related flatpak permission manifest for common sandbox escape issues.
>- Running untrusted code is never safe; sandboxing cannot change this.
{: .prompt-warning }

Flatpak is available on the Arch as part of the `Extra` repository.

To install [Flatpak](https://archlinux.org/packages/extra/x86_64/flatpak/) on Arch Linux, we will open a terminal and run:

```bash
sudo pacman -S flatpak
```
#### Reboot

Reboot your system.
#### Verify

You can check that everything is working, by running:
Verify version:
```bash
flatpak --version
```

![](assets/img/posts/flatpak/flatpak-version.png)

> The installation of flatpak will, by default, add the official [Flathub repository](https://flathub.org/) as a system-wide installation.
 {: .prompt-info }

Run the below command to list installed repositories:
Verify remotes:
```bash
flatpak remotes
```
---
### Install a Application with Flatpak

Head over to [Flathub](https://flathub.org/) and checkout the available applications you might want to install.

>In this example we are going to install an open source IDE called [vscodium](https://flathub.org/apps/com.vscodium.codium).
{: .prompt-info }

> If you find any of the software above useful and want to show the developers some ❤️, you might consider giving them a ⭐ on Github or perhaps fuel their next update by buying them a ☕, even better would be something a little stronger 🍺 by becoming a Patron on Patreon.
{: .prompt-tip }

Before we are able to search a newly added repository, we are going to add the `update` flag to the `flatpak` command to download the appstream data for repositories:
```bash
flatpak update
```

You can `search` for [vscodium](https://vscodium.com/) using the search option:
```bash
flatpak search vscodium
```

To Install, open up a terminal and run:
```bash
flatpak install flathub com.vscodium.codium
```

To run `vscodium`: 
```bash
# Depending on your system, you will probably be able to run as codium
flatpak run com.vscodium.codium
```

List installed applications:
```bash
flatpak list
```
### Uninstall

To Remove:
```bash
flatpak uninstall com.vscodium.codium
```

> Be sure to have a look at the [Manifest](https://github.com/flathub/com.vscodium.codium) for VSCodium.
{: .prompt-info }


Any issues you may run into might already be addressed so have a look at the [Troubleshooting](https://wiki.archlinux.org/title/Flatpak#Troubleshooting)  section on the Arch Wiki entry for Flatpak. Any and all information you might need for configuration and commands can be found on the Arch Wiki.

---
**Reference:**

- <https://wiki.archlinux.org/title/Flatpak> - The Arch Wiki entry for Flatpak
- <https://flatpak.org/setup/Arch> - Flatpak page for install on Arch Linux
- <https://flathub.org/> - Homepage for Flathub the application repository.
- <https://wiki.archlinux.org/title/Flatpak#Troubleshooting> - Troubleshooting Flatpak on Arch Linux