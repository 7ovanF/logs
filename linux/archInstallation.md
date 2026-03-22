# Arch Installation Steps

If you're gonna encrypt it, do it early (after partitioning; format AFTER encrypting) (though you can do it later)
## Set up disk
0. Set-up network (ping, if unsuccessful use iwd (iwctl) to setup wifi)
1. Partitions: use gdisk (gpt fdisk) or fdisk, then format them.
  Dont forget to add filesystem codes ("EFI Boot Partition", etc)
2. Mount main fs to /mnt, EFI Partition (if there is) to /mnt/boot/efi, swapon for SWAP
3. pacstrap (-K? whats pacman keyring?) essentials: 
  - base <- definitely
  - other stuff... go look at arch wiki
4. Generate fstab to /etc/fstab
5. chroot (arch-chroot) to /mnt

## Into the Fire
6. Stuff... timezone (), localization, keymap
7. passwd root
8. hostname, make user (group: wheel, make home dir, add to sudoers file)
9. oh right, sudo isnt even installed
10. mkinitcpio -P to generate initramfs image
11. grub-install --target=x84\_64-efi(should be) --efi-directory=/boot/efi --bootloader-id=GRUB
