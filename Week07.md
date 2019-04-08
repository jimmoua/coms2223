# Wed Mar 6 2019

Just went over assembly code. PC (program counters) are in octal

```
PC
00
02
04
06
10
12
14
16
20
...
```

# Fri Mar 8 2019
No op activities? Moving registers to itself. (not case for PDP-11)

170

1 111 0??

Loop from class
```
list: .word  1,  3,  5,  7, 11, 13, 15, 17 ; define a list of numbers
      .word 16, 14, 12, 10,  6,  4,  2     ; define another list of numbers
      len = < . - list > / 2

start:  mov  #len, r0
  mov  #list, r1
  clr  r2
loop: add  (r1)+, r2
  dec  r0
  bgt  loop   ; branch greater than equal
  halt
  

  .end start
```

Output (binary)

```
000000  000001    list: .word  1, 3, 5, 7, 11, 13, 15, 17
000002  000003
000004  000005
000006  000007
000010  000011
000012  000013
000014  000015
000016  000017
000020  000016           .word 16, 14, 12, 10, 6, 4, 2
000022  000014
000024  000012
000026  000010
000030  000006
000032  000004
000034  000002
000036  012700    start:  mov  #len, r0
000040  000017
000042  012701      mov  #list, r1
000044  000000
000046  005002      clr  r2
000050  062102    loop: add  (r1)+, r2
000052  005300      dec  r0
000054  003375      bgt  loop
000056  000000      halt
```
For systems that don't support function calls, we use a jump label.  similar to
C++ and C jump labels?

# Mon Mar 11 2019

### Subroutine Linkage
Link registers holds return address. Saves the program counter and will
overwrite what was in the link register. 

Control flow sequences: need to be careful with the flow of things...

JSR (for PDP-11)
```
0 000 100 sss ddd ddd
v(SP) <- reg        ; the address of the register is pushed into stack
reg   <- PC         ; The PC that holds the loc follow JSR is put into reg
PC    <- (dest)     ; Take the SR destination and put it into the PC
```

RTS
```
reg   <- (sp)^      ; popping something off stack
PC    <- reg
```

### Parameters and Locals
Data being passed between called routines and calling routines:

```
int f(int a) { // some parameter
  int b;
  ...
  ...
  return [  ]; // some return
}
```

1. Use registers. Either caller saves registers before call (and restores after
   return) OR called saves registers at start and stores at the end.When
   invoked, certain values must go into certain registers as the code is
   specifically written for specific register locations.
2. 

# Wed Mar 13 2019

### Approaches (Subroutine)

**Data connections through registers**: all saved values will go into main memory
somewhere. This is reasonable if we don't have a lot of data being transmitted
to and forth.

**Using main memory**: putting data in convenient location that we know where
it won't be used for manipulation.

## Mon Mar 25 2019

### Machine and Assembly Language
+ Data Transfer + manipulation
+ Branches (Alternation and Iteration)
+ Branching and linking, JSR

### Parameter Passing
...

### I/O Devices
Each physical device has a controller (we can say this is a hardware-level
protocol converter ; some interaction required for I/O device).

Controller contains **3 kinds of registers**
  1. Data register (need some kind of buffer to hold data, thus need data
     registers). These registers are use to send and receive data to and from
     the I/O device.
  2. Control (for commanding the device perform some kind of action). The bits
     in this register can be either write-only or both read and write, since
     accessing this register requires modification.
  3. Status (reports the state of device). This register is often read-only.

Here is a neat little ascii image I found on the web:

The logic circuit that contains these registers is called the device
controller, and the software that communicates with the controller is called a
device driver.

```
                    +-------------------+           +-----------+
                    | Device controller |           |           |   
+-------+           |                   |<--------->|  Device   |
|       |---------->| Control register  |           |           |
|  CPU  |<----------| Status register   |           |           |
|       |<--------->| Data register     |           |           |
+-------+           |                   |           |           |
                    +-------------------+           +-----------+
```

Here, each of the I/O Registers **must** have a location in memory to which the
CPU can read and write to. It is common for small peripherals such as mice and
keyboard to be represented by a few registers, but for bigger things such as a
hard drive can be represented by dozens of registers.

More info
[here](http://www.cs.uwm.edu/classes/cs315/Bacon/Lecture/HTML/ch14s03.html)

### Evolution of I/O Devices

For more [information](https://people.cs.clemson.edu/~mark/io_hist.html)

1. Polling the device [store data-output]

  ```
        [store data-output]
    Set command
    Poll status
        [read data-input]
   
    Repeat
  ```

2. Interrupts. Here, we extend the FDE hardware cycle.
  + To perform "quasi-function call" (continuous?), CPU does useful unrelated
    work (different program is ran).
  + These quasi calls are implicit. Might happen between a test and branch if
    condition type instruction.
  + Saves the PC, PSW (process status word), LoadPC ->
  + The issue with implicit quasi calls are: where to branch to? There is an
    interrupt vector (see below).

3. Direct memory access (bus masters, bus arbitration) - abbreviated as DMA.
   Since data transfer between hard drive is limited by CPU, we can allow this
   type of data transfer (DMA). The DMA controller thus allows data management
   between the busses and the hard drive.

4. I/O Processors (some processors for speed)

### Interrupt Service Routines and Subroutine calls
For subroutine calls, the crucial thing is saving the PC before doing something
else so we can restore it later on (explicit call/save).

In contrast to interrupt SR calls, in interrupt service routine there, is no
knowledge in the program that an interrupt is about to occur therefore it can't
prepare for subroutine call. However, it's easy to save where you are, issue is
where it will branch to.

The PC is saved in **interrupt vector.** As taken from Wikipedia: An "interrupt
vector table" is a data structure that associates a list of interrupt handlers
with a list of interrupt requests in a table of interrupt vectors. Defined by
Middleton: a table of addresses of Interrupt Service Routines.
+ Must not field an interrupt before the interrupt vector is initialized.
  Because of this, we will have an "Interrupted-Enabled Flaged". Upon
  Power-on/Reset the Flag is cleared.
+ There exists a priviledged Bit. Certain Instructions (and...) only work in
  Rriviledged Mode. Upon Power On/ Reset, Flag is set. Flags are kept somewhere
  and must be restored from RTI (Returned From Interrupt).
+ Since interrupts save PC and (Processor Status Word), Return From Interupt
  restores these. These seperate bits are then put into PSW and LoadPC.

## Interupts of Subroutines

After I/O was handled, software peeps exploited the "facility" (interrupts had
to exist for I/O devices).
+ system timer for "timesharing"
+ faults - illegal instruction, address values (dividing by zero?)
+ invoking operating system actions

PDP-11 refers to these in multiple names (fundamentally the same things):
+ Software Interrupts
+ Trap
+ I/O Trap
+ Emulator Trap

Instructions were added to trigger this "facility".
