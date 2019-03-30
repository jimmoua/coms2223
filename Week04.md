# Week 04

## Mon Feb 4 2019

### Scientific/Engineering Numbers
To deal with fractional numbers, we use something called fixed point notation.
This is where the user knows where and tracks where radix point is. (Radix
point works is many number bases).

Numbers must be normalized "fraction" that satisfies $1\leq f <10$

#### Fixed Point
##### Addition
+ If radix points line up: straightforward. If they are not line up, we simply
  shift them.

##### Multiplication
+ When multiplying the numbers, computer operates on two machine words.
+ Allow radix point to be captured as a floating point type.

##### Floating Point Representation
+ We usually consider in the following order in scientific notation: sign,
  exponent, then "fractional" part.
