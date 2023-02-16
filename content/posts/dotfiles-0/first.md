---
title: "The Dotfile Series Chapter 0"
date: 2023-02-16T11:12:01+11:00
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
A dotfile is a configuration file that can customize various aspects of programs that we use on Unix-like operating systems. They are given the name dotfiles due to the "." at the start of the filename (e.g. .zshrc, .tmux.conf). To see these files we need to look for hidden files within your terminal run

>```bash
>ls -la
>```

![img1](/images/dotfiles-0/img1.jpg)
<small>Example of list directory contents, with long list and don't ignore hidden files.</small>


### Creating our Dotfiles Directory
This is the easy part creating the directory, this is where we will maintain all of our dotfiles. Using symlinking we will be able to place the dotfiles into their correct places while also having them within our dotfiles directory. This gives us the ability to create a git repository to store all of our changes, which means you can replicate your configuration at any time all you need is Git. Within your home directory create `.dotfiles`.


> ```bash
> mkdir .dotfiles
> ```

![img2](/images/dotfiles-0/img2.jpg)
<small>Example of creating our .dotfiles and showing it in our home directory</small>

Now we have our .dotfiles directory it's time to get some dotfiles.

Before we move onto installing `zsh` if you don't have nerd fonts on your operating system I would recommend getting them. They allow you to give your shell a bit more customisation through symbols if you want to learn more check out this small [blog post](https://www.fiqlab.dev/blog/nerdfonts)  before proceeding.

In Ubuntu open up a new `.sh` file in your editor of choice create this script.
>```bash
>#!/bin/bash
>
>declare -a fonts=(
>    BitstreamVeraSansMono
>    CodeNewRoman
>    DroidSansMono
>    FiraCode
>    FiraMono
>    Go-Mono
>    Hack
>    Hermit
>    JetBrainsMono
>    Meslo
>    Noto
>    Overpass
>    ProggyClean
>    RobotoMono
>    SourceCodePro
>    SpaceMono
>    Ubuntu
>    UbuntuMono
>)
>
>version='2.1.0'
>fonts_dir="${HOME}/.local/share/fonts"
>
>if [[ ! -d "$fonts_dir" ]]; then
>    mkdir -p "$fonts_dir"
>fi
>
>for font in "${fonts[@]}"; do
>    zip_file="${font}.zip"
>    download_url="https://github.com/ryanoasis/nerd->fonts/releases/download/v${version}/${zip_file}"
>    echo "Downloading $download_url"
>    wget "$download_url"
>    unzip "$zip_file" -d "$fonts_dir"
>    rm "$zip_file"
>done
>
>find "$fonts_dir" -name '*Windows Compatible*' -delete
>
>fc-cache -fv
>
>```
<small>Post of the script is linked [here](https://gist.github.com/matthewjberger/7dd7e079f282f8138a9dc3b045ebefa0?permalink_comment_id=4005789#gistcomment-4005789)</small>

Run that script with `bash script_name.sh` to install a bunch of nerd fonts. Once you've run that script and they've installed let's move on.


### Installing Z shell

We are going to start with a shell, our shell is just a way for us to interact with our operating system through the CLI _(Command Line Interface)_. I personally like Z shell or _(zsh)_.

I am on Ubuntu, so I will run the following command to install zsh. _For other installations have a look at this link  [other zsh installations](https://gist.github.com/derhuerst/12a1558a4b408b3b2b6e)_

> ```bash
> sudo apt-install zsh
> ```

Once the install has finished run `zsh` in your terminal

 > ```bash
 > zsh
 > ```

![img3](/images/dotfiles-0/img3.jpg)
<small>You should come to this screen hit 0 and create your `.zshrc` we will be populating it.</small>

You'll notice that your prompt has change to your machine name and should look like this `machine_name%` 

![img4](/images/dotfiles-0/img4.jpg)
<small>Example indicates that we are in zsh.</small>

Now we are in zsh let's make zsh our default shell from bash with the following commands
>```bash
># Check current default shell.
>echo $SHELL
>
># Find location of zsh file
>which zsh
> 
> 
>chsh -s #location of zsh here e.g. /usr/bin/zsh
> 
> # If it returns a path to /zsh you've done it correctly
> grep <user_name_here> | /etc/passwd 
>```
Red: We check to see our current `$SHELL` default.

Green: First we locate our zsh with which. Then we change the default shell with `chsh -s <shell_path_here>`

Yellow: is a check to see if our change was effective.

![img5](/images/dotfiles-0/img5.jpg)

If it's returning the zsh in the yellow path let's restart our computer now and then proceed to configuring our `.zshrc`!



### Configuring our .zshrc

Now we have all that let's start to configure our first dotfile, our `.zshrc`!

Open up your `.zshrc` with `vim .zshrc`

Add these lines to your `.zshrc`.  

>```bash
># Keymap for quick source.
>alias szsh='source .zshrc'
>
># Prompt Settings.
># http://zsh.sourceforge.net/Doc/Release/Prompt->Expansion.html -- great resource. 
>PROMPT='%F{yellow}[%F{white}%n%F{yellow}]%F{green}[%~]%f %f'
>RPROMPT='%F{yellow}%W %* %(?.√.%?)'
>```

![img7](/images/dotfiles-0/img7.jpg)

Our first line is an alias; you can think of aliases as a shorter version of lengthy commands. Some individuals source a second file to their shell config because their dotfiles include so many aliases. In another piece, I'll go into greater detail about them. By using `szsh` our `source .zshrc` alias will assist us in reloading our `.zshrc` configuration. We can source our changes more quickly as a result of this.

Your terminal prompt will be affected by the next line in your `Prompt Settings.` This can be customised in a wide variety of ways. I provided a link with the starting point; if you copy it, you'll receive a nice one that I created myself. ' [%~]%f %f ' ' If you installed the nerd fonts, this "?" box is one of them.

Our first line is an alias, aliases can be thought as an abbreviation of a longer command. Some people have so many aliases within their dotfiles they create a separate file and source it to their shell. I will cover them more in another article. Our `source .zshrc` alias will help us reload our `.zshrc` config by calling `szsh`. This makes it quicker for us to source our changes.

The next line for your `Prompt Settings.` will effect your terminal prompt. There are many different customisations you can do to this. I left a link with the resource to get started, copying mine will give you a nice one I designed myself. ' [%~]%f %f' ' The '?' box here is apart of nerd fonts, if you installed your nerd fonts correctly you will see a arrow.

![img8](/images/dotfiles-0/img8.jpg)
<small>This is what your terminal should now look like.</small>


Now let's add some other basics, these are some of the defaults that I use. 
>```bash
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

A small amount of aliases to get you going the ones that stick out here are the `Easier directory navigation` having these is great for traversing.

>```bash
># Open editor.
>alias e='$EDITOR'
>
># Easier directory navigation.
>alias -g ...='../..'
>alias -g ...='../../..'
>alias -g ....='../../../..'
>
># Github shortcuts.
>alias ga='git add'
>alias gaa='git add .'
>alias gcmsg='git commit -m'
>alias ggpull='git checkout master && git pull && git status'
>alias ggpush='git push origin'
>alias gre='git restore --staged'
>alias gsat='git status'
>```


The following group of code is a little more complicated, but they are still very useful. 

The first is to help with tab completion; as part of this, dotfiles will also be included in the suggested now.

A directory will be created from the variable supplied to `md` with a filename, and it will then immediately change into that directory.

![gif](/images/dotfiles-0/mdgif.gif)

`Turn on vi mode`This isn't for everyone, but I'm a vim user, so I appreciate the utility of it in my terminal. I've left it here for you to decide. I've left the link where I got this from within the code if you'd like to understand it better.

![gif](/images/dotfiles-0/vimodegif.gif)

>```bash
># Basic auto/tab complete.
>autoload -U compinit
>zstyle ':completion:*' menu select
>zmodload zsh/complist
>compinit
>_comp_options+=(globdots) # (globdots) will include hidden files.
>
># Make directory and change into it.
>md () {
>   mkdir "$1"
>   cd "$1"
>}
>
># Enable vi mode and set key timeout to 1 second.
>bindkey -v
>export KEYTIMEOUT=1
>
># Use vim keys in tab complete menu.
>bindkey -M menuselect 'h' vi-backward-char
>bindkey -M menuselect 'j' vi-down-line-or-history
>bindkey -M menuselect 'k' vi-up-line-or-history
>bindkey -M menuselect 'l' vi-forward-char
>
># Use vi-style delete key.
>bindkey -v '^?' backward-delete-char
>
># Vi code from:
># https://www.youtube.com/watch?v=eLEo4OQ-cuQ
># https://gist.github.com/LukeSmithxyz/e62f26e55ea8b0ed41a65912fbebbe52
>
># Change cursor shape for different vi modes.
>function zle-keymap-select {
>  if [[ $1 = 'block' ]] || [[ ${KEYMAP} == vicmd ]]; then
>     echo -ne '\e[1 q' # block shape cursor for command mode
>  else
>     echo -ne '\e[5 q' # beam shape cursor for insert mode
>  fi
>}
>zle -N zle-keymap-select
>
># Use beam shape cursor by default and after each command.
>function zle-line-init {
>  zle -K viins # initiate `vi insert` as keymap (can be removed if `bindkey -V` has been set elsewhere)
 >  echo -ne "\e[5 q"
>}
>zle -N zle-line-init
>echo -ne '\e[5 q' # beam shape cursor on startup.
>preexec() { echo -ne '\e[5 q'; } # beam shape cursor after each command.
>```

We are going to install a plugin to our zsh, zsh autosuggestions is a helpful plugin and many use it. Run the command below and it will create a directory for our zsh plugin.

>```bash
>git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
>```

Once we have ran that we can now source file within in our .zshrc 
>```bash
># Autosuggestion plugin.
>source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh
>```

### Symlinking and How It Works

Now let's place our dotfiles in our `.dotfiles` directory!

![img9](/images/dotfiles-0/img9.jpg)

Once you've done that they are now located within your `.dotfiles`directory meaning they are no longer within the home directory. We need them within the home directory as it expects them to be there. We can achieve this easily through symlinking. 

>```bash
>ln -s ~/.dotfiles/.zshrc ~/.zshrc
>ln -s ~/.dotfiles/.zsh ~/.zsh
>```

![img10](/images/dotfiles-0/img10.jpg)

These will create a reference to our files or directories from one location to another without copying our files. A symlink is a pointer to the file or directory declared in the symlink. These symlinks allow us to have our files within multiple locations!
