# Volume Encryption and shit

### Enrolling hardware security tokens/devices into an encrypted volume: \
Enrollment here means adding an unlock method into the "trusted methods" list

#### How exactly it works:
- generate/uses a key associated to the hardware token
- copies the actual volume key, encrypts it with the token, then stores in a new keyslot in the volume's header (metadata)
- volume key = DEK (Device Encryption Key), key for key = KEK (Key Encryption Key)
- when conditions are fulfilled, decrypts that key and uses it to decrypt volume

> it's not "encrypting the volume with THE TOKEN"

I will try using this to auto decrypt my fedora (luks encrypted), with systemd-cryptenroll.

--- 

## Enrollment
[Archwiki: systemd-cryptenroll](https://wiki.archlinux.org/title/Systemd-cryptenroll)

`systemd-cryptenroll /dev/[volume]` to see enrolled tokens.

Types of keyslots:
- empty
- passphrases
  - password (user-set)
  - recovery (computer-generated: such as the one bitlocker needs)
- hardware devices
  - pkcs11
  - fido2 (ignoring these two for now)
  - tpm2 (special device; what i want)

### TPM2 Enrollment

Notes:
- needs tpm2-tss package
- if volume is the root partition, initramfs needs tweaking kkkkkkkkkkkkkkkk
