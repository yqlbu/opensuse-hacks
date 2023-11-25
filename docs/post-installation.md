## Yubikey setup

Reference: https://github.com/techprober/yubikey-reference

```bash
# install depedencies
sudo zypper install opensc usbutils libpcsclite1 pcsc-ccid gnupg pinentry libusb-compat-devel pam_u2f
# install GUI client
sudo zypper install yubikey-manager
# enable pcscd at boot
sudo systemctl enable pcscd --now
# check key status
sudo ykman info
```
