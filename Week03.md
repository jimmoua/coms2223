# Week 03

### Mon Jan 28 2019
Machine words are finite. This means there are a finite number of bit patterns.

In representation, we will be asked
+ How many patterns of $k$ bits do we have?

Given: $2^5 = 32$ and $2^{10} = 1024$

**Remember from golden days**: $a^{b+c} \equiv a^b \cdot a^c$

Example: How many bits used to represent $10^{17}$

$
10^{17} = 10^2 \cdot 10^3\cdot 10^3\cdot 10^3\cdot 10^3\cdot 10^3\\
\text{get everything into powers of 2}\\
\implies 57 \text{ bits}
$

Converting:

$
2573_{10}
$

Will need 12 bits because $2^{12} = 4096$ satisfies. Converting this is using
same method as in Wu's class.

#### Steps for conversion from Decimal (known value) to target base (unknown)
1. **Repeat**: divide by target leaving remainders right to left until 0.

#### To Decimal (unknown) from target base (known value).
1. **Repeat**: starting with zero while digits remain, multiply by base and add
   next digit.

**Example**: For conversion of $176532_8$ to decimal, refer to notes.

$\implies 64858_{10}$

### Wed Jan 30 2019

#### Unsigned Integers
Need to knows:
+ Converting from different number bases
+ Confirm amount of space (every machine now has some fixed word space)

#### Signed Integers
These are numbers in which they  may be positive or negative. We often
recognize whether, in base 10, a number is positive or negative because of the
sign. This is known as **sign magnitude** notation. This will work with numbers
of any counts of digits.

Refer to notes for diagram on sign magnitude
+ To negate signed magnitude, if there is a '+', we simply replace with a '-'
  sign.

Radix Complement (2's complement)
+ Early decimal computers worked with radix complement where they did 10's
  complement arithmetic.
+ To negate, subtract each digit from (radix-1) and then add 1
+ (complment bits)++

Diminished Radix Complement (1's complement)?
+ Never really went into it.
+ Complement bits

#### Practice (notes)
+ Need to practice representing in **sign magnitude**, **2's complement**,
**unsigned**.

+ 5-bit patterns $\pm 5, \pm 13, \pm 27$

### Fri Feb  1 2019

#### Negating
**Excess-k**
+ In addition, there will be an extra $k$, so we have to remove it.
+ Early decimal machines used excesss 3 notation.

#### Representing characters
Characters:
+ Letters - 52
+ Digits - 10
+ Punctuation - 30

We need a meta key to represent other characters (i.e. super key, control key,
backspace). These are known as **control characters**.

**E-BCD** - Extended binary coded decimal

The following code has 6 steps/instructions.
```c
if(ch >= 'a' && ch <= '2')
```

The following is an example of a function evaluation by table lookup.
```c
if(ch >= 'a' && ch <= '2')
  ...

/* Beging function eval by table lookup */
if(is.letter(ch))
  ...
```

For letters, we are interested in values (do not confuse with string
functions). For example, 'a' has a different value than 'z'.
