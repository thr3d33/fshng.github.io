---
title: "How To: Raspberry PI Imager"
description: I want more raspberries.
layout: post
date: 2026-01-30 09:33
modified: 2026-01-30 17:57
categories:
  - embeded
  - singleboard
  - raspberrypi
  - os
tags:
  - archlinux
  - raspberrypi
  - rpi
comments: false
image:
  path: assets/img/headers/howto-imager-rpi-arch.webp
---

> If you find any of the software shown here useful and want to show the developers some ❤️, you might consider giving them a ⭐ on GitHub or perhaps fuel their next update or release by buying them a ☕, even better would be something a little stronger 🍺 by becoming a Patron on Patreon.
{: .prompt-tip }

> Raspberry Pi have a section on the data they collect and why on their GItHub page [here](https://github.com/raspberrypi/rpi-imager?tab=readme-ov-file#anonymous-metrics-telemetry).
> Give it a read if you are concerned about privacy.
{: .prompt-info }

## Installation:

### Pacman

[Imager](https://archlinux.org/packages/extra/x86_64/rpi-imager/) is in the Arch Linux package repository and is maintained by [Christian Heusel](https://archlinux.org/packages/?maintainer=gromit "View packages maintained by Christian Heusel").

Update your system and Install using `pacman`:
```shell
sudo pacman -Syu
sudo pacman -S rpi-imager
```
{: .nolineno }

![](/assets/img/posts/rpi-imager/rofi-rpi-imager.png)

### AppImage:

[Raspberry Pi](https://www.raspberrypi.com/) release an AppImage for Linux users of [Imager](https://www.raspberrypi.com/software/)  on their software downloads page, alternatively you can head over to their [GitHub page](https://github.com/raspberrypi/rpi-imager) and have a look at the [releases](https://github.com/raspberrypi/rpi-imager/releases) section for the latest version of Imager.

![](/assets/img/posts/rpi-imager/downloads-page.png)

I have a post on [How To: AppImage on Arch Linux](https://fshng.xyz/posts/howto-appimage/), be sure to have a look if you don't know how to use them yet. 

Simply run:
```shell
chmod +x imager_2.0.6_amd64.AppImage
./imager_2.0.6_amd64.AppImage
```
{: .nolineno }

### Snap

I have released a post on [How To: Install Snap on Arch Linux](https://fshng.xyz/posts/howto-snap/) so have a look at that if you don't have it installed already. If you do, go ahead and install using `snap` by running:

```shell
sudo snap install rpi-imager
```
{: .nolineno }

![](/assets/img/posts/rpi-imager/rpi-imager-gui.png)

That's it.
You are ready to install an OS on your Raspberry Pi device.

> When in doubt, read the docs.
> Have a a look [here](https://www.raspberrypi.com/documentation/computers/getting-started.html#raspberry-pi-imager) to get started.
{: .prompt-tip }

---

**References:**
- <https://www.raspberrypi.com/> - Raspberry Pi Homepage 
- <https://github.com/raspberrypi/rpi-imager> Raspberry Pi GitHub page for Imager
- <https://www.raspberrypi.com/documentation/computers/getting-started.html#raspberry-pi-imager> - Getting Started documentation for Raspberry Pi
