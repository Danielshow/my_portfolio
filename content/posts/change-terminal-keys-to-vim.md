+++
title = "Change Terminal Keys to VIM"
date = "2020-01-22"
author = "Daniel Shotonwa"
authorTwitter = "dshow_World" #do not include @
cover = ""
tags = ["Vim", "Terminal", "Productivity"]
keywords = ["Vim", "Terminal", "Productivity"]
description = "So I have been struggling with my terminal lately, having done a lot of tweaks to improve my productivity, like using Tmux as a multiplexer, setting an alias for reoccurring commands and the likes."
showFullContent = false
+++

> So I have been struggling with my terminal lately, having done a lot of tweaks to improve my productivity, like using Tmux as a multiplexer, setting an alias for reoccurring commands and the likes.

I find it hard to use my terminal because to edit a command, you have to scroll with your arrow keys or press some weird key bindings. It has been frustrating so far.

I thought of using vim keys but don't know if it exists, just a little search online and I saw the command I needed.

```bash
$ set -o vi
```

This will set your terminal to use Vim key bindings, the default mode is insert mode so you can type anything you want. To go to command mode, just click `escape key` and you are good.

#### Important VIM keys I use while in command mode
- 0 or ^   - to move to the first character
- $   - to move to the last character
- f and a letter   - search for a letter through the current line
- F and a letter   - search for a letter backwards


This setting will not persist, it will only be available for the life cycle of the terminal. To persists it, you have to add `set -o vi` to your `.bashrc` or `.zshrc` or other profiles.

```bash
$ vim ~/.bashrc
```

After that, close your terminal and open it. Enjoy your super cool terminal.