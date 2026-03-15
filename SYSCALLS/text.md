a thought crossed my mind that this is probably not worth it, maybe i should just make an article about wifi. but hey, i havent even found out the significance in knowing about this, maybe in the cybersec world?

# SYSCALL

We've been writing code in MIPS for a while now. After a month or so of learning about computer organizationand starting to code in the seemingly-archaic-but-actually-much-newer-than-x86 MIPS, we've gotten familiar with the term `syscall`, no doubt. 

We've used syscalls primarily to input and output data. We load, specifically, the number 5 into `$v0` in order to take an integer input. Do so with the number 1, and we print out a certain word-lengthed data, which we've already loaded into `$a0`, in the form of an integer. Of course, we know that these are preset numbers called **service codes**, identifiers TODO. 

I had a question. Why choose these arbitrary set of numbers? "Probably for specific reasons that align with the MIPS architecture philosophy or... Something something." At that time, I didn't even care to ask what syscalls really do, anyway. Little did I know, if a real-life computer were built with MIPS, the "print integer" service code wouldn't be necessarily "1". Ah, no, "print integer" might not even exist.

The set "1 for print integer, 2 for print double, ..." is something that was decided not by MIPS, but by MARS, the educational MIPS simulator that we know and love (Ilman doesn't). MARS gave us only the simple syscalls that matter to us, along with simple number identifiers. In real life, MIPS syscalls are different . set by kernel

TODO: is this correct?
In fact, "service code" isn't even the correct terminology. "Syscall number"

"Kernel"

---

## What is a Syscall?

**Syscall**, short for **system call**, is how the userspace interacts with the kernel. Let's make an example. An app needs to interact with a certain device; let's say, the keyboard. Problem is, no app has direct access to the keyboard, or any hardware for that matter. That privilege is held by the **kernel**. So, the app needs to send a request to the kernel, in the form of a **system call**, in order to gain information from the keyboard.

The userspace in question encompasses all running processes, like apps and daemons.

## Syscall Numbers

I've mentioned about how syscalls need a certain identifier number, preset by the kernel.

---

## Kernel Code

where it lvies

how it executes

## USER MODE vs. KERNEL MODE

## Kernel Privileges

### Does the Kernel also manage Memory Accesses?
