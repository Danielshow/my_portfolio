+++
title = "Setting up vim, tmux, iterm and oh-my-zsh."
date = "2019-06-18"
author = "Daniel Shotonwa"
authorTwitter = "dshow_World" #do not include @
cover = ""
tags = ["VIM", "TMUX", "ITERM"]
keywords = ["VIM", "TMUX", "ITERM", "Better workflow"]
description = "A lot of people always asked how I pimped my terminal, some even call me a weirdo because I was using VIM and they were using VSCode."
showFullContent = false
+++

A lot of people always asked how I pimped my terminal, some even call me a weirdo because I was using VIM and they were using VSCode.


> VScode is cool, you use Git like a Lord but do you know how Git works, can you write commands? Some guys have even forgotten how to use Git because VScode has made everything easy. I am not a fan of easy things. I live on the Terminal. 😂

![](https://res.cloudinary.com/practicaldev/image/fetch/s--EjMDEumE--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/2600/1%2AJkrehnEox6EhKCmYjEriFA.png)

I will explain all the libraries I installed and what I used them for. I am a macOS user, so I used brew to install my packages.

#### Tools Used:

- iTerm
- Vim
- Tmux
- Oh-my-Zsh

#### iTerm

iTerm is a replacement for macOS default Terminal. With iTerm, you can split your terminal into different sizes, search for text and use copy & paste. You can also use custom font and themes with iTerm. To install iTerm on macOS:

```bash
$ brew cask install iterm2
```


Now kindly ditch your default terminal and start using iTerm. I’ve not used my default terminal for some years now.

#### VIM
Vim does not need any explanation but it's worth trying. VIM makes me productive and with vim, I can use different plugins to enable me to work faster.

Vim is preinstalled on MacOs but if you don’t have it, you can install it by running this code in your terminal. This command will install VIM and override system vim and path.

```bash
$ brew install vim --with-override-system-vi
```


To learn the basics of vim, I would recommend, mostly the best way is to first learn the basic commands using vimtutor and then move on to add plugins for any pain points that you discover as you go. For instance, if you discover that you will need a filetreejust like VSCode provides, then by searching for this, you’ll find out about NERDTREE.


#### TMUX
Tmux is a terminal multiplexer: it enables a number of terminals to be created, accessed, and controlled from a single screen. One big win of Tmux is that you can be detached from a screen and continue running in the background, then later reattached.

With TMUX, you could share your terminal into different chunks, running server, vim, redis-server etc. To install Tmux on iTerm, type:
```bash
$ brew install tmux
```



#### Oh-my-Zsh
Oh-my-zsh is an open source plugin, it enables you to add themes, fonts, and customize your terminal. it has a lot of plugins to make you 10x productive as a developer. Install it using the following:

```bash
$ brew install zsh
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

One of the plugins I use with oh-my-zsh is autosuggestion. It enables autocompletion for your terminal.

...

To make your terminal beautiful as mine 😅, I will share my box-settings and explain how you can configure your terminal. Before you use my settings, make sure you have installed Tmux, oh-my-zsh and Vim.

Using my settings, you can split your screen, writing code on one part and your terminal on the other. Developers might even call you a hacker because you will most likely like on the terminal.



You can fork my Box settings repository here.

STEP 1:
I use Vundle as my Plugin manager. Install Vundle:

```bash
$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

STEP 2:
Clone my Box settings and copy the files to your root path.

```bash
$ git clone https://github.com/Danielshow/BoxSetting
$ cd BoxSetting
```

STEP 3:
Copy all the files to your root path

To copy the files:

```bash
$ cp tmux.conf ~/.tmux.conf
$ cp vimrc ~/.vimrc
$ cp zshrc ~/.zshrc
```


Update:
I changed my terminal recently, I added spaceship-prompt. 

```bash
$ npm install -g spaceship-prompt
$ git clone https://github.com/denysdovhan/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt"
$ ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"
```
And that's all.

After copying all the files, open vim and type in : to enter a command. Type in PluginInstall to install all the plugins.

You might face a little error from you-complete-me, do this to fix it.

```bash
$ brew install cmake macvim
$ cd ~/.vim/bundle/YouCompleteMe
$ ./install.py
```

Run PluginInstall again and all should be fine.

Enjoy your super terminal 🎉.
If you face any error, you can reach out to me via twitter, I'll be willing to help.

#### [Buy me a Coffee](https://www.buymeacoffee.com/danielshow)
