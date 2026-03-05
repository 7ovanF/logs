# MIPS and Such

## ISA

Abstraction of the layer between hardware and software.\
A documentation of hardware, including the possible instructions the hardware can perform.

- Organization of programmable storage.
- Data types, data structures (encoding and representations)
- Instruction formats
- Instruction set (or opcode set)
- Addressing modes (how accessing and storing is handled)
- Exceptional conditions (such as specifically handling division by 0)

### Cycle
- Fetch instruction from memory
- Decode instruction (as the ISA specifies)l
- Operand fetch
- Execute instruction
- Store result
- Next!

### What ISA explains here:
- Instruction format or decoding: how is it decoded?
- Location of operands and result:
  - where? in memory? other than memory?
  - how many explicit operands?
  - how are memory operands located?
  - which can or cannot be in memory?
  - etc
- Data type and size: floating point? integer?
- What operations are supported/available?

## Instruction Set
more primitive.

### Machine Code
Instructions are represented/encoded in binary and stored as data. 

### Assembly language
Symbolic version of machine code.

Can also provide pseudo-instructions (another abstraction above the already abstracted binary code), such as move (which is add with 0 into a certain register)\

### CISC, RISC
#### CISC
e.g. x86

Single instruction performs complex operation, as smaller program size as memory was premium. (why?)\
That means:
- less fetches
- less instructions, but very big ones.
- u should search more

#### RISC

Threw away complex instructions, such as multiplication.
However, by doing so, it requires software to combine simpler operations

A load-store architecture. Operations are done on registers, never on memory.

Pentium 4 still uses an x86 ISA (???) but uses a RISC (internal). It "forwards" each \
-> CISC ISA, but internally RISC.

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

shamt: special for shifts (R-type; if not shift, just 0). just shift amount

funct: defines behavior.\
Also only for R-types.\
Example: add has the same opcode with addu (add unsigned), but DIFFERENT FUNCT (see greensheet)

existence of each depends on the format; order and role depends on the specific instruction/opcode. More in this section:

#### RIJ Formats
formats that instructions come in for MIPS, partitions each 32-bit instruction.

##### R-type
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

> Note for shift: used are rd and rt, not rs. shamt for the shift amount.

##### I-type
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

> Note: negative immediates are represented as 2's complement.
> Note 2: branches are done with relative addressing, not abssoslute. thats because it 16 bits arent
> enough for a full address (which is 32 bits)

###### More fun fact about branching:

since the word addresses are spread across 4 bytes, it'd be on addresses 0, 4, 8, 12, 16, and so on.\
since each instruction has the length of a word (4 bytes), while each address points to a byte (byte addressing), each intruction's address is a multiple of 4.\
This allows us to omit the last two bits, because every multiple of 4 will have "00" as their LSB.\

Once executed, this will happen:
`PC = (PC+4) + (immediate x 4)`
- branch is only executed upon getting to the next line, therefore the `PC + 4`
- `immediate x 4` to append back the original address.

How it's written in assembly: we don't need to write down the actual target address. we just write down the label.\
The assembler will calculate the appropriate `immediate`, so that the final PC will land on the right address.

> [!info] NOTES FROM V
> the relative address of a branch is just the difference between the current pointer (the PC) and the target branch, -1 to account for the pipelining
> the -1 is both for instructions above and below
> In the case that we utilize the formula PC = (PC + 4) + (imm * 4), then we can substitute the formula into (x+difference of address)  = (x+4) + (imm\*4)
> the difference of address is the difference of lines times 4, so the formula becomes
> (x+difference of instructions * 4) = (x + 4 ) + ( imm\*4)
> <=> imm * 4 = difference of instructions * 4 - 4
> <=> imm = difference of instructions - 1
> Q.E.D.
> btw, to clarify, the formula should write as PC1 = (PC0 + 4) + (imm * 4), with PC1 as the program counter destination and PC0 as the current value of the program counter,
> and the imm is the immediate value written as binary in the machine language as i format.
> later, ask V how to identify which is the source, target, and destination registers in each format from the assembly form to the machine language form.
> Top 10 rappers eminem is too afraid to diss: pak addi
> V CHALLENGE: find the max value of an array in assembly
> btw i prefer that the ormula is in the orm PC1 = PC0 + (imm + 1)\*4


### J-type
```
31        26 25                                  0
+-----------+------------------------------------+
|  opcode   |            address                |
+-----------+------------------------------------+
   (6)                    (26)
```
no registers.
ex: jump.

Why not just use I-type?\
-> Because there is a need for 26 bit address jumping.\
Unlike branch, it uses **absoslute addressing**.

But thats **still not enough for full 32-bit addresses!**\
How it does it with 26 bits:
- since the last 2 digits is still 00, account for that and assume we got the 28 lower bits ready.
- The remaining leading 4 bits? naw, programs arent that big. Just take the 4 MSBs from the CURRENT PC.
You still can't aim beyond the limits.\
Means you can accurately address 2^28 address bits = 2^8 * 2^20 bits = 256 MB.

You can surpass this limit with jr (jump register), since registers can actually store a word.

> Fun fact: MIPS ISA didnt have multiplication, but now it does.
> the outputs are put into hi and lo registers, instead of the usual $vN (value)

Types of Addressing:
- Register addressing: operand (address) is a register lw $t0, 0($t1) # t1 contains the address
- Immediate addressing: operand is a constant lw $t0, 0b101010001
- Base addressing: address is a sum of a register and a contant in the instruction lw $t0, 8($t1) #t1 contains an address, and we access the information from the address with displacement of 8 bytes
- PC relative addressing: used in bne and beq, where the labels are relatively addressed according to the current position of the program counter
- Pseudo-relative addressing: used in j, where the program counter retains the first four bits after the jump, with the rest containing the relative address to the current counter.
