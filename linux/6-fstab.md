# Wtf is an /etc/fstab?

File that defines mount points for partitions. After the root filesytem is mounted, systemd uses fstab's specifications to mount the other parts, such as /boot/efi.

`genfstab` in the `arch-install-scripts` package can generate it for you. Define what the root should be, and it will generate the fstab configuration based on how you've mounted the filesystems manually (with `mount` and such).

The root filesystem is mounted by initramfs (on what basis? how does it find the root) without paying mind to fstab (well, fstab is located INSIDE that root, so it's a chicken-and-egg problem). However, you CAN define it for remounting, which is separate and done after initramfs.

#### fstab identifiers (not that important but eh)
- UUID: the most stable, defines each partition very well
- Label: human-readable, not very absolute
- Device paths: legacy, discouraged. Something like /dev/sda can turn into /dev/sdb one day.