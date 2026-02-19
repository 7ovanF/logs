# Block Devices vs Character Devices

### Block Device: Data is stored and accessed per 'block'.
Block: a few bytes are considered as one block, rather than everything being measured in bytes
Per block is typically 512 bytes (traditionally) or 4 KiB (modern, advanced format).

#### why use blocks:
it just goes in line with how these storage devices were physically designed in the first place.
early disks (and HDDs, even today) physically cant read/write bytes arbitrarily; they read in sectors.
- in HDDs: entire sector of data is read in one rotation slice.
- in SSDs: data is spread by pages (please expand).
This is the root cause that the OS abstracts data I/O units into blocks.
Blocks:
- are either the same as the size of a single sector (the size physically read by the device) or bigger.
Key benefit: bigger, but less I/O operations, minimizing performance overhead.

This per-block system is generally used for persistent storage devices such as HDD, SSD, partitions, flash drives...

#### RAM
Byte-addressed: can read specific bytes.

### Character Device: Data comes in the form of streams
___


# UNIX vs GNU vs Linux?
- UNIX is a family of operating systems and, now, considered the standards that define the core design and interfaces of modern multiuser systems.
- GNU was created to build a free UNIX-compatible operating system and implemented most of its userland components, but lacked a usable kernel.
- Linux is a kernel that, when combined with GNU components, formed a complete UNIX-like operating system commonly called GNU/Linux.
- Linux distributions are operating systems built on the Linux kernel, usually with GNU userland, though some use alternative userlands instead.


# ++ Don't get tricked by ISP scams!!
kb = kilo BIT (1000 bits, decimal format)
kB = kilo BYTES (1000 bytes, generally 8000 bits, decimal format)
KiB = KIBIBYTES (1024 bytes, binary format)

comparison table
| Shown | Meaning  | Bytes               |
| ----- | -------- | ------------------- |
| 1 kb  | kilobit  | 1,000 bits          |
| 1 kB  | kilobyte | 1,000 bytes         |
| 1 KiB | kibibyte | 1,024 bytes         |
| 1 MB  | megabyte | 1,000,000 bytes     |
| 1 MiB | mebibyte | 1,048,576 bytes     |
| 1 GB  | gigabyte | 1,000,000,000 bytes |
| 1 GiB | gibibyte | 1,073,741,824 bytes |

for clear distinction in core utils such as ls, df, du, use either --iec or --block-size flag.
