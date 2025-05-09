---
title: "How To: NoMachine on Arch Linux"
description: "Remote Desktop Software"
layout: post
date: 2025-05-08 12:00:00
categories: 
tags: 
comments: false
image:
  path: assets/img/headers/howto-nomachine.webp
---
## Installation Notes for NoMachine on Arch and Arch-based Linux Distros

### Recommended System Requirements

**Hardware**
- Intel Core2 Duo, AMD Athlon Dual-Core or equivalent
- 1 GB RAM
- 110 MB free disk space
- Network connection (either a LAN, or Internet link: broadband, cable, DSL, etc.)

**Software**
- A desktop environment must already be installed.

### Download

If you want to install NoMachine on a Arch Linux distribution, download the compressed NoMachine tar package from the web site: <br>
[https://www.nomachine.com/download](https://www.nomachine.com/download)

### Extract

Install NoMachine or any of the server packages:

If you want to install to the **default location** /usr/NX ensure that package is placed there.

**Extract the archive in the /usr/NX directory:**

```bash
cd /usr  
sudo tar xvzf <pkgName>_<pkgVersion>_<arch>.tar.gz  
```

### Install

**Run the `nxserver` script:**

Use `--install` for installation.
```bash
sudo /usr/NX/nxserver --install 
sudo /usr/NX/nxserver --install redhat # Adding redhat for Arch linux installation, since there is no official install script for Arch
```

### Update

Use `--update` to update your installation.
```bash
sudo /usr/NX/nxserver --update
```

### Uninstall

Use `--uninstall` to uninstall your installation.
```bash
sudo /usr/NX/scripts/setup/nxserver --uninstall
```

Then remove the NX directory.
```bash
sudo rm -rf /usr/NX
```

#### AUR: [NoMachine](https://aur.archlinux.org/packages/nomachine)

There is a nomachine package on the Arch User Repository maintained by `runnytu` if you want to give that a try.

---

**References:** <br>
[Arch Linux Wiki - NoMachine](https://wiki.archlinux.org/title/NoMachine) <br>
[NoMachine Knowledge Base - Linux Installation](https://kb.nomachine.com/DT07S00245#2.4) <br>
[NoMachine Knowledge Base - AR03L00789](https://kb.nomachine.com/AR03L00789) <br>
[NoMachine Knowledge Base - AR01L00775](https://kb.nomachine.com/AR01L00775)

**AUR:** <br>
[Arch User Repository - NoMachine](https://aur.archlinux.org/packages/nomachine)

---