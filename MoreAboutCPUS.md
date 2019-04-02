# More and CPUS

## Cache
This is faster than main memory. It's very expensive the more there is
(monetary-wise). Because it's faster, it is also smaller.

## Locality

Something of CPU... it's a property exhibited by processes. Nearby locations
that are access can exploit the the accessing of local locations. Some
significance because instructions are likely to be next to each other.
Sequencing, alternation, and looping are operations likely to have instructions
next to each other.

### Local structures/operations
+ Sequencing alternation, looping
+ Local variables
+ Structs/Records

### Non local structures/operations
+ Functions
+ Linked structures
+ Accessing matrices in non-major order (indexing 2D arrays non-sequentially)

There are 3 (4) Mechanisms (fundamentals) for controlling cache.
1. Read Policy - When do data migrate into cache? (When it moves from memory to
   cache)
2. Replacement policy - which data (instructions) get evicted?
3. Write policy - When are data copied back to main memory?
4. Costs - he'll get to this one later

## Content Adressable Memory

Cache memory works through **Content Addressable Memory**.
+ This is accessed by the value that is stored there. (uses a content as
  address?)
+ Partial contents (use others that have the same values)
+ Expensive (more circuitry and takes special design time)
+ Specialized Hardware - addresses will come down there (can modify or access).
  If none of the keys respond, data will be brought up to CPU.
  ```
  |  =?     [  KEY (contains address  |     DATA VALUES          ] 
     ↓
  |  =?     [  KEY (contains address) |     DATA VALUES          ]
     ↓
  |  =?     [  KEY (contains address) |     DATA VALUES          ]
     ↓
  ```
