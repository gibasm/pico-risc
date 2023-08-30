# pico-risc

A toy RISC CPU written for fun ;)

This CPU is designed to be brutally simple and consisting of as few instructions as possible.

# ISA (draft)

## Architectural state:

The observable architectural state of this cpu consists of 14 general purpose registers *r0* - *r13*.

Each register is 16-bits wide.

There are two more, special registers:

Register *r14* contains interrupt vector table address.

Register *r15* contains memory base address for specific variants of each store and load instructions.

## Instructions:

Each instruction is 16-bit wide.

| mnemonic            | operand types       | description                  | opcode  |
| ------------------- | ------------------- | ---------------------------- | ------- |
| st [dest, off], src | [reg, imm4], reg    | store src -> [dest + off]    | 0x0     |
| st [off], src       | [imm8], reg         | store src -> [r15 + off]     | 0x1     |
| ld dest, [src, off] | reg, [reg, imm4]    | load [src + off] -> dest     | 0x2     |
| ld dest, [off]      | reg, [imm8]         | load [r15 + off] -> dest     | 0x3     |
| cp dest, src        | reg, reg            | copy src -> dest             | 0x4     |
| add dest, op1, op2  | reg, reg, reg       | add op1 + op2 -> dest        | 0x5     |
| sub dest, op1, op2  | reg, reg, reg       | sub op1 - op2 -> dest        | 0x6     |
| jz [addr, off]      | [reg, imm8]         | jump if zero to [addr + off] | 0x7     |
| jmp [addr. off]     | [reg, imm8]         | jump to [addr + off]         | 0x8     |
| xor dest, op1, op2  | reg, reg, reg       | op1 xor op2 -> dest          | 0x9     |
| or dest, op1, op2   | reg, reg, reg       | op1 or op2 -> dest           | 0xA     |
| \-                  | \-                  | reserved                     | 0xB-0xF |

## Memory

The CPU is able to address up to about 6kB of memory, it's up to actual implementation to define a memory map.

## Interrupt vector table

| interrupt name | offset | description                                                    |
| -------------- | ------ | -------------------------------------------------------------- |
| IRQ\_RESET     | 0x0000 | Hardware reset                                                 |
| IRQ\_HARDFAULT | 0x0002 | Hardware fatal (non recoverable) fault                         | 
| IRQ\_MEMFAULT  | 0x0004 | Memory access failed (e.g. when writing to read-only location) | 

Other offsets are left up to implementation.
