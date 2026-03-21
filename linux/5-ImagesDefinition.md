# Images?

A single **unified** binary representation of something — an executable, a filesystem representation, a memory image — structurally complete and appropriate for a certain loading model. Therefore, its arranged exactly in the format required for direct use, whether it be to execute directly, or to just load into RAM. (additionally "replicating"?)

Images do not guarantee self-sufficiency; some don't have the necessary dependencies. For example, AppImages and most container images are designed to be self-contained, therefore relies little on the host system. On the other hand, the kernel image relies on firmware, bootloader, drivers, and initramfs to function (or even exist on runtime).

Examples: kernel image, ELF binary, AppImage, ISO/disk image, container image (like docker's), initramfs.
> the kernel image can be found in /boot as vmlinuz-(version)

Origin: **memory imaging**. Early systems loaded programs by copying binary "images" into the RAM. These "images" are structured exactly as expected for a program loaded in RAM.