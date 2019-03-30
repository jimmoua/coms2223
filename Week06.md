# Week 06

## Mon Feb 25 2019

### Auto Increment Mode

```
MOV +(R5), R4
0 001 010 101 000 100

0 001(010 101)000 100         Increment R5 by 2 then use (R5)
Given an example array:
R5->[  ]                      R5 points to [13]
R5->[13]---->R4->[13]
    [27]
    [42]
    [33]
```

Consider following stack

```
[  ]  high address
[  ]
[  ]
[  ]
[  ]
[  ]
[  ]
[  ]  low address
```

Stacks grow from higher addresses to lower addresses.
+ As we add to the stack, we use **auto decrement mode -(Rn)**. Each time the
  instruction is executed, the contents of the register is decremented by a
  value depending on the op-code
+ To pop an element off, we would access the reigster and then use **auto
  increment (Rn)+**. With this mode, the contents inside the reigster is
  incremented each time the instruction is executed. The value of increment is
  depends on the op-code.
+ **Indexed Addressing A(Rn)** is array subscripting.

```
mov  R2,    -(R3)
0001 000010 100011
            auto decrement
```

Becomes a move-byte instruction

```
mov(B)  R2,    -(R3)
1001    000010 100011
               decrement
```

EA - Effected Address in register

## Wed Feb 27 2019

### Addressing Modes with SP & PC
Most assembly languages require operands to be processed. An operand address
provides the location in memory to where data is being stored (some
instructions do not require an operand whereas some may require a few
operands).

When an instruction registers two operands, the first is usually the
destination in memory that contains the data and the second operand is the
source.

There are three basic modes of addressing:
  1. Register addressing
  2. Immediate addressing
  3. Memory addressing

#### SP
This register must be a stack and must go toward lower addresses (stack
pointer).

```
(SP) accesses top of the stack.
OPR - , -(SP) push
OPR @(SP)+, -  pop
```

#### PC
This mode always increments by 2. PC corresponds to register 7.
+ Immediate Addressing: corresponds to auto increment. 

  ```
  010111 (27)

  PC->[01 27 03]   MOV #33, R3 (puts 33 in next mem + auto inc to next mem)
    ->[      33]
    ->[        ]
      [        ]
  ```

+ **Absolute/Direct Addressing**: In this mode, when operands are specified in
  memory addressing mode, direct access to main memory is required. This mode
  proccesses data slower because to locate the exact address, we need a start
  address (typically found in DS register) and an offset value (effective
  address).

  In direct memory addressing, one of the operands refers to a location in
  memory and the other references a register.

  ```
  ADD BYTE_VALUE, DL  ; Adds register in mem loc
  MOV GX, WORLD_VALUE ; Operand from the mem is added to register
  ```

  In direct-offset addressing, arithmetic operators are used to modify an
  address. Consider the following (table that defines some data):

  ```
  BYTE_TABLE  DB 14, 15, 22, 25      ; Tables of bytes
  WORLD_TABLE DW 134, 345, 564, 1234 ; Tables of words
  ```

  the arithmetic operations access data from the tables in memory into the
  registers.

  ```
  MOV CL, BYTE_TABLE[2]   ; Gets the 3rd element of the BYTE_TABLE
  MOV CL, BYTE_TABLE + 2  ; Gets the 3rd element of the BYTE_TABLE
  MOV CX, WORD_TABLE[3]   ; Gets the 4th element of the WORD_TABLE
  MOV CX, WORD_TABLE + 3  ; Gets the 4th element of the WORD_TABLE
  ```

  ```
      [        ]
      [01 37 03] MOV @# Label, R3
      [        ]<-- (aboslute addr for label = 1000)
      [        ]
      [01    03] MOV Label, R3 (indexed addressing)
      [    +300]
      [        ]<-- label: 1000
  ```

+ Relative Addressing (Indexed Addressing)
  This is array indexing, where we have some pointer to point to registers.

  ```
  array-->[        ]   points to the first element
          [        ]
          [        ]
          [        ]
  ```

## Fri Mar 1 2019

2 OPERAND INSTRUCTIONS:

  ```
  [WORD BYTE][SOURCE][DESTINATION]
  ```

  Where the first is the byte word, second is the source, and the third is the
  destination.

  ```
  1 MOV Move
  2 CMP      Compare - calculate (src)-(dest)
  3 BIT      Bit-test Bitwise AND, no result
  4 BIC      Bit clear dst &=~(src)
  5 BIS      Bit set (dest)|=(src)
  6 ADD/SUB
  ```

EGG: Word/Byte?
+ Reading more online regarding eggs, break your eggs on the end at which it's
  convenient.

Assemblers translate assembly language into machine language. There are two
passes through the source code:
1. Calculates memory address corresponding to each label. The current location
   counter (not program counter, just current location in code)
2. Generate code (including data)


## MISC

LOOP: | NC (flags)

Pseudo-operators
Directives

```
2 types of operands    data movement
1 1/2 operand          A.L.
  1/2 part is just the register
  1 is just whatever you're manipulating

1 operand              data manipulation
```

So there are two parts of the operand for indirection instruction:
  1. Mem location
  2. Data for manipulation
