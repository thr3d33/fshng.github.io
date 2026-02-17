---
title: "How To: LXC and LXD on Arch Linux"
description: Lovely Cans
date: 2026-01-26 00:00:00
modified: 2026-02-17 17:48
layout: post
author: nyrrd
categories:
  - container
  - lxd
  - lxc
tags:
  - linux
  - how-to
  - archlinux
  - containers
  - lxc
  - lxd
  - snap
comments: false
image:
  path: assets/img/headers/howto-lxc-lxd-arch.webp
---

> If you find any of the software above useful and want to show the developers some ❤️, you might consider giving them a ⭐ on Github or perhaps fuel their next update by buying them a ☕, even better would be something a little stronger 🍺 by becoming a Patron on Patreon.
{: .prompt-tip }

## Installation:

##### Update system:

Update your system and Install the [LXC](https://wiki.archlinux.org/title/Linux_Containers) and [LXD](https://wiki.archlinux.org/title/LXD) packages, these are on the Arch Linux package repository.

Make sure everything is up-to-date with an system update.
```bash
sudo pacman -Syu
```
{: .nolineno }



##### Install:

Install the [lxc]() (maintained by [Sergej Pupykin](https://archlinux.org/packages/?maintainer=spupykin "View packages maintained by Sergej Pupykin") and [Morten Linderud](https://archlinux.org/packages/?maintainer=Foxboron "View packages maintained by Morten Linderud")) & [lxd](https://archlinux.org/packages/extra/x86_64/lxd/)  (maintained by [Morten Linderud](https://archlinux.org/packages/?maintainer=Foxboron "View packages maintained by Morten Linderud") and [George Rawlinson](https://archlinux.org/packages/?maintainer=grawlinson "View packages maintained by George Rawlinson"))  package using `pacman`.
```bash
sudo pacman -S lxd lxc
```
{: .nolineno }


## Configuration:

#### LXC

##### Using [unprivileged](https://linuxcontainers.org/lxc/security/#unprivileged-containers) containers:

Unprivileged containers safe by design.

> Use [privileged](https://linuxcontainers.org/lxc/security/#privileged-containers) containers with care. There are a number of exploits which will let you escape such containers and get full root privileges on the host.
{: .prompt-danger }

Example with `root`:
```shell
sudo usermod -v 1000000-1000999999 -w 1000000-1000999999 root
``` 
{: .nolineno }

Example with the `$USER`:
```shell
sudo usermod -v 1000000-1000999999 -w 1000000-1000999999 $USER # Your user
``` 
{: .nolineno }

Before we logout to lets `enable` and `start` the `lxc.service`

```shell
sudo systemctl enable lxc.service
sudo systemctl start lxc.service
sudo systemctl status lxc.service
```
{: .nolineno }
#### LXD
##### Add user to the LXD group:

> Adding any user to the `lxd` group will be granting them elevated privileges (root). Use with caution.
{: .prompt-warning }
 
```shell
sudo usermod -aG lxd $USER
```
{: .nolineno }


**Logout**

Log back in.



---
## Usage

Once you have logged back in.

Initialise `lxd`. This requires elevated privileges, if your user was added to the group correctly, it will initialise and prompt you with a series of configurations questions:

```shell
lxd init
```
{: .nolineno }



We can list the remote repositories we have access to by default.

```shell
lxc list remote
```
{: .nolineno }

We can also list the images that are present on the repository.

```shell
lxc image list images:
```
{: .nolineno }

In the below example we launch an `archlinux/amd64` Linux container from the `images` repository.

```shell
lxc launch images:archlinux/amd64 <container-name> # <container-name> is optional
```
{: .nolineno }

## Using [Snap](https://documentation.ubuntu.com/lxd/stable-5.21/installing/#how-to-install-lxd)



---
**References:**
- <https://wiki.archlinux.org/title/LXD> - Arch Wiki entry for LXD.
- <https://wiki.archlinux.org/title/Linux_Containers> - Arch WIki enry for Linux Containers (LXC).
- <https://documentation.ubuntu.com/lxd/stable-5.21/> - Ubuntu Documentation on LXD

---

<img src="https://cloud.umami.is/p/4nRyAyDKe" width="1" height="1" border="0" alt="" style="display:block; max-height:0px; max-width:0px; overflow:hidden;">

