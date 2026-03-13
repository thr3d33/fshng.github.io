---
title: "How To: LXC and LXD on Arch Linux - Part 1"
description: You have some lovely cans.
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

> If you find any of the software shown here useful and want to show the developers some ❤️, you might consider giving them a ⭐ on GitHub or perhaps fuel their next update or release by buying them a ☕, even better would be something a little stronger 🍺 by becoming a Patron on [Patreon](https://www.patreon.com/) or supporting them using [LibrePay](https://liberapay.com/), [OpenCollective](https://opencollective.com/) or simply becoming a [Sponsor](https://github.com/open-source/sponsors) on their project on GitHub. A little love goes a long way with these developers. 
{: .prompt-tip }

## LXC (Linux Containers)
### Installation:
##### Update system:

- Make sure everything is up-to-date on your Arch Linux system with a package repository update and system upgrade.
```shell
sudo pacman -Syu
```
{: .nolineno }

![System Update](/assets/img/posts/lxc-lxd/pacman-update.png){:  } _System update with sudo pacman -Syu_

The beginning section of this **How: To** will be dealing with LXC directly and how to interact with the service without a manager like [LXD]() and [Incus]() (More on this at a later stage).

##### Install:

- Install the [lxc](https://archlinux.org/packages/extra/x86_64/lxc/) (maintained by [Sergej Pupykin](https://archlinux.org/packages/?maintainer=spupykin "View packages maintained by Sergej Pupykin") and [Morten Linderud](https://archlinux.org/packages/?maintainer=Foxboron "View packages maintained by Morten Linderud")) & [arch-install-scripts](https://archlinux.org/packages/extra/any/arch-install-scripts/) (maintained by [David Runge](https://archlinux.org/packages/?maintainer=dvzrv "View packages maintained by David Runge") and [Morten Linderud](https://archlinux.org/packages/?maintainer=Foxboron "View packages maintained by Morten Linderud")) packages using `pacman`.
```bash
sudo pacman -S lxc arch-install-scripts
```
{: .nolineno }

![](/assets/img/posts/lxc-lxd/lxc-arch-install-scripts.png){:  } _Install lxc and arch-install-scripts_

Once installed they will allow you to run **privileged** containers on your host system.

### Configuration:

LXC supports two different types of containers, they are **privileged** and **unprivileged**.
When using **Privileged** containers the root UID within the container is mapped to the root UID on a host, a compromised container could lead to a compromised host.
 **Unprivileged** containers however, have the root UID within the container mapped to an unprivileged UID on the host, this means that even if an adversary is able to gain access to your host by escaping the container, they would find that they ideally, have limited to no rights on the host system. **Unprivileged** containers are considered safe by default thus Arch [linux](https://archlinux.org/packages/?name=linux), [linux-lts](https://archlinux.org/packages/?name=linux-lts) and [linux-zen](https://archlinux.org/packages/?name=linux-zen) kernel packages currently provide out-the-box support for **unprivileged** containers.
 
 > Use [privileged](https://linuxcontainers.org/lxc/security/#privileged-containers) containers with care. There are a number of exploits which will let you escape such containers and get full root privileges on the host. If they are specifically required for your purposes then it is recommended to create them as simple as possible.
{: .prompt-danger }

##### Using [unprivileged](https://linuxcontainers.org/lxc/security/#unprivileged-containers) containers:

First we want to enable support for **unprivileged** containers.

- We will modify the `/etc/lxc/default.conf` file and add the below lines:
```shell
lxc.idmap = u 0 100000 65536
lxc.idmap = g 0 100000 65536
```
{: .nolineno }
{: file='/etc/lxc/default.conf'}

![](assets/img/posts/lxc-lxd/more-default-conf.png){:  } _Add mapping to the /etc/lxc/default.conf file_

This will map a range of `65536` consecutive UIDs, starting from UID `0` on the container-side which will be mapped to `100000` on the host-side, meaning we are allocating the UID `100000` right up until UID `165535` (`100000` - `165535`) for use with our Linux containers.

![](/assets/img/posts/lxc-lxd/host-container-lxc-uid-gid.png){:  } _A simple visual illustration of the UID and GID mapping with relation to the host and container_

Then we create or modify our [subordinate user ID](https://man.archlinux.org/man/subuid.5) and [subordinate group ID](https://man.archlinux.org/man/subgid.5) files, `/ets/subuid` and `/etc/subgid` respectively.  

Here we will allocate the containerised mapping range to each user who will interact  with the `lxc` service or in plain English, run Linux containers. 
Below is an example for the `root` user, once done any container that is created by the `root` user will only run as **unprivileged**.

- Add your user to the `/etc/subuid`{: .filepath}. file.
Below is an example where we allocate the `root` user with the `UID` and `GID` mapping in the `/etc/subuid`{: .filepath}. file.
```shell
root:100000:65536
```
{: .nolineno }
{: file='/etc/subuid'}

- Add your user to the `/etc/subgid`{: .filepath}. file. 
Below is an example where we allocate the `root` user with the `UID` and `GID` mapping in the `/etc/subgid`{: .filepath}. file.
```shell
root:100000:65536
```
{: .nolineno }
{: file='/etc/subgid'}

![](assets/img/posts/lxc-lxd/subuid-subgid.png){:  } _Mapping set for both root and $USER_

#### Control groups (cgroups):

>Have a look at [Rootless Containers: Enabling CPU, CPUSET, and I/O delegation](https://rootlesscontaine.rs/getting-started/common/cgroup2/#enabling-cpu-cpuset-and-io-delegation) for more information on rootless containers and delegating resources.
{: .prompt-info }

Running **unprivileged** containers as an **unprivileged** user only works if you delegate a [cgroup](https://wiki.archlinux.org/title/Cgroup "Cgroup") in advance, for  the user to control **cpu** and **io** resources.

```shell
systemd-run --unit=_myshell_ --user --scope -p "Delegate=yes" lxc-start _container_name_
```
{: .nolineno }

Alternatively,
We do this with a drop-in-file and delegate **unprivileged** [cgroups](https://wiki.archlinux.org/title/Cgroup "Cgroup") by creating a `systemd` unit.

```shell
[Service]
Delegate=cpu cpuset io
```
{: .nolineno }
{: file="/etc/systemd/system/user@1000.service.d/delegate.conf"}

![](assets/img/posts/lxc-lxd/cgroups-delegate.png){:  } _Delegation using a drop-in-file_

- Now do a quick reboot.

We can verify our changes were made permanent checking the slice the user session is under has **cpu** and **io** controller permissions in the `/sys/fs/cgroup/user.slice/user-1000.slice/cgroup.controllers`{: .filepath}. file.
- `cat` or `more` the `cgroup.controllers` file:
```shell
cat /sys/fs/cgroup/user.slice/user-1000.slice/cgroup.controllers
```
{: .nolineno }

Output:
```shell
cpuset cpu io memory pids
```

![](assets/img/posts/lxc-lxd/verify-cgroups.png){:  } _cat command to query the cgroup.controllers and verify the delegated resources_

{: .nolineno }
{: file='/sys/fs/cgroup/user.slice/user-1000.slice/cgroup.controllers'}


#### Network configuration:

LXC supports a number of virtual network types and devices, see [here](https://man.archlinux.org/man/lxc.container.conf.5#NETWORK). We will be using a bridge device for this walkthrough, to be specific we will be using a NAT bridge.

A NAT bridge is very useful when considering networking for your containers, LXC ships with `lxc-net` which creates a NAT bridge called `lxcbr0`, its a standalone bridge that is not connected to the host in any way through the hosts network device or the physical network. It exists in a private subnet within the host.

![](assets/img/posts/lxc-lxd/host-container-lxc-net-bridge.png){:  } _Illustration of the lxc-net bridge creating a private subnet 10.0.3.2 within the host_

Another option is a host bridge, it would mean the network manager on the host would provide the networking to the container. The host and container would then be on the same network and the Linux containers running on the host could be thought of as just another device on the network which would provide some benefit when working with network-exposed services like a webserver and is simple enough but it would also incur some risk as the containers would then widen your attack-surface and should also be thought of as an added threat vector.

Below we will be using a `lxc-net` NAT bridge.

![](assets/img/posts/lxc-lxd/host-container-host-bridge.png){:  } _Illustration of the container and host sharing a network and on the same subnet_

Think of `lxc-net` as a **Virtual NAT router**.

- `cd` into your `/etc/default` directory.
```shell
cd /etc/default
```
{: .nolineno }

![](assets/img/posts/lxc-lxd/vi-lxcnet.png){:  } _cd into /etc/default and sudo vi lxc-net_

- Create or edit the `lxc-net`file and change the `USE_LXC_BRIDGE` option to `true`.

```shell
sudo vi lxc-net
```
{: .nolineno }

![](assets/img/posts/lxc-lxd/vi-lxc-net.png){:  } _vi editor altering USE_LXC_BRIDGE option to true_

- You can view your network interface with the `ip` command and adding the `all` option. 

```shell
ip a
```
{: .nolineno }

![](assets/img/posts/lxc-lxd/ip-a-before.png){:  } _ip a on the host device before any change are made_

- Now we can query the `status`, `enable` and `start` the`lxc-net.service` with `systemctl` to get everything working.

```shell
sudo systemctl status lxc-net.service
sudo systemctl enable lxc-net.service
sudo systemctl start lxc-net.service
```
{: .nolineno }

![](assets/img/posts/lxc-lxd/lxc-net-service.png){:  } _Query the status of the lxc-net service and then enable and start the service._

- Now when we query our network interfaces again you can see the `lxcbr0`.

```shell
ip a
```
{: .nolineno }

![](assets/img/posts/lxc-lxd/ip-a-after.png){:  } _ip a on the host device after configuration, lxcbr0 now present_

#### Firewall configuration:

Depending on the firewall configured on your system some changes can be made to have the inbound packets go from the `lxcbr0` to the host and the outbound packets go from `lxcbr0` through the host and to other networks.

In the below example we will use [ufw](https://archlinux.org/packages/extra/any/ufw/).

- This can be done by running the below commands:
```shell
ufw allow in on lxcbr0
ufw route allow in on lxcbr0
``` 
{: .nolineno }

![](assets/img/posts/lxc-lxd/ufw-rules.png){:  } _Adding firewall rules for lxcbr0 using ufw_

An additional step is required according to [lxc-usernet(5)](https://man.archlinux.org/man/lxc-usernet.5), this will allow non-root users to create and start containers.

- We need to create and edit the `lxc-usernet` file in the `/etc/lxc`{: .filepath} directory.
```shell
sudo touch /etc/lxc/lxc-usernet
sudo vi /etc/lxc/lxc-usernet
```
{: .nolineno }

![](assets/img/posts/lxc-lxd/vi-usernet.png){:  } _Create and edit the lxc-usernet configuration file_

- Add the `user` (your user), `type` (only `veth`) is currently supported, `bridge` (`lxcbr0`) created in the previous section and the `number` which is the number of network interfaces of a given type to be allocated to a group or user in this case.
```shell
user type bridge number
```
{: .nolineno }
{: file='/etc/lxc/lxc-usernet'}


![](assets/img/posts/lxc-lxd/lxc-usernet.png){:  } _Verifying changes to the lxc-usernet file_

A copy of the `/etc/lxc/default.conf` should be put into the users LXC config directory `~/.config/lxc/default.conf`.

#### Permissions (Access)

**Easiest option:**<br>
Running containers as a non-root user requires `+x` permissions on `~/.local/share/`. Make that change with [chmod](https://wiki.archlinux.org/title/Chmod "Chmod") before starting a container.

If this is not possible then another option in available where use an [ACL](https://wiki.archlinux.org/title/Access_Control_Lists#Usage) entry with the execution permissions on the necessary directories. In this case `$HOME`, `$HOME/.local`, `$HOME/.local/share` and `$HOME/.local/share/lxc`

This option worked for me as I ran into permission issues when trying to start the LXC container the first time around. See be below [Troubleshooting]() section.

**More secure option:**<br>
- Running the below`setfacl` commands will grant `x` or `execute` permissions on the `~/`, `~/.local`, `~/.local/share`, `~/.local/share/lxc` directories respectively.

```shell
setfacl -m u:100000:x /home/<username>
setfacl -m u:100000:x /home/<username>/.local
setfacl -m u:100000:x /home/<username>/.local/share
setfacl -m u:100000:x /home/<username>/local/share/lxc
```
{: .nolineno }

![](assets/img/posts/lxc-lxd/acl-access.png){:  } _Granting execute permissions_

### Usage:

- We can create containers with `lxc-create` command.
```shell
lxc-create --name <CONTAINER_NAME> --template download -- --dist archlinux --release current --arch amd64
``` 
{: .nolineno }

![](assets/img/posts/lxc-lxd/lxc-create.png){:  } _Using lxc-create to create our first container_

- Verify:
```shell
more ~/.config/lxc/<CONTAINER_NAME>/config
``` 
{: .nolineno }

![](assets/img/posts/lxc-lxd/seraph-config.png){:  } _We query the newly created container config in our users local lxc directory ~/.config/lxc/_

##### Basic commands:

- List all installed LXC containers:
```shell
lxc-ls -f 
``` 
{: .nolineno }

![](assets/img/posts/lxc-lxd/list-lxc.png){:  } _List created containers_

- Start any created containers by running `lxc-start`
```shell
lxc-start -n CONTAINER_NAME
``` 
{: .nolineno }

![](assets/img/posts/lxc-lxd/lxc-start.png){:  } _Use lxc-start to start our Linux container_

- Verify status of Linux containers:
```shell
lxc-ls -f 
``` 
{: .nolineno }

![](assets/img/posts/lxc-lxd/lxc-start-verify.png){:  } _We can verify the status of the container with lxc-ls -f_

- We can then stop any container with the `lxc-stop` command:
```shell
lxc-stop -n CONTAINER_NAME
``` 
{: .nolineno }

![](assets/img/posts/lxc-lxd/lxc-stop.png){:  } _Stopping the container and verifying it has been stopped_

Now you have two commands you can use to connect to the LXC container, primarily `lxc-console` and `lxc-attach`.

`lxc-console` will provide you with a login shell. 
Once you login you can treat the the LXC containers like any Linux system where you can operate and administer accordingly (change, passwords, add users, install packages, etc).

Connect to created containers with `lxc-console`:
```shell
lxc-console -n CONTAINER_NAME
# Optional commands if the default <Ctrl+a q>
lxc-console -n CONTAINER_NAME --escape=PREFIX # --escape=e <Ctrl+e q>
``` 
{: .nolineno }

> If you use the `lxc-console` command then escaping the login shell after you connect can be a challenge, you will have to mash `Ctrl`, `a` and `q`  together to escape to your terminal like this **<Ctrl+a q>**.
> This can be changed by using the `--escape=<PREFIX>` flag.
{: .prompt-info }

![](assets/img/posts/lxc-lxd/lxc-console.png){:  } _Login shell after using lxc-console command_

Using the `lxc-attach` command will connect you to the LXC container, bypassing the the login and starting you with a root prompt inside the container.

Using the `--clear-env` flag ensures the host does not pass its own environment variables to the container, if this is preferred then you can leave it out, that being said, you may run into issues if your containers are based on another distribution. 

- Connect to container using `lxc-ttach`:
```shell
lxc-attach -n CONTAINER_NAME --clear-env
```
{: .nolineno }

![](assets/img/posts/lxc-lxd/lxc-attach.png){:  } _Root prompt after using lxc-attach command_

- If we are unhappy with any configuration or looking to free up space we can remove a container with `lxc-destroy`.
```shell
lxc-destroy -n CONTAINER_NAME -f
```
{: .nolineno }

![](assets/img/posts/lxc-lxd/lxc-destroy.png){:  } _Removing created container_

---

### Troubleshooting:

**Error:**
```text
Permission denied - Could not access /home/<user>. Please grant it x access, or add an ACL for the container root
```
{: .nolineno }

![](/assets/img/posts/lxc-lxd/lxc-start-error.png){:  } _Error when trying to start container_

![](/assets/img/posts/lxc-lxd/lxc-start-error-logout.png){:  } _Logging out for more information_

![](/assets/img/posts/lxc-lxd/tail-logfile.png){:  } _Tailing the log file generated for more details on errors_

**Cause:**

Container doesn't have permission to access to the user directories.

**Solution:**

```shell
setfacl -m u:100000:x /home/<username>
setfacl -m u:100000:x /home/<username>/.local
setfacl -m u:100000:x /home/<username>/.local/share
setfacl -m u:100000:x /home/<username>/local/share/lxc
```
{: .nolineno }

---
**References:**
- <https://wiki.archlinux.org/title/Linux_Containers> - Arch WIki enry for Linux Containers (LXC).


---

<img src="https://cloud.umami.is/p/4nRyAyDKe" width="1" height="1" border="0" alt="" style="display:block; max-height:0px; max-width:0px; overflow:hidden;">

