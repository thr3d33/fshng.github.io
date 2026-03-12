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

## Installation: LXC

##### Update system:

- Make sure everything is up-to-date on your Arch Linux system with a package repository update and system upgrade.
```shell
sudo pacman -Syu
```
{: .nolineno }

![](/assets/img/posts/lxc-lxd/pacman-update.png)

The beginning section of this **How: To** will be dealing with LXC directly and how to interact with the service without a manager like [LXD]() and [Incus]() (More on this at a later stage).

##### Install:

- Install the [lxc](https://archlinux.org/packages/extra/x86_64/lxc/) (maintained by [Sergej Pupykin](https://archlinux.org/packages/?maintainer=spupykin "View packages maintained by Sergej Pupykin") and [Morten Linderud](https://archlinux.org/packages/?maintainer=Foxboron "View packages maintained by Morten Linderud")) & [arch-install-scripts](https://archlinux.org/packages/extra/any/arch-install-scripts/) (maintained by [David Runge](https://archlinux.org/packages/?maintainer=dvzrv "View packages maintained by David Runge") and [Morten Linderud](https://archlinux.org/packages/?maintainer=Foxboron "View packages maintained by Morten Linderud")) packages using `pacman`.
```bash
sudo pacman -S lxc arch-install-scripts
```
{: .nolineno }

![](/assets/img/posts/lxc-lxd/lxc-arch-install-scripts.png)

Once installed they will allow you to run **privileged** containers on your host system.

## Configuration: LXC

LXC supports two different types of containers, they are **privileged** and **unprivileged**.
**Privileged** containers, the root UID within the container is mapped to the root UID on a host. 
 **Unprivileged** containers, the root UID within the container is mapped to an unprivileged UID on the host, this means that even if an adversary is able to gain access to your container and escape, they would find that they ideally, have limited to no rights on the host system. **Unprivileged** containers are considered safe by default thus Arch [linux](https://archlinux.org/packages/?name=linux), [linux-lts](https://archlinux.org/packages/?name=linux-lts) and [linux-zen](https://archlinux.org/packages/?name=linux-zen) kernel packages currently provide out-the-box support for **unprivileged** containers.
 
 > Use [privileged](https://linuxcontainers.org/lxc/security/#privileged-containers) containers with care. There are a number of exploits which will let you escape such containers and get full root privileges on the host. If they are specifically required for your purposes then it is recommended to create them as simple as possible.
{: .prompt-danger }

##### Using [unprivileged](https://linuxcontainers.org/lxc/security/#unprivileged-containers) containers:

First we want to enable support for **unprivileged** containers.

- We will modify the `/etc/lxc/defautlt.conf` file and add the below lines:
```shell
lxc.idmap = u 0 100000 65536
lxc.idmap = g 0 100000 65536
```
{: .nolineno }
{: file='/etc/lxc/defautlt.conf'}

![](assets/img/posts/lxc-lxd/more-default-conf.png)

This will map a range of `65536` consecutive UIDs, starting from UID `0` on the container-side which will be mapped to `100000` on the host-side, meaning we are allocating the UID `100000` right up until UID `165535` (`100000` - `165535`) for use with our Linux containers.

![](/assets/img/posts/lxc-lxd/host-container-lxc-uid-gid.png)

Then we create or modify our [subordinate user ID](https://man.archlinux.org/man/subuid.5) and [subordinate group ID](https://man.archlinux.org/man/subgid.5) files, `/ets/subuid` and `/etc/subgid` respectively.  

Here we will allocate the containerised mapping range to each user who will interact the `lxc` service or in this case run Linux containers. 
Below is an example for the `root` user, once done any container that is created by the `root` user will only run as **unprivileged**.

- Add your user to the `/etc/subuid`{: .filepath}. file.
Below is an example where allocate the `root` user with the `UID` and `GID` mapping in the `/etc/subuid`{: .filepath}. file.
```shell
root:100000:65536
```
{: .nolineno }
{: file='/etc/subuid'}

- Add your user to the `/etc/subgid`{: .filepath}. file. 
Below is an example where allocate the `root` user with the `UID` and `GID` mapping in the `/etc/subgid`{: .filepath}. file.
```shell
root:100000:65536
```
{: .nolineno }
{: file='/etc/subgid'}

![](assets/img/posts/lxc-lxd/subuid-subgid.png)

##### Control groups (cgroups):

>Have a look at [Rootless Containers: Enabling CPU, CPUSET, and I/O delegation](https://rootlesscontaine.rs/getting-started/common/cgroup2/#enabling-cpu-cpuset-and-io-delegation) for more information on rootless containers and delegating resources.
{: .prompt-info }

Running **unprivileged** containers as an **unprivileged** user only works if you delegate a [cgroup](https://wiki.archlinux.org/title/Cgroup "Cgroup") in advance, for  the user to control **cpu** and **io** resources.

We do this with a drop-in-file and delegate **unprivileged** [cgroups](https://wiki.archlinux.org/title/Cgroup "Cgroup") by creating a `systemd` unit.

![](assets/img/posts/lxc-lxd/cgroups-delegate.png)

```shell
[Service]
Delegate=cpu cpuset io
```
{: .nolineno }
{: file="/etc/systemd/system/user@1000.service.d/delegate.conf"}

- Now do a quick reboot.

We can verify our changes were made permanent checking the slice the user session is under has **cpu** and **io** controller permissions in the `/sys/fs/cgroup/user.slice/user-1000.slice/cgroup.controllers`{: .filepath}. file.
```shell
cat /sys/fs/cgroup/user.slice/user-1000.slice/cgroup.controllers
```
{: .nolineno }

```shell
cpuset cpu io memory pids
```

![](assets/img/posts/lxc-lxd/verify-cgroups.png)

{: .nolineno }
{: file='/sys/fs/cgroup/user.slice/user-1000.slice/cgroup.controllers'}

##### Network configuration:



```shell

```
{: .nolineno }


![](assets/img/posts/lxc-lxd/vi-lxcnet.png)

![](assets/img/posts/lxc-lxd/vi-lxc-net.png)

![](assets/img/posts/lxc-lxd/ip-a-before.png)

![](assets/img/posts/lxc-lxd/ip-a-after.png)

##### Firewall configuration:

In the below example we will use [ufw](https://archlinux.org/packages/extra/any/ufw/).

```shell
ufw allow in on lxcbr0
ufw route allow in on lxcbr0
``` 
{: .nolineno }

![](assets/img/posts/lxc-lxd/ufw-rules.png)

Running containers as a non-root user requires `+x` permissions on `~/.local/share/`. Make that change with [chmod](https://wiki.archlinux.org/title/Chmod "Chmod") before starting a container.

![](assets/img/posts/lxc-lxd/vi-usernet.png)

![](assets/img/posts/lxc-lxd/lxc-usernet.png)

#### Usage:

EXAMPLE:
create LXC:
```shell
lxc-create --name <CONTAINER_NAME> --template download -- --dist archlinux --release current --arch amd64
``` 
{: .nolineno }

![](assets/img/posts/lxc-lxd/lxc-create.png)

Verify:
```shell
more ~/.config/lxc/<CONTAINER_NAME>/config
``` 
{: .nolineno }

![](assets/img/posts/lxc-lxd/seraph-config.png)

##### Basic commands:

List all installed LXC containers:
```shell
lxc-ls -f 
``` 
{: .nolineno }

![](assets/img/posts/lxc-lxd/list-lxc.png)

start installed LXC containers:
```shell
lxc-start -n CONTAINER_NAME
``` 
{: .nolineno }

start installed LXC containers:
```shell
lxc-stop -n CONTAINER_NAME
``` 
{: .nolineno }

start installed LXC containers:
```shell
lxc-console -n CONTAINER_NAME
``` 
{: .nolineno }

---


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

---

## Installation: LXD

[lxd](https://archlinux.org/packages/extra/x86_64/lxd/)  (maintained by [Morten Linderud](https://archlinux.org/packages/?maintainer=Foxboron "View packages maintained by Morten Linderud") and [George Rawlinson](https://archlinux.org/packages/?maintainer=grawlinson "View packages maintained by George Rawlinson"))  package using `pacman`.
```bash
sudo pacman -S lxd
```
{: .nolineno }


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

