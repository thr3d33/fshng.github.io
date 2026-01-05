---
title: "How To: Install Jekyll (Static site generator)"
description: "Jekyll is a Static Site Generator"
layout: post
date: 2025-07-15 11:33:00
categories: 
tags: 
comments: false
image: 
  path: assets/img/headers/howto-jekyll-arch.webp 
---
## Install Jekyll on Arch Linux and Arch-based distributions
##### Prerequisites

Jekyll requires the following:

- Ruby version **2.7.0** or higher `ruby -v`
- RubyGems `gem -v`
- GCC `gcc -v`  & `g++ -v` 
- Make `make -v`

Assuming:
```bash
# XDG USER DIRS
# https://wiki.archlinux.org/title/XDG_Base_Directory#Supported
# https://wiki.archlinux.org/title/Ruby#RubyGems 
export XDG_CONFIG_HOME=$HOME/.config
export XDG_CACHE_HOME=$HOME/.cache
export XDG_DATA_HOME=$HOME/.local/share
export XDG_STATE_HOME=$HOME/.local/state
```

```bash
# Arch Wiki - RubyGems
export GEM_HOME="$(gem env user_gemhome)"
export PATH="$PATH:$GEM_HOME/bin"
# Arch Wiki - RubyBundler & Docs https://bundler.io/v2.5/man/bundle-config.1.html#CONFIGURE-BUNDLER-DIRECTORIES
export BUNDLE_USER_CACHE=$XDG_CACHE_HOME/bundle
export BUNDLE_USER_CONFIG=$XDG_CONFIG_HOME/bundle/config
export BUNDLE_USER_PLUGIN=$XDG_DATA_HOME/bundle
```



```bash
## Jekyll webiste
echo '# Ruby Gems' >> ~/.bashrc ##~/.zshrc
echo 'export GEM_HOME="$XDG_CONFIG_HOME/gems"' >> ~/.bashrc ##~/.zshrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc ##~/.zshrc
# Arch Wiki Ruby
echo 'export GEM_HOME="$(gem env user_gemhome)"' >> ~/.bashrc ##~/.zshrc
echo 'export PATH="$PATH:$GEM_HOME/bin"' >> ~/.bashrc ##~/.zshrc
source ~/.bashrc ##~/.zshrc
```

Arch Linux and Arch-based: 
```bash
sudo pacman -Syu
# Usually installed by default
sudo pacman -S gcc make
# Main packages
sudo pacman -S ruby rubygems base-devel ruby-erb
```

Installing Jekyll:
```bash
gem install jekyll 
gem install bundler
# One-liner
gem install jekyll bundler
```

Install project dependencies:
```bash
cd <PROJECT_DIR>
bundle --version
bundle
```

Jekyll serve:
```bash
bundle exec jekyll serve
```

http://localhost:4000

Theme:
https://chirpy.cotes.page/posts/getting-started/

```bash
services:
  jekyll:
    image: jekyll/jekyll
    volumes:
      - .:/srv/jekyll
      - ./vendor/bundle/:/usr/local/bundle
    ports:
      - "4000:4000"
    command: jekyll serve --force_polling --drafts
```

---
<!---
Links:
https://jekyllrb.com/docs/installation/#requirements
https://jekyllrb.com/docs/installation/other-linux/
https://jekyllrb.com/docs/installation/ubuntu/
Troubleshooting:
On Arch:
https://cristalcorp.github.io/blog/2024/09/20/Jekyll_On_Arch.html
Arch Wiki:
https://wiki.archlinux.org/title/XDG_Base_Directory#Supported
https://wiki.archlinux.org/title/Ruby#RubyGems
--->
