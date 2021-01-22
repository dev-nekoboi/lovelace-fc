# Lovelace <a name="top"></a>
## Assembly
[Quick Reference](#quick-reference)

### Numbers <a name="numbers"></a>
[Back to Top](#top)

Formatting: <a name="numbers-formatting"></a>
- Hexadecimal numbers must be prefixed with `$`.
- Binary numbers must be prefixed with `%`.
- Decimal numbers may be prefixed with `!` (optional).
- Literals must be prefixed with `#`. `#` should precede `$`, `%`, and `!`.
- Numbers not prefixed with `#` are addresses.

Examples: <a name="numbers-examples"></a>
- `#$10`: 16 as a literal
- `#%10`: 2  as a literal
- `#!10`/`#10`: 10 as a literal
- `$80`: `128` as an address
- `$280`: `640` as an address

### Labels <a name="labels"></a>
[Back to Top](#top)

Labels can be used in place of an address to jump to where the label was defined. A label can be accessed from anywhere in the file.  
Labels must be:
- Prefixed with `:`.
- Alone on the line where they are defined.

You may prefix a label with `>` or `<` to get the most and least significant bytes respectively as a literal. Both `>` and `<` precede `:`.

### Flags <a name="flags"></a>
[Back to Top](#top)

#### S - Sign <a name="flags-sign"></a>
Position in flag register: `x0000000`  
Set by instructions with a numerical result if that result is negative.

#### C - Carry <a name="flags-carry"></a>
Position in flag register: `0x000000`  
Set by arithmetic instructions if a 1 bit in the 9th position was truncated.

#### I - Interrupt Disable <a name="flags-interrupt-disable"></a>
Position in flag register: `00x00000`  
Only ever set manually. Enables or disables interrupts.

#### O - Overflow <a name="flags-overflow"></a>
Position in flag register: `000x0000`  
Set by arithmetic instructions if the sign of the result is affected by truncation.

#### G - Greater <a name="flags-greater"></a>
Position in flag register: `000000x0`  
Set by the CMP instruction if the first operand is greater than the second.

#### Z - Zero <a name="flags-zero"></a>
Position in flag register: `0000000x`  
Set by instructions with a numerical result if that result is 0, or by the CMP instruction if its operands are equal.


### Comments (does not apply to the mini-assembler) <a name="notes"></a>
[Back to Top](#top)

Anything following a `;` on a line is a comment, and is not compiled.

### CPU/ASM Notes
- LovelaceCPU is big-endian system.
- Instruction operands are written in form \<source> \<destination>.
- All operands are 8-bit, excepting adr which is 16-bit.

### Documentation Notes
- reg represents one of `r0`, `r1`, `r2`, or `r3`.
- acc refers to the accumulator.
- lit represents an 8-bit literal.
- flg refers to the flags register.
- sp refers to the stack poiner.
- adr represents an address.
- zp represents a zeropage address.

### Instructions <a name="instructions"></a>
[Back to Top](#top)

#### NOP <a name="instructions-nop"></a>
- No operation
- Opcodes:
- 0x01
	- 1 Cycle
	- NOP

#### STR \<reg/acc/lit> \<adr/zp> <a name="instructions-str"></a>
- Stores data from source operand to destination address
- Opcodes:
	- 0x09
		- 4 cycles
		- STR r0 \<adr>
	- 0x0A
		- 4 cycles
		- STR r1 \<adr>
	- 0x0B
		- 4 cycles
		- STR r2 \<adr>
	- 0x0C
		- 4 cycles
		- STR r3 \<adr>
	- 0x0D
		- 4 cycles
		- STR acc \<adr>
	- 0x0E
		- 4 cycles
		- STR \<lit> \<adr>
	- 0x0F
		- 3 cycles
		- STR r0 \<zp>
	- 0x10
		- 3 cycles
		- STR r1 \<zp>
	- 0x11
		- 3 cycles
		- STR r2 \<zp>
	- 0x12
		- 3 cycles
		- STR r3 \<zp>
	- 0x13
		- 3 cycles
		- STR acc \<zp>
	- 0x14
		- 3 cycles
		- STR \<lit> \<zp>

#### STRI \<reg/acc/lit> \<adr> <a name="instructions-stri"></a>
- Stores data from source operand to address pointed to by destination address
- Opcodes:
	- 0x15
		- 6 cycles
		- STRI r0 \<adr>
	- 0x16
		- 6 cycles
		- STRI r1 \<adr>
	- 0x17
		- 6 cycles
		- STRI r2 \<adr>
	- 0x18
		- 6 cycles
		- STRI r3 \<adr>
	- 0x19
		- 6 cycles
		- STRI acc \<adr>
	- 0x56
		- 7 cycles
		- STRI \<lit> \<adr>

#### STRIZ \<reg/acc/lit> \<zp> <a name="instructions-striz"></a>
- Stores data from source operand to address pointed to by destination zero-page address
- Opcodes:
	- 0x1A
		- 5 cycles
		- STRI r0 \<zp>
	- 0x1B
		- 5 cycles
		- STRI r1 \<zp>
	- 0x1C
		- 5 cycles
		- STRI r2 \<zp>
	- 0x1D
		- 5 cycles
		- STRI r3 \<zp>
	- 0x1E
		- 5 cycles
		- STRI acc \<zp>
	- 0x57
		- 6 cycles
		- STRIZ \<lit> \<adr>

#### LOD \<lit/adr/zp> \<reg/acc> <a name="instructions-lod"></a>
- Loads data from source operand into destination register
- Opcodes:
	- 0x1F
		- 4 cycles
		- LOD \<adr> r0
	- 0x20
		- 4 cycles
		- LOD \<adr> r1
	- 0x21
		- 4 cycles
		- LOD \<adr> r2
	- 0x22
		- 4 cycles
		- LOD \<adr> r3
	- 0x23
		- 4 cycles
		- LOD \<adr> acc
	- 0x24
		- 3 cycles
		- LOD \<zp> r0
	- 0x25
		- 3 cycles
		- LOD \<zp> r1
	- 0x26
		- 3 cycles
		- LOD \<zp> r2
	- 0x27
		- 3 cycles
		- LOD \<zp> r3
	- 0x28
		- 3 cycles
		- LOD \<zp> acc
	- 0x29
		- 2 cycles
		- LOD \<lit> r0
	- 0x2A
		- 2 cycles
		- LOD \<lit> r1
	- 0x2B
		- 2 cycles
		- LOD \<lit> r2
	- 0x2C
		- 2 cycles
		- LOD \<lit> r3
	- 0x2D
		- 2 cycles
		- LOD \<lit> acc

#### LODI \<adr> \<reg/acc> <a name="instructions-lodi"></a>
- Loads data from address pointed to by source address into destination register
- Opcodes:
	- 0x2E
		- 6 cycles
		- LODI \<adr> r0
	- 0x2F
		- 6 cycles
		- LODI \<adr> r1
	- 0x30
		- 6 cycles
		- LODI \<adr> r2
	- 0x31
		- 6 cycles
		- LODI \<adr> r3
	- 0x32
		- 6 cycles
		- LODI \<adr> acc

#### LODIZ \<zp> \<reg/acc> <a name="instructions-lodiz"></a>
- Loads data from address pointed to by source zero-page address into destination register
- Opcodes:
	- 0x33
		- 5 cycles
		- LODI \<zp> r0
	- 0x34
		- 5 cycles
		- LODI \<zp> r1
	- 0x35
		- 5 cycles
		- LODI \<zp> r2
	- 0x36
		- 5 cycles
		- LODI \<zp> r3
	- 0x37
		- 5 cycles
		- LODI \<zp> acc

#### TRN \<reg/acc/sp/flg> \<reg/acc> <a name="instructions-trn"></a>
- Copies value from source register to destination register
- Opcodes:
	- 0x38
		- 1 cycle
		- TRN r0 r1
	- 0x39
		- 1 cycle
		- TRN r0 r2
	- 0x3A
		- 1 cycle
		- TRN r0 r3
	- 0x3B
		- 1 cycle
		- TRN r0 acc
	- 0x3C
		- 1 cycle
		- TRN r1 r0
	- 0x3D
		- 1 cycle
		- TRN r1 r2
	- 0x3E
		- 1 cycle
		- TRN r1 r3
	- 0x3F
		- 1 cycle
		- TRN r1 acc
	- 0x40
		- 1 cycle
		- TRN r2 r0
	- 0x41
		- 1 cycle
		- TRN r2 r1
	- 0x42
		- 1 cycle
		- TRN r2 r3
	- 0x43
		- 1 cycle
		- TRN r2 acc
	- 0x44
		- 1 cycle
		- TRN r3 r0
	- 0x45
		- 1 cycle
		- TRN r3 r1
	- 0x46
		- 1 cycle
		- TRN r3 r2
	- 0x47
		- 1 cycle
		- TRN r3 acc
	- 0x48
		- 1 cycle
		- TRN acc r0
	- 0x49
		- 1 cycle
		- TRN acc r1
	- 0x4A
		- 1 cycle
		- TRN acc r2
	- 0x4B
		- 1 cycle
		- TRN acc r3
	- 0x4C
		- 1 cycle
		- TRN sp r0
	- 0x4D
		- 1 cycle
		- TRN sp r1
	- 0x4E
		- 1 cycle
		- TRN sp r2
	- 0x4F
		- 1 cycle
		- TRN sp r3
	- 0x50
		- 1 cycle
		- TRN sp acc
	- 0x51
		- 1 cycle
		- TRN flg r0
	- 0x52
		- 1 cycle
		- TRN flg r1
	- 0x53
		- 1 cycle
		- TRN flg r2
	- 0x54
		- 1 cycle
		- TRN flg r3
	- 0x55
		- 1 cycle
		- TRN flg acc

#### ADD \<reg/acc/lit> <a name="instructions-add"></a>
- Adds the source to the accumulator, setting some flags.
- Flags: S, Z, O, C
- Opcodes:
	- 0x5e
		- 1 cycle
		- ADD r0
	- 0x5f
		- 1 cycle
		- ADD r1
	- 0x60
		- 1 cycle
		- ADD r2
	- 0x61
		- 1 cycle
		- ADD r3
	- 0x62
		- 1 cycle
		- ADD acc
	- 0x63
		- 2 cycles
		- ADD \<lit>

#### ADDC \<reg/acc/lit> <a name="instructions-addc"></a>
- Adds the source and the carry flag to the accumulator, setting some flags.
- Flags: S, Z, O, C
- Opcodes:
	- 0x64
		- 1 cycle
		- ADDC r0
	- 0x65
		- 1 cycle
		- ADDC r1
	- 0x66
		- 1 cycle
		- ADDC r2
	- 0x67
		- 1 cycle
		- ADDC r3
	- 0x68
		- 1 cycle
		- ADDC acc
	- 0x69
		- 2 cycles
		- ADDC \<lit>

#### SUB \<reg/lit> <a name="instructions-sub"></a>
- Subtracts the source to the accumulator, setting some flags.
- Flags: S, Z, O, C
- Opcodes:
	- 0x6a
		- 1 cycle
		- SUB r0
	- 0x6b
		- 1 cycle
		- SUB r1
	- 0x6c
		- 1 cycle
		- SUB r2
	- 0x6d
		- 1 cycle
		- SUB r3
	- 0x6e
		- 2 cycles
		- SUB \<lit>

#### SUBC \<reg/lit> <a name="instructions-subc"></a>
- Subtracts the source and the carry flag to the accumulator, setting some flags.
- Flags: S, Z, O, C
- Opcodes:
	- 0x6f
		- 1 cycle
		- SUBC r0
	- 0x70
		- 1 cycle
		- SUBC r1
	- 0x71
		- 1 cycle
		- SUBC r2
	- 0x72
		- 1 cycle
		- SUBC r3
	- 0x73
		- 2 cycles
		- SUBC \<lit>

#### SHR \<reg/acc/lit> <a name="instructions-shr"></a>
- Bitshifts the accumulator to the right, setting some flags.
- Flags: S, Z
- Opcodes:
	- 0x74
		- 1 cycle
		- SHR r0
	- 0x75
		- 1 cycle
		- SHR r1
	- 0x76
		- 1 cycle
		- SHR r2
	- 0x77
		- 1 cycle
		- SHR r3
	- 0x78
		- 1 cycle
		- SHR acc
	- 0x79
		- 2 cycles
		- SHR \<lit>

#### SHL \<reg/acc/lit> <a name="instructions-shl"></a>
- Bitshifts the accumulator to the left, setting some flags.
- Flags: S, Z
- Opcodes:
	- 0x7a
		- 1 cycle
		- SHL r0
	- 0x7b
		- 1 cycle
		- SHL r1
	- 0x7c
		- 1 cycle
		- SHL r2
	- 0x7d
		- 1 cycle
		- SHL r3
	- 0x7e
		- 1 cycle
		- SHL acc
	- 0x7f
		- 2 cycles
		- SHL \<lit>

#### AND \<reg/lit> <a name="instructions-and"></a>
- Applies a bitwise and to the accumulator, setting some flags.
- Flags: S, Z
- Opcodes:
	- 0x80
		- 1 cycle
		- AND r0
	- 0x81
		- 1 cycle
		- AND r1
	- 0x82
		- 1 cycle
		- AND r2
	- 0x83
		- 1 cycle
		- AND r3
	- 0x84
		- 2 cycles
		- AND \<lit>

#### OR \<reg/lit> <a name="instructions-or"></a>
- Applies a bitwise or to the accumulator, setting some flags.
- Flags: S, Z
- Opcodes:
	- 0x85
		- 1 cycle
		- OR r0
	- 0x86
		- 1 cycle
		- OR r1
	- 0x87
		- 1 cycle
		- OR r2
	- 0x88
		- 1 cycle
		- OR r3
	- 0x89
		- 2 cycles
		- OR \<lit>
#### XOR \<reg/lit> <a name="instructions-xor"></a>
- Applies a bitwise xor to the accumulator, setting some flags.
- Flags: S, Z
- Opcodes:
	- 0x8a
		- 1 cycle
		- XOR r0
	- 0x8b
		- 1 cycle
		- XOR r1
	- 0x8c
		- 1 cycle
		- XOR r2
	- 0x8d
		- 1 cycle
		- XOR r3
	- 0x8e
		- 2 cycles
		- XOR \<lit>

#### NOT <a name="instructions-not"></a>
- Applies a bitwise not to the accumulator, setting some flags.
- Flags: S, Z
- Opcodes:
	- 0x8f
		- 1 cycle
		- NOT

#### INC \<reg/acc> <a name="instructions-inc"></a>
- Adds 1 to the argument, setting some flags.
- Flags: S, Z, O, C
- Opcodes:
	- 0x90
		- 1 cycle
		- INC r0
	- 0x91
		- 1 cycle
		- INC r1
	- 0x92
		- 1 cycle
		- INC r2
	- 0x93
		- 1 cycle
		- INC r3
	- 0x94
		- 1 cycle
		- INC acc

#### DEC \<reg/acc> <a name="instructions-dec"></a>
- Subtracts 1 to the argument, setting some flags.
- Flags: S, Z, O, C
- Opcodes:
	- 0x95
		- 1 cycle
		- DEC r0
	- 0x96
		- 1 cycle
		- DEC r1
	- 0x97
		- 1 cycle
		- DEC r2
	- 0x98
		- 1 cycle
		- DEC r3
	- 0x99
		- 1 cycle
		- DEC acc
#### PSH \<reg/acc/lit> <a name="instructions-psh"></a>
- Pushes a value to the stack. This increments the stack pointer.
- Opcodes:
	- 0xa2
		- 2 cycles
		- PUSH r0
	- 0xa3
		- 2 cycles
		- PUSH r1
	- 0xa4
		- 2 cycles
		- PUSH r2
	- 0xa5
		- 2 cycles
		- PUSH r3
	- 0xa6
		- 2 cycles
		- PUSH acc
	- 0xa7
		- 3 cycles
		- PUSH \<lit>

#### POP \<reg/acc> <a name="instructions-pop"></a>
- Pops a value from the stack. This decrements the stack pointer.
- Opcodes:
	- 0xa8
		- 2 cycles
		- POP r0
	- 0xa9
		- 2 cycles
		- POP r1
	- 0xaa
		- 2 cycles
		- POP r2
	- 0xab
		- 2 cycles
		- POP r3
	- 0xac
		- 2 cycles
		- POP acc

#### DRP <a name="instructions-drp"></a>
- Drops a value from the stack. This decrements the stack pointer.
- Opcodes:
	- 0xad
		- 1 cycle
		- DROP

#### CMP \<reg/acc> \<reg/acc/lit> <a name="instructions-cmp"></a>
- Compares two values. The Z flag is set if they are equal, and the G flag is set if the first is greater than the second.
- Note that comparisons are unsigned.
- Opcodes:
	- 0xb6
		- 1 cycle
		- CMP r0 r1
	- 0xb7
		- 1 cycle
		- CMP r0 r2
	- 0xb8
		- 1 cycle
		- CMP r0 r3
	- 0xb9
		- 1 cycle
		- CMP r0 acc
	- 0xba
		- 1 cycle
		- CMP r1 r0
	- 0xbb
		- 1 cycle
		- CMP r1 r2
	- 0xbc
		- 1 cycle
		- CMP r1 r3
	- 0xbd
		- 1 cycle
		- CMP r1 acc
	- 0xbe
		- 1 cycle
		- CMP r2 r0
	- 0xbf
		- 1 cycle
		- CMP r2 r1
	- 0xc0
		- 1 cycle
		- CMP r2 r3
	- 0xc1
		- 1 cycle
		- CMP r2 acc
	- 0xc2
		- 1 cycle
		- CMP r3 r0
	- 0xc3
		- 1 cycle
		- CMP r3 r1
	- 0xc4
		- 1 cycle
		- CMP r3 r2
	- 0xc5
		- 1 cycle
		- CMP r3 acc
	- 0xc6
		- 1 cycle
		- CMP acc r0
	- 0xc7
		- 1 cycle
		- CMP acc r1
	- 0xc8
		- 1 cycle
		- CMP acc r2
	- 0xc9
		- 1 cycle
		- CMP acc r3
	- 0xca
		- 2 cycles
		- CMP r0 \<lit>
	- 0xcb
		- 2 cycles
		- CMP r1 \<lit>
	- 0xcc
		- 2 cycles
		- CMP r2 \<lit>
	- 0xcd
		- 2 cycles
		- CMP r3 \<lit>
	- 0xce
		- 2 cycles
		- CMP acc \<lit>

#### BRA \<flg> \<adr> <a name="instructions-bra"></a>
- Acts like a JMP conditional on the flag being set.
- Opcodes:
	- 0xcf
		- 3 cycles if jump, 1 otherwise
		- BRA S
	- 0xd0
		- 3 cycles if jump, 1 otherwise
		- BRA C
	- 0xd1
		- 3 cycles if jump, 1 otherwise
		- BRA I
	- 0xd2
		- 3 cycles if jump, 1 otherwise
		- BRA O
	- 0xd3
		- 3 cycles if jump, 1 otherwise
		- BRA G
	- 0xd4
		- 3 cycles if jump, 1 otherwise
		- BRA Z

#### BRAN \<flg> \<adr> <a name="instructions-bran"></a>
- Acts like a JMP conditional on the flag being clear.
- Opcodes:
	- 0xd5
		- 3 cycles if jump, 1 otherwise
		- BRAN S
	- 0xd6
		- 3 cycles if jump, 1 otherwise
		- BRAN C
	- 0xd7
		- 3 cycles if jump, 1 otherwise
		- BRAN I
	- 0xd8
		- 3 cycles if jump, 1 otherwise
		- BRAN O
	- 0xd9
		- 3 cycles if jump, 1 otherwise
		- BRAN G
	- 0xda
		- 3 cycles if jump, 1 otherwise
		- BRAN Z

#### JMP \<adr> <a name="instructions-jmp"></a>
- Sets the program counter to the address.
- Opcodes:
	- 0xdb
		- 3 cycles
		- JMP \<adr>

#### JMPI \<adr> <a name="instructions-jmpi"></a>
- Jumps to address pointed to by given address.
- Opcodes:
	- 0xdc
		- 5 cycles
		- JMP \<adr>

#### JSR \<adr> <a name="instructions-jsr"></a>
- Pushes the PC to the stack and jumps to the given address.
- Opcodes:
	- 0xdd
		- 5 cycles
		- JSR \<adr>

#### JSRI \<adr> <a name="instructions-jsri"></a>
- Pushes the PC to the stack and jumps to the address pointed to by the given address.
- Opcodes:
	- 0xde
		- 7 cycles
		- JSRI \<adr>

#### RET <a name="instructions-ret"></a>
- Pops an address off of the stack and jumps to it.
- Opcodes:
	- 0xdf
		- 3 cycles
		- RET

#### RETI <a name="instructions-reti"></a>
- Pops an address off of the stack and jumps to it. Also pops the flags register.
- Opcodes:
	- 0xe0
		- 4 cycles
		- RETI

#### SET \<flg> <a name="instructions-set"></a>
- Sets a given flag.
- Opcodes:
	- 0xe9
		- 1 cycle
		- SET S
	- 0xea
		- 1 cycle
		- SET C
	- 0xeb
		- 1 cycle
		- SET I
	- 0xec
		- 1 cycle
		- SET O
	- 0xed
		- 1 cycle
		- SET G
	- 0xee
		- 1 cycle
		- SET Z

#### CLR \<flg> <a name="instructions-clr"></a>
- Clears a given flag.
- Opcodes:
	- 0xef
		- 1 cycle
		- CLR S
	- 0xf0
		- 1 cycle
		- CLR C
	- 0xf1
		- 1 cycle
		- CLR I
	- 0xf2
		- 1 cycle
		- CLR O
	- 0xf3
		- 1 cycle
		- CLR G
	- 0xf4
		- 1 cycle
		- CLR Z


### Pseudo-Instructions <a name="pseudo-instructions"></a>
[Back to Top](#top)
#### ADR \<name> \<adr/zp> <a name="pseudo-instructions-adr"></a>
Creates a constant that you can use instead for addresses instead of typing the number

#### LIT \<name> \<lit> <a name="pseudo-instructions-lit"></a>
Creates a constant that you can use instead for literals instead of typing the number

#### BYTES \<8-bit value> ... <a name="pseudo-instructions-bytes"></a>
Inserts arbitrary number of 8-bit values into the bytecode at current location  
Values must be prefixed with `#`  
If you do not then undefined behavior occurs

### Quick Reference <a name="quick-reference"></a>
[Back to Top](#top)
- [Numbers](#numbers)
	- [Formatting](#numbers-formatting)
	- [Examples](#numbers-examples)
- [Labels](#labels)
- [Flags](#flags)
	- [Sign (S)](#flags-sign)
	- [Carry (C)](#flags-carry)
	- [Interrupt Disable (I)](#flags-interrupt-disable)
	- [Overflow (O)](#flags-overflow)
	- [Greater (G)](#flags-greater)
	- [Zero (Z)](#flags-zero)
- [Notes](#notes)
- [Instructions](#instructions)
	- [NOP](#instructions-nop)
	- [STR](#instructions-str)
	- [STRI](#instructions-stri)
	- [STRIZ](#instructions-striz)
	- [LOD](#instructions-lod)
	- [LODI](#instructions-lodi)
	- [LODIZ](#instructions-lodiz)
	- [TRN](#instructions-trn)
	- [ADD](#instructions-add)
	- [ADDC](#instructions-addc)
	- [SUB](#instructions-sub)
	- [SUBC](#instructions-subc)
	- [SHR](#instructions-shr)
	- [SHL](#instructions-shl)
	- [AND](#instructions-and)
	- [OR](#instructions-or)
	- [XOR](#instructions-xor)
	- [NOT](#instructions-not)
	- [INC](#instructions-inc)
	- [DEC](#instructions-dec)
	- [PSH](#instructions-psh)
	- [POP](#instructions-pop)
	- [DRP](#instructions-drp)
	- [CMP](#instructions-cmp)
	- [BRA](#instructions-bra)
	- [BRAN](#instructions-bran)
	- [JMP](#instructions-jmp)
	- [JMPI](#instructions-jmpi)
	- [JSR](#instructions-jsr)
	- [JSRI](#instructions-jsri)
	- [RET](#instructions-ret)
	- [RETI](#instructions-reti)
	- [SET](#instructions-set)
	- [CLR](#instructions-clr)
- [Pseudo-Instructions](#pseudo-instructions)
	- [ADR](#pseudo-instructions-adr)
	- [LIT](#pseudo-instructions-lit)
	- [BYTES](#pseudo-instructions-bytes)

[Back to Top](#top)
