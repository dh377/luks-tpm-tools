# LUKS TPM tools (luks-tpm-tools)

Make your _backup key_ on removeable device (USB/MMC) , generate **STRONG** (BitLocker like 48-digit) `recovery` key, `seal` your key on TPM device and enjoy automatic unlocking of your Full Disk Encryption.

This tool is compatible with TPM version **1.2** and **2**.

## Reqirements
1. Ubuntu dist.
1. [`tpm-tools` package](apt:tpm2-tools) or [`tpm2-tools` package](apt:tpm2-tools) (The package should be installed exclusively.)
2. TPM v1.2/v2.0 device/emulator

## Installation
```sh
chmod +x ./install
sudo ./install
```

## Tools

### `key_backup`

This command generates an auto unlock USB key. You don't need to enter long passphrase when you unlock your system (if TPM PCR failed) or run `key_seal`.

### `key_recovery`

Generate strong recovery key. It looks similar with MS BitLocker's 48-digit (20 bytes) recovery password. If your LUKS passphrase length is shorter than 16, it is highly recommended to run at least once. Printed out and save it at physically safe place.

### `key_seal`

Entering Passphrase options when newly generate and seal the keyfile:
1. (Automatic) use backup USB;
2. (Manual) use recovery passphrase.

Keyfile will save at the NVRAM area in your TPM device (default).

However, if you set `NVRAM=""` as a default parameter in `/etc/default/luks-tpm-tools`, `key_seal` trying to use `/boot` partition to save keyfile as an encrypted form, instead of use NVRAM.

NVRAM (Non-Volatile RAM) semiconductors may damage the device if too many writes are performed, but there is little room for a big problem in LUKS key operation, which is mainly read after writing once.

NVRAM has a more security advantage of protecting the key by setting the READ_STCLEEAR flag to refuse to read again after being read once at boot time.
