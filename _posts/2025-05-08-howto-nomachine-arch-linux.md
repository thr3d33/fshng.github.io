---
title: "How To: NoMachine on Arch Linux"
description: Remote desktop software over the network
layout: post
date: 2025-05-08 12:00:00
modified: 2026-01-29T16:04:07+02:00
categories:
  - utilites
  - rdp
tags:
  - linux
  - remote-desktop
  - nomachine
  - archlinux
comments: false
image:
  path: assets/img/headers/howto-nomachine.webp
---
[NoMachine](https://www.nomachine.com/) allows you to access a graphical desktop of a computer over the network. Unlike some other remote desktop solutions, NoMachine does not require an intermediary server to establish the connection.
### Recommended System Requirements

**Hardware**
- Intel Core2 Duo, AMD Athlon Dual-Core or equivalent
- 1 GB RAM
- 110 MB free disk space
- Network connection

**Software**
- A desktop environment must already be installed.

>This app is not developed in the open, so only its developers know how it works. It may be insecure in ways that are hard to detect, and it may change without oversight.
{: .prompt-warning }
### Download

If you want to install NoMachine on a Arch Linux distribution, download the compressed NoMachine tar package from the web site.
You can head over to their downloads page to see what other products/services they offer
[NoMachine Downloads](https://www.nomachine.com/download) or head straight to the Linux download page [Linux Binaries](https://download.nomachine.com/download/?id=1&platform=linux)

### Verify

The NoMachine team provides a `md5` signature for verifying the packages you download form their website. 

```bash
md5sum <pkgName>_<pkgVersion>_<arch>.tar.gz | grep <md5_signature>
```

Example using latest package as of this post
```bash
md5sum nomachine_9.0.188_11_x86_64.tar.gz | grep d8fed96bce337b13b602593b850a432e
```

Output:
`d8fed96bce337b13b602593b850a432e *nomachine_9.0.188_11_x86_64.tar.gz`

### Extract

Install NoMachine or any of the server packages:

If you want to install to the default location `/usr/NX` ensure that package is placed there. Alternatively `/usr/local/bin/` and `/opt` can be used.  

> In this tutorial `/usr/local/bin` is used to extract the installation package.
> While the installation directory will be in `/opt`
{: .prompt-info }

**Extract the archive in the /usr/local/bin/ directory:**

```bash
cd /usr/local/bin/
sudo tar xvzf <pkgName>_<pkgVersion>_<arch>.tar.gz  
```

```bash
cd /usr/local/bin/
sudo tar xvzf nomachine_<pkgVersion>_<arch>.tar.gz  
```
### Install
 
> Read all scripts that you wish to run, especially those that require privileged access.
{: .prompt-warning }

**Run the `nxserver` script:**

>Adding the `redhat` flag after `--install` option is required when installing on Arch and Arch-based distros as its not officially supported.
{: .prompt-info }

Use `--install` option for installation.
```bash
cd NX 
sudo NX/nxserver --install redhat 
```

Alternatively, 
Using non-default location like `/opt`.
```bash
sudo NX_INSTALL_PREFIX=/opt /usr/local/bin/NX/nxserver --install redhat
```

Cleanup your directory:
```bash
cd /usr/local/bin/
sudo rm -rf nomachine_<pkgVersion>_<arch>.tar.gz README.md
```

---
#### Install via [AUR](https://aur.archlinux.org/):

There is a `nomachine` package on the Arch User Repository maintained by [runnytu](https://aur.archlinux.org/packages/nomachine) if you want to give that a try.

---
#### Post-Install:

Some useful environment variables for your rc file:
```shell
# Environment Variables
# Exporting the Installation directory for NX  
export NX_INSTALL_PREFIX=/opt
# Adding NX to PATH.
export NX_HOME=/opt/NX
export PATH=$PATH:$NX_HOME/bin
```

```bash
nxserver --version
```

Output: `NoMachine - Version <VERSION_NR>`

---
### Update

Use `--update` option to update your installation.
```bash
# This script executes another located in /etx/NX/
sudo /opt/NX/bin/nxserver --update
# Assuming you added $NX_HOME to your path
sudo $NX_HOME/bin/nxserver --update
sudo nxserver --update
```

---
### Uninstall

Use `--uninstall` option to uninstall your installation.
```bash
sudo /opt/NX/scripts/setup/nxserver --uninstall
# Assuming you added $NX_HOME to your path
sudo $NX_HOME/scripts/setup/nxserver --uninstall
```

Then remove the NX directory.
```bash
sudo rm -rf /opt/NX
```

---
<!--
**References:** <br>
[Arch Linux Wiki - NoMachine](https://wiki.archlinux.org/title/NoMachine) <br>
[NoMachine Knowledge Base - Linux Installation](https://kb.nomachine.com/DT07S00245#2.4) <br>
[NoMachine Knowledge Base - AR03L00789](https://kb.nomachine.com/AR03L00789) <br>
[NoMachine Knowledge Base - AR01L00775](https://kb.nomachine.com/AR01L00775)
-->