# Week 05

## Wed Feb 13 2019

### PDP-11
There are several variants of machine. This changed bytes from 6-bit to 8-bits.
8 bit bytes were I/O characters. 16-bits were small memory words.

**Choosing word size**: if we have big data, must put big data in addresses.
  + Little Endian: This stores the least significant bit first.
  + Big Endian: Storing order in which the "big-end" or MSB is stored first.

  So For example, if we are given a hexadecimal number 4F52, in Big Endian will
  store 4F first (2-bytes are required) and then 52 in the adajacent memory
  location. In storage, it is 4F52.

  In Little Endian, since the LSB (least significant bit) is stored first, 52
  will be stored and then 4F, thus the representation in memory will look like
  524F.
  
This matters because computers communicate differently for depending in
Endian-ness.

### Documentations
There are two documents that you will have to write:
1. System documentation (system manual)
2. Reference documentation (assume we know that the user already knows a lot
   about the system, so this documentation is a sort of reference to them)

### Variety of PDP-11
+ Byte-sizes, word-sizes. PDP-11 designed to work in an environment with 8bit
  bytes and 16 bit data (words).
+ So, if using ASCII, there was 65536 bytes of memory or 32768 (for 16-bit
  words)
+ Endian-ness
+ Alignment: a multi-word unit is located on an address that's a multiple of
  its size.

### Machine Language vs Assembly Lanuage
These are somewhat the same thing? They are not. Machine machine is binary. We
will express machine language in octal or hexadecimal. For assembly language,
it is a textual form of instructions. There are a total of 7 different
instructions. These instructions are turned into four bit patterns which are
then turned into another bit pattern to represent a pattern of hexadecimal. In
assembly, we want to specify data and data locations. There will be
instructions in assembly that is not machine language instructions - these are
called **pseudo-operations**.

Assembly language programs are just a bunch of line with the following forms:

```
[label] [op-code] [operands] [comment field]
```

If no label, there is at least 1 space in the label field. The operands usually
respond in what is appropriate for the op-code. The comment field usually has
some special character for it (for x86 ASM, it's a semicolon).


### Pseudo-Operations
+ Data and data locations
+ Ability to specify labels (specifying addresses).
+ Placement of various machine language parts

### Counting Operands
Idea of half an operand is a register.

The PDP-11 has 8 registers which are represented as such
+ R0, R1, R2, ... R5, R6, R7
  + R7 - Program Counter (Program register)
  + R6 - Stack pointer (does some scoping e.g. when dealing with recurrsion)
    + There are six registers to play around with.

In addition to the 8 registers, there is a **PSW** (Processor Status Word). It
has several flags in it. These represent Boolean values.

**Example**: 4 Flags
> N  Z  V  C
>
> N - Last result was negative?
>
> Z - was zero?
>
> V - Generated a 2's complement overflow?
>
> C - had a carry (borrow)?

### Addressing Modes
How an instruction specifies an operand? Operands can be in registers or
memory.
+ An operand (source or destination) is 6-bits
  + Indirection bit: the eventual 16-bit value is the **address** of the
    operand (notation for this is symbol @)

```
00 - no other change ("register") (array subscripting?)
01 - auto post increment
10 - pre auto decrement
11 - indexed
```

## Wed Feb 20 2019

### Registers and Array Subscripting

The left part of the word:

```
1 - MOV/MOVB (if destination is a register, the value is a signed-extened bit)
2 - ...
3 - ...
4 - ...
5 - ...
6 - ...
```

The right part of the word: the last three bits (register number) and the right
last are the indirection bits.

+ Registeres R0-R5 are the GPR (General Purpose Registers)
+ Register R6 is the Stack pointer
+ Register R7 is the PC (program counter)

Bits left of indirection bit:

```
00 - Register
01 - Auto (post)increment
10 - Auto (post)decrement
11 - Indexed
```

**Indirection Example 1**

```
MOV   R5,     (R4)             
0001  000101  001100                In bits
        ^       ^ indirection bit
        | indirection bit

0|001|000|101|001|100
  becomes...
    010514   In Octal
```

Move R5 into R4. This is a 16-bit instruction, so the value in R4 has to be
even.

**Indirection Example 2**

```
MOV  R5,    R4
0001 000101 000100
     ^^ These two bits are the two bits before the indirection bit.
```
