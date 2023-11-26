## Yubikey setup

Reference: https://github.com/techprober/yubikey-reference

### Installation

```bash
# install depedencies
sudo zypper install opensc usbutils libpcsclite1 pcsc-ccid gnupg pinentry libusb-compat-devel pam_u2f
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

```bash
lsusb

#outputs
Bus 002 Device 002: ID <vendor_id>:<product_id> Yubico.com Yubikey 4/5 OTP+U2F+CCID

# add rules
# /etc/udev/rules.d/70-u2f.rules
SUBSYSTEMS=="usb", ATTRS{idVendor}=="<vendor_id>", ATTRS{idProduct}=="<product_id>", TAG+="uaccess", TAG+="udev-acl", OWNER="<username>"

# reload rules
sudo udevadm control --reload-rules
sudo udevadm trigger
# restart scdaemon
sudo systemctl restart pcscd

# check key status
ykman info
gpg --card-status
```
