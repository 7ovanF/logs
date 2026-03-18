RegDst: if r type 1 (has rd)
jump: self explanatory
ALUSrc = source of ALU (read 2 or imm)
MemtoReg = memory to register (lw,lh...)
RegWrite = write to reg
MemRead = ye
MemWrite = ye
Branch = ye

ALUOp = something...
then goes to ALUControl
switch ALUOp
if 00: ALUControl = 0010 =  AND
if 01:
if 10:
if 11:
