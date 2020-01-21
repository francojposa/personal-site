---
layout: resource
title: Python, Pyenv, and Virtualenvwrapper - MacOS Dev Setup
slug: golang_setup_macos
description: "The worry-free Python development environment setup for MacOS"
order_number: 3
---

## Our choice of tools: Pyenv & VirtualenvWrapper

Installing Python 3 & managing different versions and virtual environments on a Mac has been the subject of [many](https://docs.python-guide.org/starting/install3/osx/), [many](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-macos), online [guides](https://installpython3.com/mac/), and I have spent many, many, hours as a Python dev struggling to use these guides and the infinite combinations of Python management tools to create a local development environment that doesn't drive me insane.
Eventually, I found that the combination of tools that provide the control and simplicity I need:

 * Pyenv:  for installing & switching between different python versions
 * Virtualenvwrapper: for creating & managing virtual environments

Hopefully by writing up this guide I can save someone else the hours I have lost to the Python ecosystem!

## 1. Do Not Install Anything! (Yet)