# My Arch Install
### Personal simple guide for installing Arch Linux

## Initial Setup
```
$ ls /sys/firmware/efi/efivars
$ ip link
$ ping archlinux.org
$ timedatectl set-ntp true
$ timedatectl status
```

## Partitioning
```
$ fdisk -l
$ fdisk /dev/*disk*
```
Set partitions according to the following:
### For UEFI

|Mount point|Partition|Partition type|Suggested Size|
|-----------|---------|--------------|--------------|
|/mnt/boot  |/dev/\*efi\*|EFI system partition|300MiB+
|[SWAP]     |/dev/\*swap\*|Linux swap|512 MiB+
|/mnt       |/dev/\*root\*|Linux x86-64 root (/)|Remainder

### For BIOS
|Mount point|Partition|Partition type|Suggested Size|
|-----------|---------|--------------|--------------|
|[SWAP]     |/dev/\*swap\*|Linux Swap|512 MiB+
|/mnt       |/dev\*root\*|Linux|Remainder

## Formatting
```
$ mkfs.ext4 /dev/*root*
$ mkswap /dev/*swap*
```
If you have created a EFI System Partition then use:
```
$ mkfs.fat -F 32 /dev/*efi*
```

## Mounting
```
$ mount /dev/*root* /mnt
$ swapon /dev/*swap*
```
For UEFI:
```
$ mount /dev/*efi* /mnt/boot
```

## Installing essential packages
```
$ pacstrap /mnt base linux linux-firmware
```

## Configuring the system
```
$ genfstab -U /mnt >> /mnt/etc/fstab
$ arch-chroot /mnt
$ ln -sf /usr/share/zoneinfo/*Region*/*City* /etc/localtime
$ hwclock --systohc
$ pacman -S *cli editor of your choice*
```
Edit ```/etc/locale.gen``` and uncomment ```en_US.UTF-8 UTF-8```
```
$ locale-gen
$ echo "LANG=en_US.UTF-8" > /etc/locale.conf
$ echo "*myhostname*" > /etc/hostname
$ passwd
$ useradd -m *username*
$ passwd *username*
```

## Installing other packages
```
$ pacman -S sudo networkmanager
$ systemctl enable NetworkManager.service
```

## Installing & Setting up GRUB
If you already have another distro with GRUB on it just update GRUB there. Otherwise:
```
$ pacman -S grub
$ grub-install --target=i386-pc /dev/*disk*
$ grub-mkconfig -o /boot/grub/grub.cfg
```

## Finishing Up
```
$ exit
$ reboot
```

