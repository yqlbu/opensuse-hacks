# Bootstrap System

OpenSUSE Tumbleweed Hyprland Setup

## Table of Conents

<!-- vim-markdown-toc GFM -->

* [Set consolefonts](#set-consolefonts)

<!-- vim-markdown-toc -->

## Set consolefonts

```bash
# Install terminus fonts
sudo zypper in terminus-bitmap-fonts
# ls available console fonts
ls /usr/share/kbd/consolefonts/
setfont ter-v32n
# /etc/vconsole.conf
FONT=ter-v32n
```
