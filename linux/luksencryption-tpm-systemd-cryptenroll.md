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
- if volume is the root partition, initramfs needs tweaking. 
  - can be mkinitcpio (arch) or dracut (most others)
  - in my case i use dracut.

1. See list
```
sudo cryptsetup luksDump /dev/(partitionName)
```

Or for general "what tokens are enrolled?":
```
sudo systemd-cryptenroll /dev/(partitionName)
```

2. Enroll TPM2 
```
sudo systemd-cryptenroll --tpm2-device=auto --tpm2-pcrs=(PCRS tokens. ex: 0,2) /dev/(partitionName)
```

3. Unenroll
```
sudo systemd-cryptenroll --wipe-slot=(tokenName. ex: tpm2) /dev/(partitionName)
```

| PCR | Domain            | Stability     | Typical use    |
| --- | ----------------- | ------------- | -------------- |
| 0   | Firmware code     | Stable        | Common         |
| 1   | Firmware config   | Unstable      | Avoid          |
| 2   | Option ROMs       | Stable        | Common         |
| 3   | Vendor-specific   | Unpredictable | Avoid          |
| 4   | Bootloader        | Moderate      | Optional       |
| 5   | Kernel/initramfs  | Unstable      | Avoid          |
| 6   | State transitions | Rare          | Avoid          |
| 7   | Secure Boot       | Moderate      | Conditional    |
