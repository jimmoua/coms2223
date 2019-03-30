## PDP-11 System Architecture

# PDP-11 Notes

## PDP-11 Traits

Here are some features of the PDP-11/40 (since there are various PDP-11, this
the 11/40 is the one focused on in class)
+ It used a 16-bit word size (two 8-bit bytes). Had direct addressing of 32K
  16-bit words or 64K 8-bit words. (K = 1024)
+ Word or byte processing very efficient handling of 8-bit characters
+ Asynchronous operations: systems run at their highest possible speeds
+ Stack processing: hardware sequential memory manipulation made it easy to
  handle structured data, subroutines, and interrupts.
+ Had 8 GPR that were very fast (remember which register is what)
  + Registeres R0-R5 are the GPR (General Purpose Registers)
  + Register R6 is the Stack pointer
  + Register R7 is the PC (program counter)
+ Vectored interrupts: fast interrupts without device polling
+ Single and doubled operand instructions: powerful and convenient set of
  micro-programmed instructions

## PDP-11 ISA
The basic order code of the PDP-11 uses both single and double operand address
instructions.

## Octal Representation
The 16-bit PDP-11 word can be represented as a 6-digit octal word.

This is how the 16-bit word usually looks like, but we can group it and convert
the binary digits into octal. The MSB of the 16-bit word also becomes the MSB
digit of the 6-digit octal word.

```
[,,15] [14,13,12] [11,10,9] [8,7,6] [5,4,3] [2,1,0]
```

```
MOV   R5,     (R4)             
0001  000101  001100                In bits
        ^       ^ indirection bit
        | indirection bit

[0] [001] [000] [101] [001] [100]
 0     1     0     5     1     4

  which is a 6-digit octal word: 010514
```

### Converting MISC

We can convert from Binary to Octal, as shown above. To convert binary to
hexadecimal, we can convert it to its octal form. After converting it to octal
form, DROP all leading 0s (not following) and group it into groups of 4.

```
        3   4   5     Decimal
      011 100 101     Binary
      
      0111 0101       Group the binary digits into groups of 4
      1110  101       remove leading 0s and group into 4
      1110 0101       Pad in the 0s where they are need for groups of 4
         E    5       Hexadecimal
```

### 10-bit 2's Complement

To convert a positive decimal number to 10-bit 2's complement:
1. Simply convert the decimal to binary (there exists a range we can only
   support with n-bits)
2. Pad the number with 0s as MSB if needed to satisfy the 10-bit.

To convert a negative decimal number to 10-bit 2's:
1. Convert the decimal number to binary.
2. Pad the number with 0s as MSB if needed.
3. "Flip" the bits.
4. Add 1 to the binary value after flipping the bits.

## Processor Status Word

The Processor Status Word (PSW) is located at location 777776 (not that that
will be of any relevance). This contains the information on the PDP-11/40.

![PSW Diagram](./img/psw.jpg)

#### Modes
Bits 15 and 14 are the current mode (either user or kernel mode). Bits 13 and
12 represent the previous modes, and bits 11 through 8 aren't used (maybe).

#### Processor Priority

Bits 7 through 5 are the processor priority bits. If the CPU operates at a high
priority level, external devices are not allowed to interrupt it with service
requests. The higher the bit that the CPU is working on, the harder it is to
require service. Therefore, operating at lower bits will likely allow for
interrupts.

#### Conditional Codes

The conditional codes are labeled as such:
**N  Z  V  C**

```
N - 1 if last result was negative

Z - 1 if the result was zero.

V - 1 if generated a 2's complement overflow

C - 1 if had to carry from the MSB
```

### Trap Bit

The trap bit (T) when set will load a new PSW. This bit is useful for debugging
programs because you can install breakpoints with it.

## PDP-11 Addressing Modes

Data stored in memory must be access and manipulated. Handling data is
specified by the PDP-11 ISA (Instruction Set Architecture). These include
`MOV`, `ADD`, and many more.

In the PDP-11, any of the GPR can be used as a stack pointer.

### Single Operand Addressing
Examples of a single operand instruction is `CLR`, `INC`, and `TST`. (Clear,
Increment, Test).

Remember the 16-bit word size from above.
+ Bits 15 through 6 specify the op-code (operation code). The op-code defined
  the type of instruction to be executed.
+ Bits 5 throuh 0 form a six-bit field. This is called the destination address
  field **and contains two subfields**.
  1. Bits 0 through 2 specify who of the eight GPR is to be reference by the
     instruction word.
  2. Bits 3 through 5 specify how the selected register will be used
     (address-mode). **Bit 3 indicated direct or indirect addressing**.
     (Indirect addressing is also known as deferred addressing).

### Double Operand Addressing

These are operations which imply **two operands** and are handled by
instructions that specify two addresses.

**Two Operands:**
1.Source Operand
2. Destination Operand

The insturction format for the double operand instruction is:
+ Bits 15 through 12 and used for the op-code.
+ Bits 11 through 6 are used for source address. This source address is then
  used to locate the first operand.
+ Bits 5 through 0 are used for the destination address. Similarly, the
  destination address is then used to find the second operand. The manual gives
  the example of `ADD A, B` which means add the contents of the sources A to
  the contents of B. A will be unchanged after the execution, but register B
  will contain the the addition and contents of A.
