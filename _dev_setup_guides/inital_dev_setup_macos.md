---
layout: resource
title: Zsh, Homebrew, and Sublime Text - MacOS Dev Setup
slug: intial_dev_setup_macos
order_number: 1
---
## The Very Basics
This guide is meant to walk through the most basic components of a dev environment setup on MacOs Catalina - the steps you would perform before writing your first lines of code on a new or factory-reset machine.

For my purposes and preferences, these basic components are:

1. Configure zsh 
2. Set terminal app preferences
3. Install Homebrew package manager
4. Install & configure Sublime text 3

We will leave configuration for specific programming languages and toolchains for subsequent guides

## 1. Configure Zsh

The following assumes you are using the MacOS built-in Terminal.app

### Setting up Terminal.app to use zsh
1. Open Terminal.app
2. Open the Preferences pane using `Cmd +  ,` or by navigating to Terminal > Preferences in the menu bar
3. In the General tab, note that "Default login shell" is selected. On MacOS Catalina, the default shell has been updated to `/bin/zsh`. On previous MacOS versions, you can enter `/bin/zsh` in the "Command (complete path)" option to switch from the default (`/bin/bash`) to `zsh`
4. Use the Profiles tab to set any font, color, and cursor preferences you may have.


### Your first entry into `~/.zshrc`

Your zsh configuration will largely be managed in `.zshrc` file in your home directory. This file is executed as a shell script whenever you initiate a new zsh session.

The most basic use of your `.zshrc` and similar dotfiles is to set environment variables, but in time you will use it to initialize common tools and dependencies as well as execute any shell scripts or commands you find to be useful. 

To start, we are only going to have our `.zshrc` to do one thing: `echo $PATH`. As the system `PATH` is so crucial to command-line tooling, I prefer to be start every terminal session with a printout of the current `PATH`.

**Option 1: Command Line**
```
% echo "echo \$PATH" > ~/.zshrc
```

Note that the the `$` character needs to be escaped with a backslash so the actual string `echo $PATH` ends up in your dotfile, rather than the string `echo [whatever your PATH was when you ran this command]` which would not be particularly helpful.

**Option 2: A text editor**

Assuming you do not already have your preferred text editor installed, you can use the TextEdit app that ships with MacOS as the default editor for text files. The `open` command will attempt to open a file with whichever app is the default for that file's type.
```
% touch ~/.zshrc
% open ~/.zshrc
```

Add the line `echo $PATH` to the file and save.

**Test it out**

The `.zshrc` will be invoked when you open a new terminal session in a new Terminal tab (`Cmd + T`), a new window (`Cmd + N`), or by running the file using the `source` command.

You should see the contents of your `PATH` printed out. Compare the output to directly invoking `echo $PATH` from your terminal.
```
% source ~/.zshrc
/usr/bin:/bin:/usr/sbin:/sbin:/Library/Apple/usr/bin:/Library/Apple/bin
% [carry on doing your thing]
```
