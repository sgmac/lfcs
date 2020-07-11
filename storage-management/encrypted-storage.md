# Encrypted storage

- Verify `grep -Ei config_dm_crypt /boot/config-$(uname -)`
- apt install crypsetup
- `cryptsetup -y luksFormat /dev/sda1` (Set password)
- `cryptsetup luksOpen /dev/sda1 luks-vol1` (Enter the password)
- `blks` shows the partition as encrypted
- Make the filesystem
- `cryptsetup luksClose luks-vol1` (Enter the password)


