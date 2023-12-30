## Table of Contents

<!-- vim-markdown-toc GFM -->

* [Update repository sources](#update-repository-sources)
* [Setup firewall](#setup-firewall)
* [OPI (OBS Package Installer)](#opi-obs-package-installer)
* [Yubikey setup](#yubikey-setup)
    * [Installation](#installation)
    * [Fix access issue](#fix-access-issue)

<!-- vim-markdown-toc -->

## Update repository sources

Ref: https://mirrors.ustc.edu.cn/help/opensuse.html

```bash
# disable current sources
sudo zypper mr -da
# update repositories
sudo zypper ar -fcg 'https://mirrors.ustc.edu.cn/opensuse/tumbleweed/repo/oss' USTC:OSS
sudo zypper ar -fcg 'https://mirrors.ustc.edu.cn/opensuse/tumbleweed/repo/non-oss' USTC:NON-OSS
sudo zypper ar -fcg 'https://mirrors.ustc.edu.cn/opensuse/update/tumbleweed' USTC:UPDATE
# refresh
sudo zypper ref
# upgrade
sudo zypper dup
```

## Setup firewall

Ref: https://www.linode.com/docs/guides/introduction-to-firewalld-on-centos/

```bash
# inspect current zone and rules
sudo firewall-cmd --get-default-zone
sudo firewall-cmd --zone=public --list-all
# add custom firewall rules
sudo firewall-cmd --zone=public --add-port=5598/tcp --permanent
# reload
sudo firewall-cmd reload
# remove custom firewall rules
sudo firewall-cmd --zone=public --remove-port=12345/tcp --permanent
# enable firewalld at boot
sudo systemctl enable firewalld
```

## OPI (OBS Package Installer)

```sudo
sudo zypper install opi
```

## Yubikey setup

Reference: <https://github.com/techprober/yubikey-reference>

Import keys: <https://github.com/techprober/yubikey-reference/blob/master/docs/import-from-yubikey.md>

### Installation

```bash
# install depedencies
sudo zypper install pcsc-lite pcsc-ccid perl-pcsc pcsc-tools opensc usbutils gnupg pinentry libusb-compat-devel pam_u2f
# install GUI client
sudo zypper install yubikey-manager
# enable pcscd at boot
sudo systemctl enable pcscd --now
# check key status
sudo ykman info
sudo opensc-tool --list-readers
# install age-plugin-yubikey
nix-env -iA nixpkgs.age-plugin-yubikey
```

### Fix access issue

Access card reader as non-root user

Ref: https://blog.apdu.fr/posts/2023/11/pcsc-lite-and-polkit/

```bash
# /etc/sysconfig/pcscd
PCSCD_ARGS=--disable-polkit
# restart pcscd
sudo systemctl restart pcscd
# check key status
ykman info
pcsc_scan -r
gpg --card-status
```
