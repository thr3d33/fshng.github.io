---
title: "How To: Steam on Arch Linux"
description: "Gaming with Steam on Arch Linux"
layout: post
date: 2025-08-01 00:00:00
categories: [utiites]
tags: [linux, steam, gaming]
comments: false
image:
  path: assets/img/headers/howto-steam-arch.webp
---

**Steam:**

Pacman:
```bash
sudo pacman -S steam
```

Snap:
```bash
sudo snap install steam
```

Flatpak:
```bash
## Install
flatpak install flathub com.valvesoftware.Steam
## Run
flatpak run com.valvesoftware.Steam
```

**Steamlink:**

1. Install [Flatpak](2025-06-12-howto-flatpak-arch-linux.md)

2. Install the `steamlink` flatpak from [flathub](https://flathub.org/apps/com.valvesoftware.SteamLink).

```bash
flatpak install flathub com.valvesoftware.SteamLink
```

```bash
flatpak run com.valvesoftware.SteamLink
```
---

**Reference:** 
- https://wiki.archlinux.org/title/Steam
- https://snapcraft.io/steam
