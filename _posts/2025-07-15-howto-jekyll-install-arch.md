---
title: "How To: Install Jekyll (Static site generator)"
description: Jekyll is a Static Site Generator
layout: post
date: 2025-07-15 11:33:00
modified: 2026-02-06 13:30
categories:
  - web
  - ssg
tags:
  - web
  - ssg
  - jekyll
comments: false
image:
  path: assets/img/headers/howto-jekyll-arch.webp
---

> If you find any of the software shown here useful and want to show the developers some ❤️, you might consider giving them a ⭐ on GitHub or perhaps fuel their next update or release by buying them a ☕, even better would be something a little stronger 🍺 by becoming a Patron on Patreon.
{: .prompt-tip }

### Prerequisites

Jekyll requires the following:

- [Ruby](https://archlinux.org/packages/extra/x86_64/ruby/) version **2.7.0** or higher - `ruby -v`
- [RubyGems](https://archlinux.org/packages/extra/any/rubygems/) - `gem -v`
- [GCC](https://archlinux.org/packages/core/x86_64/gcc/) - `gcc -v`  & `g++ -v` 
- [Make](https://archlinux.org/packages/core/x86_64/make/)  - `make -v`

#### Installing Prerequisites

##### Environments variables:

Below are some environment variables for `export`,  you can add them to your `.zshrc`, `.zshenv` or `.bashrc` files.

**[XDG](https://wiki.archlinux.org/title/XDG_Base_Directory#Supported) USER DIRS:** <br>
Below are the [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir/latest/#index) environment variables for where to put files and configuration on your Linux system. This will ensure your `$HOME` is kept clean. 😉
```shell
export XDG_CONFIG_HOME=$HOME/.config
export XDG_CACHE_HOME=$HOME/.cache
export XDG_DATA_HOME=$HOME/.local/share
export XDG_STATE_HOME=$HOME/.local/state
```
{: file='~/.zshenv'}
{: .nolineno }

**[GO](https://wiki.archlinux.org/title/Ruby#RubyGems):** <br>
Here we are setting the `GEM_HOME` directory using the `gem` command (This will only be possible after you have installed it)  and then adding the path for `GEM_HOME` to our `PATH` environment variable. 
```shell
export GEM_HOME="$(gem env user_gemhome)"
export PATH="$PATH:$GEM_HOME/bin"
```
{: file='~/.zshenv'}
{: .nolineno }

**[Bundler](https://bundler.io/v2.5/man/bundle-config.1.html#CONFIGURE-BUNDLER-DIRECTORIES):** <br>
Bundler's home, cache and plugin directories and config file can be configured through environment variables. The default location for Bundler's home directory is `~/.bundle`. 😞
```shell
export BUNDLE_USER_CACHE=$XDG_CACHE_HOME/bundle
export BUNDLE_USER_CONFIG=$XDG_CONFIG_HOME/bundle/config
export BUNDLE_USER_PLUGIN=$XDG_DATA_HOME/bundle
```
{: file='~/.zshenv'}
{: .nolineno }

Below is a quick snippet of code that will `echo` the environment variables into your `.bashrc` or `.zshrc` depending on what you are running, simply comment out or delete the other.

```shell
## Jekyll webiste
echo '# Ruby Gems' >> ~/.bashrc ##~/.zshrc
echo 'export GEM_HOME="$XDG_CONFIG_HOME/gems"' >> ~/.bashrc ##~/.zshrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc ##~/.zshrc
# Arch Wiki Ruby
echo 'export GEM_HOME="$(gem env user_gemhome)"' >> ~/.bashrc ##~/.zshrc
echo 'export PATH="$PATH:$GEM_HOME/bin"' >> ~/.bashrc ##~/.zshrc
# Bundler
echo 'export BUNDLE_USER_CACHE=$XDG_CACHE_HOME/bundle' >> ~/.bashrc ##~/.zshrc
echo 'export BUNDLE_USER_CONFIG=$XDG_CONFIG_HOME/bundle/config' >> ~/.bashrc ##~/.zshrc
echo 'export BUNDLE_USER_PLUGIN=$XDG_DATA_HOME/bundle' >> ~/.bashrc ##~/.zshrc
source ~/.bashrc ##~/.zshrc
```
{: .nolineno }

Make sure to source your `.bashrc` or `.zshrc` files before you install the packages.

##### Package Installation

On Arch Linux and Arch-based we have `pacman`, update and then install `ruby`,  `rubygems`,  `base-devel` and `ruby-erb` packages. `gcc` and `make` are generally installed by default on most systems but in case it is not, make sure to install it as well.

You can run the below commands to `update` and `install` the packages:
```shell
sudo pacman -Syu
# Usually installed by default
sudo pacman -S gcc make
# Main packages
sudo pacman -S ruby rubygems base-devel ruby-erb
```
{: .nolineno }

Lets check that everything is installed by checking the `version` for each command.

`ruby --version`
![](/assets/img/posts/jekyll/ruby-version.png)

`gem --version`
![](/assets/img/posts/jekyll/gem-version.png)

---

### Installation:

Now that we have installed the prerequisites, we can use `gems` and begin installing `Jekyll` and `bundler`. <br>
We then run:
```shell
gem install jekyll 
gem install bundler
# One-liner
gem install jekyll bundler
```
{: .nolineno }

Check that everything is installed by querying the `version` for each command.

`jekyll --version`
![](/assets/img/posts/jekyll/jekyll-version.png)

`bundler --version`
![](/assets/img/posts/jekyll/bundler-version.png)

You are set.

---

### Build That S<span style="color:#d79921">#!</span><span hidden>hi</span>t (BTS)

#### Project/Theme:

Now that we have everything installed, the fun can begin. You can use `jekyll` and start a new site or you can use a theme to get your project started. Below I have outlined examples on both. <br> 
That being said, the developers put a lot of time into their documentation, never hesitate to give it a read when you are unsure of something. <br> 
Jekyll has some good docs and a section on getting started [here](https://jekyllrb.com/docs/).

##### Projects:

Starting a new blog:<br>
We create a new site by running below command:
```shell
# 'blog' is the name of the project (site), be sure to change it to your specification.
jekyll new blog
```
{: .nolineno }

![](/assets/img/posts/jekyll/jekyll-new-site.png)

`cd` into your new site directory, in this case `blog/` :
```shell
cd blog
```
{: .nolineno }

![](/assets/img/posts/jekyll/ls-site.png)
Build and run your `jekyll` site with the below command:
```shell
bundle exec jekyll serve
```
{: .nolineno }

**Visit your development site:** <br>
Your `localhost` on port `4000`
- <http://localhost:4000> - `localhost:4000`

> If you have a theme you want to install, there will usually be build instructions in the themes repository and generally in the `README.md` or in their documentation 📚 , these open-source developers are thoughtful that way. 😎
{: .prompt-info }

##### Theme:

Below is only a general outline, some themes might have specific dependencies and requirements, be sure to have a look at the `README.md`.

**Install project dependencies.** <br>
Build and install dependencies for your theme by running `bundle --install`:
```shell
cd <PROJECT_DIR>
bundle --version
bundle install
```
{: .nolineno }

You can then `serve` your development site on your <http://localhost> by running:
```shell
bundle exec jekyll serve
```
{: .nolineno }

**Visit your development site:**<br>
Your `localhost` on port `4000`
- <http://localhost:4000> - `localhost:4000`

**Real world example:**

This blog is a [fork](https://github.com/cotes2020/jekyll-theme-chirpy) of a theme that was created by this lovely animal, [Cotes Chung](https://github.com/cotes2020).  <br>
If you would like to know how I installed it, have a look at this developers documentation page on getting started with GitHub pages [here](https://chirpy.cotes.page/posts/getting-started/).

> If you like this theme and the work that [Cotes Chung](https://chirpy.cotes.page/) has done, you can always show your appreciation by clicking that  ⭐ and/or supporting their work with a ☕.
{: .prompt-tip }

---

**References:**
- <https://jekyllrb.com/docs/installation/#requirements> - Jekyll installation requirements. 
- <https://jekyllrb.com/docs/installation/other-linux/> - Jekyll documentation for installing on other Linux systems
- <https://jekyllrb.com/docs/installation/ubuntu/> -  Jekyll documentation for installation on Ubuntu as reference for other Linux operating systems.
- <https://wiki.archlinux.org/title/XDG_Base_Directory#Supported> - The Arch Wiki entry for XDG application and package support.
- <https://wiki.archlinux.org/title/Ruby#RubyGems> - Arch Wiki entry for Ruby Gems
- -  [ ] Buy A Beer. <https://cristalcorp.github.io/blog/2024/09/20/Jekyll_On_Arch.html> - Helpful blog post for installing Jekyll on Arch Linux. 
