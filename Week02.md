# Week 02

### Wed Jan 23 2019

#### Data Path
  + Refer to notes for diagram of CPU
  + Has some **control unit** $\rightarrow$ sources
  + **PC** - program counter
  + **MAR** - Memory address register
  + **MDR** - Memory data register
  + **IR** - Instruction register

#### Animating the CPU to perform F-D-E
(Refer to notebook for diagram)

**Asserting**: putting a signal into a circuit such that it behaves however is
convenience for the circuit.

#### Fetch
Assuming that we start in a state (the first state)
  + Assert: use PC
  + Assert: Load into MAR
  + Assert: Read

MAR $\leftarrow$ PC; Read

We then go to another state (assumed the second state)
  + Read;
  + PC $\leftarrow$ PC $+1$
  + Load MDR

Another state (assumed third)
  + IR $\leftarrow$ MDR;

(Fetch is three steps?) $\rightarrow$ two step also possible

#### Decode
After fetching, decode instruction and move into different series of sequences.

#### Moving Into Sequence Of States
There can be different series of sequences. Here, we are executing
instructions.
1. Data movement
2. ALU
3. Controls
4. Special instructions
  + These are the machine hood and misc instructions

#### Inputs to FSM that controls CPU
+ Need to know what's in the IR (op-code field of IR is most important)
+ Condition Codes
  + PC $\leftarrow$ = $x$, where x is some input
+ Interupt requests: Stops the program, and saves program counter value then do
  something that will get next program counter. If a interupt request is not
  called, then the program will continue as normal.
+ Machine Run/Stop (is running or not running)

### Fri Jan 25 2019
Begin with some review...
#### Review
There are inputs (IR, PSW, Interupt Request) and there are outputs (Control
wires to ALU, Buses, Registers, GPR)

#### Building Finite State Machines
Refer to notes for diagram
1. A 2D array of pairs
2. A 2D array of next state plus 1D array of output
3. A 1D array of (output, next)
  + Bitwise OR of IR into next state
  + Firmware is an example of this

#### Conceptual Function
**Diagram via notes**

Given a domain and a range, we perform things that will correspond to the
following:
1. Representation
2. Concrete form
3. Circuit Manipulation
4. Concrete form of results
5. Interpret

#### Unsigned Integers
+ Need to be able to convert among number bases
+ We use a positional number system so that in the right-most position, we have
  units and in the left-most position, we have a count of a unit.
  1. Takes number and divides it from base
  2. Leaves remainders from the right to left
  3. Keep going until we get to zero
  4. (This is basically modulus, or the operations from logic design)
