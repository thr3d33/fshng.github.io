---
title: "How To: Starship on Arch Linux"
description: A cross-shell rust prompt that is minimal and customizable.
layout: post
categories:
  - prompt
  - archlinux
  - shell
tags:
  - shell
comments: false
image:
  path: assets/img/headers/howto-starship-arch.webp
---

> If you find any of the software shown here useful and want to show the developers some ❤️, you might consider giving them a ⭐ on GitHub or perhaps fuel their next update or release by buying them a ☕, even better would be something a little stronger 🍺 by becoming a Patron on [Patreon](https://www.patreon.com/) or supporting them using [LibrePay](https://liberapay.com/), [OpenCollective](https://opencollective.com/) or simply becoming a [Sponsor](https://github.com/open-source/sponsors) on their project on GitHub. A little love goes a long way with these devs. 
{: .prompt-tip }

## What is [Starship](https://starship.rs/):

A cross-shell prompt that is designed to be minimal, fast and highly customizable for any shell and for any operating system powered by [Rust](https://rust-lang.org/).

This is usually a go to for me when I am working on my prompt and inevitably break something and need a workable solution on the fly, but easier options are available, it really depends on you and your needs. This is only my opinion. 

---

### Installation

**[XDG](https://wiki.archlinux.org/title/XDG_Base_Directory#Supported) USER DIRS:**<br>
Below are the [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir/latest/#index) environment variables for where to put files and configuration on your Linux system. This will ensure your `$HOME` is kept clean and **s<span style="color:#d79921">#!</span><span hidden>hi</span>t**. 😉
```shell
export XDG_CONFIG_HOME=$HOME/.config
export XDG_CACHE_HOME=$HOME/.cache
export XDG_DATA_HOME=$HOME/.local/share
export XDG_STATE_HOME=$HOME/.local/state
```
{: file='~/.zshenv'}
{: .nolineno }

Starship has [partial](https://wiki.archlinux.org/title/XDG_Base_Directory#Partial) XDG support. This mean its supports the standard but we have to add the variables manually. 😒

Add the below snippet to your .bashrc, .zshrc or .zshenv configuration file.
```shell
export STARSHIP_CONFIG="$XDG_CONFIG_HOME"/starship.toml
export STARSHIP_CACHE="$XDG_CACHE_HOME"/starship
```
{: file='~/.zshenv'}
{: .nolineno }

![](/assets/img/posts/starship/spaceship-bashrc.png)

Source your `rc` configuration file to make sure the changes are accessible to your terminal.

```shell
source .bashrc
```
{: .nolineno }


> `rc` files are your `.bashrc` or `.zshrc` configuration files, this will depend on what you are using and/or where you store your environment variables.
{: .prompt-info }

##### Prerequisites

[Starship](https://github.com/starship) does need a [Nerd Font](https://www.nerdfonts.com/) installed on your system and enabled and used on your terminal in order to render those lovely icons.

You can use your pre-installed font manager depending on the [Desktop Environment](https://wiki.archlinux.org/title/Desktop_environment) you are running, however Arch Linux has got you covered and you can install Nerd fonts using `pacman`.

A quick update on our package database:
```shell
sudo pacman -Syu
```
{: .nolineno }

Query the available Nerd Fonts:
```shell
pacman -Ss ttf | awk '/-nerd/'
```
{: .nolineno }

![](/assets/img/posts/starship/list-nerdfonts.png)

For individual fonts `ttf-<FONT_NAME>-nerd`.
Starship recommends [FiraCode](https://github.com/ryanoasis/nerd-fonts/releases/download/v3.4.0/FiraCode.zip).

On Arch Linux we have the [FiraCode](https://archlinux.org/packages/extra/any/ttf-firacode-nerd/) on the Arch package repository.

Install:
```shell
# Example - Installing FiraCode Nerd Font
sudo pacman -S ttf-firacode-nerd 
```
{: .nolineno }

![](/assets/img/posts/starship/font-install.png)

> This is technically a prerequisite, for now. In the future release of `starship` the Nerd Fonts wont be a requirement. The [No Nerd](https://starship.rs/presets/no-nerd-font) (No symbols) will be the default prompt on installation of `starship` removing any requirements to get up and going. See below Preset section or have a look at the presets for `starship` on their website [here](https://starship.rs/presets/).
{: .prompt-info }

`curl` is usually installed by default but in case it is not, you can have a look at the below snippet.

Install `curl`:
```shell
sudo pacman -S curl
```
{: .nolineno }

Now we are ready.

![](/assets/img/posts/starship/standby.png)

---

#### Installation Options:

You have two options for installing Starship.

<!-- ![](https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExMG9ndHl6ejdiNzdsNzAxbjlwa2N0aXdveWpkM2txN3d5YWlvNzVxdCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/NU96is98L0kuRIYkFm/giphy.gif) -->

<img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExMG9ndHl6ejdiNzdsNzAxbjlwa2N0aXdveWpkM2txN3d5YWlvNzVxdCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/NU96is98L0kuRIYkFm/giphy.gif">
<br>

#### Option One: Manual Install
Manual install using a install script provided by [Starship](https://starship.rs/guide/). 

> I recommend you always read scripts before you run them and/or run scripts only from sources you trust.
{: .prompt-danger }

I have added a copy of the script below which you can download using the above command or from their GitHub repository [here](https://github.com/starship/starship/blob/master/install/install.sh).

I have added a copy of the script at the end of the post for your reading pleasure.


Installation script:
```shell
curl -sS https://starship.rs/install.sh | sh
```
{: .nolineno }

![](/assets/img/posts/starship/install-script.png)

😓

Have a look that everything installed and you can use the `starship` command:
```shell
starship --version
```
{: .nolineno }

![](/assets/img/posts/starship/starship-version-manual.png)

Initialise `starship` in your `rc` file.
See below `.bashrc` example.

```shell
eval "$(starship init bash)"
```
{: .nolineno }
{: file='~/.bashrc'}

![](/assets/img/posts/starship/starship-bashrc.png)

Source your `rc` file so your changes can take effect.
```shell
source .bashrc # .zshrc
```
{: .nolineno }


#### Option Two: Package Manager

Package manager, [Starship](https://github.com/starship/starship) is supported by most Linux operating systems and their respective package managers and in this case `pacman` is no exception.

The [starship](https://archlinux.org/packages/extra/x86_64/starship/) package is maintained by [Maxime Gauduin](https://archlinux.org/packages/?maintainer=alucryd "View packages maintained by Maxime Gauduin"), [Caleb Maclennan](https://archlinux.org/packages/?maintainer=alerque "View packages maintained by Caleb Maclennan"), [Orhun Parmaksız](https://archlinux.org/packages/?maintainer=orhun "View packages maintained by Orhun Parmaksız"). 

```shell
sudo pacman -S starship
```
{: .nolineno }

![](/assets/img/posts/starship/starship-install-pacman.png)

```shell
starship --version
```
{: .nolineno }

![](/assets/img/posts/starship/starship-version-pacman.png)

Initialise `starship` in your `rc` file.
See below `.bashrc` example.

```shell
eval "$(starship init bash)"
```
{: .nolineno }
{: file='~/.bashrc'}

![](/assets/img/posts/starship/starship-bashrc.png)

Source your `rc` file so your changes can take effect.
```shell
source .bashrc # .zshrc
```
{: .nolineno }

---

### Configuration

#### Initial Configuration:

We need to create a `starship` configuration file `starship.toml` in our `~/.config/`{: .filepath} directory.

All configurations that are done in `starship` are done in a [TOML](https://github.com/toml-lang/toml) file:

```shell
touch ~/.config/starship.toml
```
{: .nolineno }


#### Setup:

Have a look at the configuration section for setting up your new prompt. This has all the information you need on setting everything up from start to finish.

- <https://starship.rs/config/#configuration>

#### Advanced Setup:

The Advanced configuration settings are for those lovely things like adding a [prompt](https://starship.rs/advanced-config/#enable-right-prompt) on the right and making them [transient](https://starship.rs/advanced-config/#transientprompt-and-transientrightprompt-in-bash).

- <https://starship.rs/advanced-config/#advanced-configuration>

### Presets:

Presets are a set of complete configurations you can use and/or learn from and modify. 
Below I have two options as an example.

##### [No Nerd Fonts](https://starship.rs/presets/no-nerd-font)

As I mentioned above, this will be the new default prompt in the later versions of `starship`. In the meantime we can make use of it by adding it to our `starship.toml` file.

Using the `starship` command:
```shell
starship preset no-nerd-font -o ~/.config/starship.toml
```
{: .nolineno }

or copy-paste:
```toml
"$schema" = 'https://starship.rs/config-schema.json'

[battery]
full_symbol = "• "
charging_symbol = "⇡ "
discharging_symbol = "⇣ "
unknown_symbol = "❓ "
empty_symbol = "❗ "

[erlang]
symbol = "ⓔ "

[fortran]
symbol = "F "

[nodejs]
symbol = "[⬢](bold green) "

[pulumi]
symbol = "🧊 "

[typst]
symbol = "t "
```
{: .nolineno }
{: file='~/.config/starship.toml'}


##### [Gruvbox Rainbow](https://starship.rs/presets/gruvbox-rainbow)

This is a favourite of mine since it uses the [Gruvbox](https://github.com/morhetz/gruvbox) theme. I think you might like it too.

This one will require the Nerd Fonts.

Using the `starship` command.
```shell
starship preset gruvbox-rainbow -o ~/.config/starship.toml
```
{: .nolineno }

Copy-paste:
```toml
"$schema" = 'https://starship.rs/config-schema.json'

format = """
[](color_orange)\
$os\
$username\
[](bg:color_yellow fg:color_orange)\
$directory\
[](fg:color_yellow bg:color_aqua)\
$git_branch\
$git_status\
[](fg:color_aqua bg:color_blue)\
$c\
$cpp\
$rust\
$golang\
$nodejs\
$php\
$java\
$kotlin\
$haskell\
$python\
[](fg:color_blue bg:color_bg3)\
$docker_context\
$conda\
$pixi\
[](fg:color_bg3 bg:color_bg1)\
$time\
[ ](fg:color_bg1)\
$line_break$character"""

palette = 'gruvbox_dark'

[palettes.gruvbox_dark]
color_fg0 = '#fbf1c7'
color_bg1 = '#3c3836'
color_bg3 = '#665c54'
color_blue = '#458588'
color_aqua = '#689d6a'
color_green = '#98971a'
color_orange = '#d65d0e'
color_purple = '#b16286'
color_red = '#cc241d'
color_yellow = '#d79921'

[os]
disabled = false
style = "bg:color_orange fg:color_fg0"

[os.symbols]
Windows = "󰍲"
Ubuntu = "󰕈"
SUSE = ""
Raspbian = "󰐿"
Mint = "󰣭"
Macos = "󰀵"
Manjaro = ""
Linux = "󰌽"
Gentoo = "󰣨"
Fedora = "󰣛"
Alpine = ""
Amazon = ""
Android = ""
AOSC = ""
Arch = "󰣇"
Artix = "󰣇"
EndeavourOS = ""
CentOS = ""
Debian = "󰣚"
Redhat = "󱄛"
RedHatEnterprise = "󱄛"
Pop = ""

[username]
show_always = true
style_user = "bg:color_orange fg:color_fg0"
style_root = "bg:color_orange fg:color_fg0"
format = '[ $user ]($style)'

[directory]
style = "fg:color_fg0 bg:color_yellow"
format = "[ $path ]($style)"
truncation_length = 3
truncation_symbol = "…/"

[directory.substitutions]
"Documents" = "󰈙 "
"Downloads" = " "
"Music" = "󰝚 "
"Pictures" = " "
"Developer" = "󰲋 "

[git_branch]
symbol = ""
style = "bg:color_aqua"
format = '[[ $symbol $branch ](fg:color_fg0 bg:color_aqua)]($style)'

[git_status]
style = "bg:color_aqua"
format = '[[($all_status$ahead_behind )](fg:color_fg0 bg:color_aqua)]($style)'

[nodejs]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[c]
symbol = " "
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[cpp]
symbol = " "
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[rust]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[golang]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[php]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[java]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[kotlin]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[haskell]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[python]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[docker_context]
symbol = ""
style = "bg:color_bg3"
format = '[[ $symbol( $context) ](fg:#83a598 bg:color_bg3)]($style)'

[conda]
style = "bg:color_bg3"
format = '[[ $symbol( $environment) ](fg:#83a598 bg:color_bg3)]($style)'

[pixi]
style = "bg:color_bg3"
format = '[[ $symbol( $version)( $environment) ](fg:color_fg0 bg:color_bg3)]($style)'

[time]
disabled = false
time_format = "%R"
style = "bg:color_bg1"
format = '[[  $time ](fg:color_fg0 bg:color_bg1)]($style)'

[line_break]
disabled = false

[character]
disabled = false
success_symbol = '[](bold fg:color_green)'
error_symbol = '[](bold fg:color_red)'
vimcmd_symbol = '[](bold fg:color_green)'
vimcmd_replace_one_symbol = '[](bold fg:color_purple)'
vimcmd_replace_symbol = '[](bold fg:color_purple)'
vimcmd_visual_symbol = '[](bold fg:color_yellow)'
```
{: .nolineno }
{: file='~/.config/starship.toml'}

Now doesn't this look the business.😎
![](/assets/img/posts/starship/starship-gruvbox-prompt.png)

You are set.
![](/assets/img/posts/starship/starship-gruvbox.png)

---

### Read The Script:

![](https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExdG1vOHo1ampvM2NoYjd2azNhajAycHJoZXRwMm5uOWdicDU1NGg1ZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/I1U9DTjCqOF3i/giphy.gif)

Copy of the [Install.sh](https://raw.githubusercontent.com/starship/starship/refs/heads/master/install/install.sh) below:
```shell
#!/usr/bin/env sh

set -eu
printf '\n'

BOLD="$(tput bold 2>/dev/null || printf '')"
GREY="$(tput setaf 0 2>/dev/null || printf '')"
UNDERLINE="$(tput smul 2>/dev/null || printf '')"
RED="$(tput setaf 1 2>/dev/null || printf '')"
GREEN="$(tput setaf 2 2>/dev/null || printf '')"
YELLOW="$(tput setaf 3 2>/dev/null || printf '')"
BLUE="$(tput setaf 4 2>/dev/null || printf '')"
MAGENTA="$(tput setaf 5 2>/dev/null || printf '')"
NO_COLOR="$(tput sgr0 2>/dev/null || printf '')"

SUPPORTED_TARGETS="x86_64-unknown-linux-gnu x86_64-unknown-linux-musl \
                  i686-unknown-linux-musl aarch64-unknown-linux-musl \
                  arm-unknown-linux-musleabihf x86_64-apple-darwin \
                  aarch64-apple-darwin x86_64-pc-windows-msvc \
                  i686-pc-windows-msvc aarch64-pc-windows-msvc \
                  x86_64-unknown-freebsd"

info() {
	printf '%s\n' "${BOLD}${GREY}>${NO_COLOR} $*"
}

warn() {
	printf '%s\n' "${YELLOW}! $*${NO_COLOR}"
}

error() {
	printf '%s\n' "${RED}x $*${NO_COLOR}" >&2
}

completed() {
	printf '%s\n' "${GREEN}✓${NO_COLOR} $*"
}

has() {
	command -v "$1" 1>/dev/null 2>&1
}

curl_is_snap() {
	curl_path="$(command -v curl)"
	case "$curl_path" in
	/snap/*) return 0 ;;
	*) return 1 ;;
	esac
}

# Make sure user is not using zsh or non-POSIX-mode bash, which can cause issues
verify_shell_is_posix_or_exit() {
	if [ -n "${ZSH_VERSION+x}" ]; then
		error "Running installation script with \`zsh\` is known to cause errors."
		error "Please use \`sh\` instead."
		exit 1
	elif [ -n "${BASH_VERSION+x}" ] && [ -z "${POSIXLY_CORRECT+x}" ]; then
		error "Running installation script with non-POSIX \`bash\` may cause errors."
		error "Please use \`sh\` instead."
		exit 1
	else
		true # No-op: no issues detected
	fi
}

get_tmpfile() {
	suffix="$1"
	if has mktemp; then
		printf "%s.%s" "$(mktemp)" "${suffix}"
	else
		# No really good options here--let's pick a default + hope
		printf "/tmp/starship.%s" "${suffix}"
	fi
}

# Test if a location is writeable by trying to write to it. Windows does not let
# you test writeability other than by writing: https://stackoverflow.com/q/1999988
test_writeable() {
	path="${1:-}/test.txt"
	if touch "${path}" 2>/dev/null; then
		rm "${path}"
		return 0
	else
		return 1
	fi
}

download() {
	file="$1"
	url="$2"

	if has curl && curl_is_snap; then
		warn "curl installed through snap cannot download starship."
		warn "See https://github.com/starship/starship/issues/5403 for details."
		warn "Searching for other HTTP download programs..."
	fi

	if has curl && ! curl_is_snap; then
		cmd="curl --fail --silent --location --output $file $url"
	elif has wget; then
		cmd="wget --quiet --output-document=$file $url"
	elif has fetch; then
		cmd="fetch --quiet --output=$file $url"
	else
		error "No HTTP download program (curl, wget, fetch) found, exiting…"
		return 1
	fi

	$cmd && return 0 || rc=$?

	error "Command failed (exit code $rc): ${BLUE}${cmd}${NO_COLOR}"
	printf "\n" >&2
	case "${VERSION}" in
	latest) ;;
	v*) ;;
	*)
		info "Note: Release tags include the 'v' prefix (e.g., 'v1.2.3')."
		info "You specified '${VERSION}'. Did you mean 'v${VERSION}'?"
		printf "\n" >&2
		;;
	esac
	info "This is likely due to Starship not yet supporting your configuration."
	info "If you would like to see a build for your configuration,"
	info "please create an issue requesting a build for ${MAGENTA}${TARGET}${NO_COLOR}:"
	info "${BOLD}${UNDERLINE}https://github.com/starship/starship/issues/new/${NO_COLOR}"
	return $rc
}

unpack() {
	archive=$1
	bin_dir=$2
	sudo=${3-}

	case "$archive" in
	*.tar.gz)
		flags=$(test -n "${VERBOSE-}" && echo "-xzvof" || echo "-xzof")
		${sudo} tar "${flags}" "${archive}" -C "${bin_dir}"
		return 0
		;;
	*.zip)
		flags=$(test -z "${VERBOSE-}" && echo "-qqo" || echo "-o")
		UNZIP="${flags}" ${sudo} unzip "${archive}" -d "${bin_dir}"
		return 0
		;;
	esac

	error "Unknown package extension."
	printf "\n"
	info "This almost certainly results from a bug in this script--please file a"
	info "bug report at https://github.com/starship/starship/issues"
	return 1
}

usage() {
	printf "%s\n" \
		"install.sh [option]" \
		"" \
		"Fetch and install the latest version of starship, if starship is already" \
		"installed it will be updated to the latest version."

	printf "\n%s\n" "Options"
	printf "\t%s\n\t\t%s\n\n" \
		"-V, --verbose" "Enable verbose output for the installer" \
		"-f, -y, --force, --yes" "Skip the confirmation prompt during installation" \
		"-p, --platform" "Override the platform identified by the installer [default: ${PLATFORM}]" \
		"-b, --bin-dir" "Override the bin installation directory [default: ${BIN_DIR}]" \
		"-a, --arch" "Override the architecture identified by the installer [default: ${ARCH}]" \
		"-B, --base-url" "Override the base URL used for downloading releases [default: ${BASE_URL}]" \
		"-v, --version" "Install a specific version of starship (e.g. v1.2.3) [default: ${VERSION}]" \
		"-h, --help" "Display this help message"
}

elevate_priv() {
	if ! has sudo; then
		error 'Could not find the command "sudo", needed to get permissions for install.'
		info "If you are on Windows, please run your shell as an administrator, then"
		info "rerun this script. Otherwise, please run this script as root, or install"
		info "sudo."
		exit 1
	fi
	if ! sudo -v; then
		error "Superuser not granted, aborting installation"
		exit 1
	fi
}

install() {
	ext="$1"

	if test_writeable "${BIN_DIR}"; then
		sudo=""
		msg="Installing Starship, please wait…"
	else
		warn "Escalated permissions are required to install to ${BIN_DIR}"
		elevate_priv
		sudo="sudo"
		msg="Installing Starship as root, please wait…"
	fi
	info "$msg"

	archive=$(get_tmpfile "$ext")

	# download to the temp file
	download "${archive}" "${URL}"

	# unpack the temp file to the bin dir, using sudo if required
	unpack "${archive}" "${BIN_DIR}" "${sudo}"
}

# Currently supporting:
#   - win (Git Bash)
#   - darwin
#   - linux
#   - linux_musl (Alpine)
#   - freebsd
detect_platform() {
	platform="$(uname -s | tr '[:upper:]' '[:lower:]')"

	case "${platform}" in
	msys_nt*) platform="pc-windows-msvc" ;;
	cygwin_nt*) platform="pc-windows-msvc" ;;
	# mingw is Git-Bash
	mingw*) platform="pc-windows-msvc" ;;
	# use the statically compiled musl bins on linux to avoid linking issues.
	linux) platform="unknown-linux-musl" ;;
	darwin) platform="apple-darwin" ;;
	freebsd) platform="unknown-freebsd" ;;
	esac

	printf '%s' "${platform}"
}

# Currently supporting:
#   - x86_64
#   - i386
#   - arm
#   - arm64
detect_arch() {
	arch="$(uname -m | tr '[:upper:]' '[:lower:]')"

	case "${arch}" in
	amd64) arch="x86_64" ;;
	armv*) arch="arm" ;;
	arm64) arch="aarch64" ;;
	esac

	# `uname -m` in some cases mis-reports 32-bit OS as 64-bit, so double check
	if [ "${arch}" = "x86_64" ] && [ "$(getconf LONG_BIT)" -eq 32 ]; then
		arch=i686
	elif [ "${arch}" = "aarch64" ] && [ "$(getconf LONG_BIT)" -eq 32 ]; then
		arch=arm
	fi

	printf '%s' "${arch}"
}

detect_target() {
	arch="$1"
	platform="$2"
	target="$arch-$platform"

	if [ "${target}" = "arm-unknown-linux-musl" ]; then
		target="${target}eabihf"
	fi

	printf '%s' "${target}"
}

confirm() {
	if [ -z "${FORCE-}" ]; then
		printf "%s " "${MAGENTA}?${NO_COLOR} $* ${BOLD}[y/N]${NO_COLOR}"
		set +e
		read -r yn </dev/tty
		rc=$?
		set -e
		if [ $rc -ne 0 ]; then
			error "Error reading from prompt (please re-run with the '--yes' option)"
			exit 1
		fi
		if [ "$yn" != "y" ] && [ "$yn" != "yes" ]; then
			error 'Aborting (please answer "yes" to continue)'
			exit 1
		fi
	fi
}

check_bin_dir() {
	bin_dir="${1%/}"

	if [ ! -d "$BIN_DIR" ]; then
		error "Installation location $BIN_DIR does not appear to be a directory"
		info "Make sure the location exists and is a directory, then try again."
		usage
		exit 1
	fi

	# https://stackoverflow.com/a/11655875
	good=$(
		IFS=:
		for path in $PATH; do
			if [ "${path%/}" = "${bin_dir}" ]; then
				printf 1
				break
			fi
		done
	)

	if [ "${good}" != "1" ]; then
		warn "Bin directory ${bin_dir} is not in your \$PATH"
	fi
}

print_install() {
	# if the shell does not fit the default case change the config file
	# and or the config cmd variable
	for s in "bash" "zsh" "ion" "tcsh" "xonsh" "fish"; do
		# shellcheck disable=SC2088
		# we don't want these '~' expanding
		config_file="~/.${s}rc"
		config_cmd="eval \"\$(starship init ${s})\""

		case ${s} in
		ion)
			# shellcheck disable=SC2088
			config_file="~/.config/ion/initrc"
			config_cmd="eval \$(starship init ${s})"
			;;
		fish)
			# shellcheck disable=SC2088
			config_file="~/.config/fish/config.fish"
			config_cmd="starship init fish | source"
			;;
		tcsh)
			config_cmd="eval \`starship init ${s}\`"
			;;
		xonsh)
			config_cmd="execx(\$(starship init xonsh))"
			;;
		esac

		printf "  %s\n  Add the following to the end of %s:\n\n\t%s\n\n" \
			"${BOLD}${UNDERLINE}${s}${NO_COLOR}" \
			"${BOLD}${config_file}${NO_COLOR}" \
			"${config_cmd}"
	done

	for s in "elvish" "nushell"; do

		warning="${BOLD}Warning${NO_COLOR}"
		case ${s} in
		elvish)
			# shellcheck disable=SC2088
			config_file="~/.config/elvish/rc.elv"
			config_cmd="eval (starship init elvish)"
			warning="${warning} Only elvish v0.17 or higher is supported."
			;;
		nushell)
			# shellcheck disable=SC2088
			config_file="${BOLD}your nu config file${NO_COLOR} (find it by running ${BOLD}\$nu.config-path${NO_COLOR} in Nushell)"
			config_cmd="mkdir (\$nu.data-dir | path join \"vendor/autoload\")
        starship init nu | save -f (\$nu.data-dir | path join \"vendor/autoload/starship.nu\")"
			warning="${warning} This will change in the future.
  Only Nushell v0.96 or higher is supported."
			;;
		esac
		printf "  %s\n  %s\n  And add the following to the end of %s:\n\n\t%s\n\n" \
			"${BOLD}${UNDERLINE}${s}${NO_COLOR}" \
			"${warning}" \
			"${config_file}" \
			"${config_cmd}"
	done

	printf "  %s\n  Add the following to the end of %s:\n  %s\n\n\t%s\n\n" \
		"${BOLD}${UNDERLINE}PowerShell${NO_COLOR}" \
		"${BOLD}Microsoft.PowerShell_profile.ps1${NO_COLOR}" \
		"You can check the location of this file by querying the \$PROFILE variable in PowerShell.
  Typically the path is ~\Documents\PowerShell\Microsoft.PowerShell_profile.ps1 or ~/.config/powershell/Microsoft.PowerShell_profile.ps1 on -Nix." \
		"Invoke-Expression (&starship init powershell)"

	printf "  %s\n  You need to use Clink (v1.2.30+) with Cmd. Add the following to a file %s and place this file in Clink scripts directory:\n\n\t%s\n\n" \
		"${BOLD}${UNDERLINE}Cmd${NO_COLOR}" \
		"${BOLD}starship.lua${NO_COLOR}" \
		"load(io.popen('starship init cmd'):read(\"*a\"))()"

	printf "\n"
}

is_build_available() {
	arch="$1"
	platform="$2"
	target="$3"

	good=$(
		IFS=" "
		for t in $SUPPORTED_TARGETS; do
			if [ "${t}" = "${target}" ]; then
				printf 1
				break
			fi
		done
	)

	if [ "${good}" != "1" ]; then
		error "${arch} builds for ${platform} are not yet available for Starship"
		printf "\n" >&2
		info "If you would like to see a build for your configuration,"
		info "please create an issue requesting a build for ${MAGENTA}${target}${NO_COLOR}:"
		info "${BOLD}${UNDERLINE}https://github.com/starship/starship/issues/new/${NO_COLOR}"
		printf "\n"
		exit 1
	fi
}

# defaults
if [ -z "${PLATFORM-}" ]; then
	PLATFORM="$(detect_platform)"
fi

if [ -z "${BIN_DIR-}" ]; then
	BIN_DIR=/usr/local/bin
fi

if [ -z "${ARCH-}" ]; then
	ARCH="$(detect_arch)"
fi

if [ -z "${BASE_URL-}" ]; then
	BASE_URL="https://github.com/starship/starship/releases"
fi

if [ -z "${VERSION-}" ]; then
	VERSION="latest"
fi

# Non-POSIX shells can break once executing code due to semantic differences
verify_shell_is_posix_or_exit

# parse argv variables
while [ "$#" -gt 0 ]; do
	case "$1" in
	-p | --platform)
		PLATFORM="$2"
		shift 2
		;;
	-b | --bin-dir)
		BIN_DIR="$2"
		shift 2
		;;
	-a | --arch)
		ARCH="$2"
		shift 2
		;;
	-B | --base-url)
		BASE_URL="$2"
		shift 2
		;;
	-v | --version)
		VERSION="$2"
		shift 2
		;;

	-V | --verbose)
		VERBOSE=1
		shift 1
		;;
	-f | -y | --force | --yes)
		FORCE=1
		shift 1
		;;
	-h | --help)
		usage
		exit
		;;

	-p=* | --platform=*)
		PLATFORM="${1#*=}"
		shift 1
		;;
	-b=* | --bin-dir=*)
		BIN_DIR="${1#*=}"
		shift 1
		;;
	-a=* | --arch=*)
		ARCH="${1#*=}"
		shift 1
		;;
	-B=* | --base-url=*)
		BASE_URL="${1#*=}"
		shift 1
		;;
	-v=* | --version=*)
		VERSION="${1#*=}"
		shift 1
		;;
	-V=* | --verbose=*)
		VERBOSE="${1#*=}"
		shift 1
		;;
	-f=* | -y=* | --force=* | --yes=*)
		FORCE="${1#*=}"
		shift 1
		;;

	*)
		error "Unknown option: $1"
		usage
		exit 1
		;;
	esac
done

TARGET="$(detect_target "${ARCH}" "${PLATFORM}")"

is_build_available "${ARCH}" "${PLATFORM}" "${TARGET}"

printf "  %s\n" "${UNDERLINE}Configuration${NO_COLOR}"
info "${BOLD}Bin directory${NO_COLOR}: ${GREEN}${BIN_DIR}${NO_COLOR}"
info "${BOLD}Platform${NO_COLOR}:      ${GREEN}${PLATFORM}${NO_COLOR}"
info "${BOLD}Arch${NO_COLOR}:          ${GREEN}${ARCH}${NO_COLOR}"

# non-empty VERBOSE enables verbose untarring
if [ -n "${VERBOSE-}" ]; then
	VERBOSE=v
	info "${BOLD}Verbose${NO_COLOR}: yes"
else
	VERBOSE=
fi

printf '\n'

EXT=tar.gz
if [ "${PLATFORM}" = "pc-windows-msvc" ]; then
	EXT=zip
fi

if [ "${VERSION}" != "latest" ]; then
	URL="${BASE_URL}/download/${VERSION}/starship-${TARGET}.${EXT}"
else
	URL="${BASE_URL}/latest/download/starship-${TARGET}.${EXT}"
fi

info "Tarball URL: ${UNDERLINE}${BLUE}${URL}${NO_COLOR}"
confirm "Install Starship ${GREEN}${VERSION}${NO_COLOR} to ${BOLD}${GREEN}${BIN_DIR}${NO_COLOR}?"
check_bin_dir "${BIN_DIR}"

install "${EXT}"
completed "Starship ${VERSION} installed"

printf '\n'
info "Please follow the steps for your shell to complete the installation:"

print_install
```
{: .nolineno }
{: file='~/.install.sh'}

---