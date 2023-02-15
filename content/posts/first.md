---
title: "The Dotfile Series"
date: 2023-02-10T11:12:01+11:00
draft: false
---
# Dotfiles Series pt. 1
Within this series I'm going to take you through creating a dotfiles directory and populating it with
many different dotfiles.

Topics covered:
- [Creating our Dotfiles Directory ](#creating-our-dotfiles-directory)
- [Installing Z shell](#installing-z-shell)
- [Configuring our .zshrc](#configuring-our-.zshrc)
- [Symlinking and How It Works](#symlinking-and-how-it-works)

### First What is a Dotfile?
A dotfile is a configuration file that can customize various aspects of programs that we use on Unix-like operating systems. They are given the name dotfiles due to the "." at the start of the filename (e.g. .zshrc, .tmux.conf). 

### Creating our Dotfiles Directory
This is the easy part creating the directory, this is where we will maintain all of our dotfiles. Using symlinking we will be able to place the dotfiles into their correct places while also having them within our dotfiles directory. This gives us the ability to create a git repository to store all of our changes, which means you can replicate your configuration at any time all you need is Git. Within your home directory create `.dotfiles`.

> `mkdir .dotfiles`

Now we have our .dotfiles directory it's time to get some dotfiles.

### Installing Z shell
We are going to start with a shell, our shell is just a way for us to interact with our operating system through the CLI _(Command Line Interface)_. I personally like Z shell or _(zsh)_.

I am on Ubuntu, so I will run the following command to install zsh. _For other installations have a look at this link  [other zsh installations](https://gist.github.com/derhuerst/12a1558a4b408b3b2b6e)_

> `sudo apt-install zsh`

Once the install run `zsh` in your terminal

 > `zsh`

You should be greeted with the following screen, select the option below we are going to populate it ourselves. Once


### Configuring our .zshrc

Let's start to configure our first dotfile, our .zshrc.

Open up your .zshrc with `vim .zshrc`

>```
># Enable colors.
>autoload -U colors && colors
>
> # History in cache directory.
> HISTSIZE=10000
>SAVEHIST=10000
>HISTFILE=~/.cache/zsh/history
>
># Set Defaults.
>export EDITOR=vim
>export VISUAL=vim
>```
