## Yubikey setup

Reference: https://github.com/techprober/yubikey-reference

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
gpg --card-status
```
