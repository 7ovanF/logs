# Kernel Versions?

You can choose between different kernel versions in grub. Actually, kernel upgrades are usually done side-by-side (no actual uninstalls, just installing next to the old one).

The "holy trinity" of kernel components
- `/boot/vmlinuz-<version>` the kernel binary
- `/boot/initramfs-<version>.img` initramfs. The early userspace
- `/lib/modules/<version>/*` ALL loadable modules

Relevant installable packages:
- `kernel-image-<version>` for apt, `kernel-core-<version>`for Fedora  (core kernel binary)
- `kernel-modules-<version>` (contains modules such as drivers)
- `kernel-modules-extra-<version>` (optional)
Usually, versions are installed side-by-side.

Worth trying to find out: `depmod` (builds module dependency graphs), `modprobe` (resolves, loads drivers)