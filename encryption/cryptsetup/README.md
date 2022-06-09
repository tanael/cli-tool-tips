# cryptsetup

## Creating an encrypted block device on a USB stick

### erase everything from the chosen block device

```bash
wipefs -a /dev/sdw1
```

OR

```bash
cat /dev/zero > /dev/sdw1
```

### OR use a loop block device
```bash
dd if=/dev/zero of=/tmp/loopdev bs=1k count=102400
losetup -f
losetup /dev/loop0 /tmp/loopdev
```

### setup LUKS container on device

Assuming we continue using the loop device.

```bash
cryptsetup luksFormat /dev/loop0
```

### use the mapped device

```bash
cryptsetup luksOpen /dev/loop0 secret
```

This provides `/dev/mapper/secret`

### create a file system

```bash
mkfs.ext4 /dev/mapper/secret
```

### mount it

```bash
mkdir -p /mnt/secret
mount /dev/mapper/secret /mnt/secret
```

### information about the device

```bash
cryptsetup luksDump /dev/loop0
```

### unmount

```bash
umount /mnt/secret
cryptsetup luksClose /dev/mapper/secret
```
