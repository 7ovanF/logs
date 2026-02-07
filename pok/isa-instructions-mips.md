# MIPS and Such

## ISA

Describes instructions, ...
> WIP

> **microarchitecture** A.K.A computer organization: **a hardware design; the way an ISA is implemented in a particular processor**.
> defines the way a processor executes instructions, handles data, and performs various operations, all of which are defined by the ISA.
> A microarchitecture is considered compliant to an ISA if it is able to correctly execute all instructions defined by the ISA and produce the architecturally specified results.
> 
> an ISA can be implemented with different microarchitectures, corresponding to the goals the machine intends to achieve.
>
> [Wikipedia](https://en.wikipedia.org/wiki/Microarchitecture) [Codasip](https://codasip.com/glossary/microarchitecture/)

---
## MIPS

**MIPS** is a **RISC ISA** characterized by a load/store architecture, fixed-length instructions, a large uniform register file, and simple addressing modes.
this results in **simple, fast, and predictable hardware-level operations**. 
The MIPS ISA is implemented in different processor microarchitectures for reasons mostly related to simplicity, but is mostly extinct, outside of educational environments.

Such idea can be observed in the fact that instruction sizes are fixed to 32 bits (unlike x86 with variable length instructions).
As such, the CPU will work the exact same way each time it works: 
The PC will increment by 4 (if no control-flow instructions are specified), instruction decoding is simplified as instruction fields are in fixed positions, etc.

MIPS is not simply an assembly language. MIPS Assembly is just the human-readable representation of the MIPS ISA's machine code\*.
The ISA still exists without the assembly syntax, as a binary encoding.

\**such are assembly languages in general: they are just human-readable representations of an ISA's defined binary codes.*

---
## Instructions
fixed-size binary word.

the CPU can
1. fetch it from memory
2. decode (to know what to do)
3. execute it

particularly, in the Von Neumann architecture, it is transmitted as data throughout the system.

### Parts of an Instruction
#### 1. Opcode = Operation code.
Tells the CPU what kind of instruction is used; selects datapath behavior.

Specifically how it works: it issues control signals by
1. Enable certain control signals
2. Disable others
3. Routes data through specific hardware paths

ex: add, sub, load, branch.

#### 2. Operands: Data to Use
Specifies where the data comes from; whether it be the registers, memory, or just immediates.

*note: memory operands require address calculations.*

#### 3. Destination
Specifies where the result will go. Can be registers, memory, or the Program Counter (specifically for branches or jumps).

#### 4. Control / Modifiers
Extra information, such as shift amounts, condition codes, addressing modes.

The opcode alone cannot define certain specifics of how the datapath will behave.

### MIPS Case

in MIPS, instructions are 32-bit long.

Opcodes are still essential.

rs, rt, rd: names of fields inside an instruction. defines which registers the CPU use and what to do with each.
- rs: source register
- rt: target register
- rd: destination register
*These aren't the registers themselves, think of them as roles/slots.*

> WIP: funct and shamt

existence of each depends on the format; order and role depends on the specific instruction/opcode. More in this section:

#### RIJ Formats
formats that instructions come in for MIPS, partitions each 32-bit instruction.

- R-type
```
31        26 25   21 20   16 15   11 10   6 5        0
+-----------+-------+-------+-------+-------+--------+
|  opcode   |  rs   |  rt   |  rd   | shamt | funct  |
+-----------+-------+-------+-------+-------+--------+
   (6)        (5)     (5)     (5)     (5)      (6)
```
rd as destination, rs and rt as source register 1 and 2

for register to register operations.

Examples: add, sub.

` add $t0, $t1, $t2 `
$t0 as rd (dest), $t1 as rs (src), $t2 as rt (target).

demonstrates how the format doesn't define the order in which the rs, rt, and rd fields are written.


- I-type
```
31        26 25   21 20   16 15                    0
+-----------+-------+-------+----------------------+
|  opcode   |  rs   |  rt   |      immediate       |
+-----------+-------+-------+----------------------+
   (6)        (5)     (5)            (16)
```
rs as source, rt as destination. immediate is literal used.

I-type instruction opcodes: 
- immediates (ex: addi)
- stores (ex: sw)
- loads (ex: la)
- branches (ex: beq)

- J-type
```
31        26 25                                  0
+-----------+------------------------------------+
|  opcode   |            address                |
+-----------+------------------------------------+
   (6)                    (26)
```
no registers.
ex: jump.
