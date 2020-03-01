---
layout: post
title: "Moved My Site - the Installation memory for Ubuntu 18.04"
date: 2020-03-01T20:43:10+08:00
author: tigressbailey
sitemap: false
keywords: ""
description: ""
---

March is officially here.

I'm looking forward the season of Spring.

I miss the green landscape all over the city.

I purchased a new VPS for a better user experience.

And a few majority improvements are documented in the Github.

## Install ZSH

Install zsh, curl and git by running
```
sudo apt-get install zsh curl git
```
Follow the installation instructions on the GitHub page of Oh My Zsh. Currently it is running the following command:
```
sh -c "$(curl -fsSL <https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh>)"
```
During the installation, Zsh is set to be your default login shell.
Thus, manually running `chsh -s /usr/bin/zsh` is not necessary.

Logout and login. Done.

Install ZSH plugins: zsh-autosuggestions, zsh-syntax-highlighting, zsh-completions
```
git clone <https://github.com/zsh-users/zsh-autosuggestions> ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

git clone <https://github.com/zsh-users/zsh-completions> ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions

git clone <https://github.com/zsh-users/zsh-syntax-highlighting.git>
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```
Add the plugin to the list of plugins for Oh My Zsh to load (inside ~/.zshrc):
```
plugins=(zsh-autosuggestions
plugins=(… zsh-completions
autoload -U compinit && compini
```
Enable highlighting
```
source ./zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```
## Solution for `chsh: PAM authentication failed solution` errors.
```
sudo vim /etc/pam.d/chsh
```
then, comment auth required pam_shells.so
```
sudo chsh $USER -s $(which zsh)
```
Revert bash for root user
```
sudo chsh -s /bin/bash root
```

## If you assign a user to /etc/sudoers

Try to login root with PWD
```
su
```
Input root PWD and login as root
```
sudo chown root:root /etc/sudoers/
sudo chmod 440 /etc/sudoers/
sudo chown root:root /etc/sudoers.d
sudo chmod 755 /etc/sudoers.d
```
This could help to assign the sudoers to root.

## Sync files between local machine and the remote server.
```
scp -r -P 12345 /local_folder_path username@123.123.123.123:/remote_folder_path

scp -r username@123.123.123.123:~/remote_folder_path /local_folder_path
```

## nmap the ports
```
nmap 123.123.123.123
```

## SSH without 22 port
```
ssh -i ~/.ssh/my_rsa -p 12345 username@123.123.123.123
```
<!--more-->
