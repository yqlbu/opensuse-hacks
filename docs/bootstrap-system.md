# Bootstrap System

OpenSUSE Tumbleweed Hyprland Setup

## Table of Conents

<!-- vim-markdown-toc GFM -->

* [Set consolefonts](#set-consolefonts)
* [Configure system locales](#configure-system-locales)
* [Configure zramd](#configure-zramd)
* [Set default shell](#set-default-shell)
* [Configure Nix](#configure-nix)
* [Install firmware packages](#install-firmware-packages)
    * [System related](#system-related)
* [Network related](#network-related)
    * [Audio related](#audio-related)
    * [Bluetooth related](#bluetooth-related)

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

## Configure system locales

Reference: https://techviewleo.com/configure-system-locales-on-opensuse-sles/

```bash
sudo yast2
```

System > Language > Secondray Language

## Configure zramd

Reference: https://old.reddit.com/r/openSUSE/comments/mfs53x/tip_enable_zram_easily/

```bash
sudo zypper in systemd-zram-service
systemctl enable --now zramswap.service
lsblk

# modify configuration if needed
# /etc/default/zramd
```

## Set default shell

```bash
sudo zypper in fish
sudo chsh -s /usr/bin/fish $USER
```

## Configure Nix

Reference: https://christitus.com/nix-package-manager/

Install

```bash
bash -c "sh <(curl -L https://nixos.org/nix/install) --daemon"
```

Set max jobs

```bash
# /etc/nix/nix.conf
max-jobs = auto
```

Update channel

```bash
nix-channel --add https://nixos.org/channels/nixpkgs-unstable
nix-channel --update
```

## Install firmware packages

### System related

```bash
sudo zypper in xdg-utils xdg-user-dirs
nix-env -iA nixpkgs.tldr nixpkgs.trash-cli
```

## Network related

```bash
sudo zypper in iptables-nft
```

### Audio related

Reference:

- https://wiki.archlinux.org/title/PipeWire
- https://github.com/mikeroyal/PipeWire-Guide

```bash
sudo pacman -S pipewire pipewire-alsa pipewire-pulseaudio pipewire-jack wireplumber pavucontrol pamixer alsa-utils
systemctl --user enable --now pipewire-pulse.socket
systemctl --user enable --now pipewire.socket
systemctl --user enable --now wireplumber.service
```

Load DSP drivers for ThinkPad X1 Carbon

```bash
# /etc/modprobe.d/dsp-fix.conf
options snd_intel_dspcfg dsp_driver=1
```

### Bluetooth related

```bash
sudo zypper in --no-recommends bluez blueman
```
