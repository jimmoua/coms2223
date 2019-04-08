# More and CPUS

These notes are taken after the second test...

## Cache

From 2703, this is memory that is inside of the CPU. This is faster than main
memory because it's "closer" to the CPU. It's very expensive the more there is
(monetary-wise). Because it's faster, it is also smaller.

## Locality

Something of CPU... it's a property exhibited by processes. Nearby locations
that are access can exploit the the accessing of local locations. Some
significance because instructions are likely to be next to each other.
Sequencing, alternation, and looping are operations likely to have instructions
next to each other.

Locality is a property that CPUs tend to exhibit. Nearby locations that are
accessed are can exploit the accessing of nearby local locations (this is close
to the definition of locality). The processor will often access the same set of
memory over a short period of time (definition of locality).

Below are some examples where we might see locality.

### Local structures/operations (locality)
+ Sequencing alternation, looping
+ Local variables
+ Structs/Records

### Non local structures/operations (not locality)
+ Functions
+ Linked structures
+ Accessing matrices in non-major order (indexing 2D arrays non-sequentially)


## Controlling cache
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
    [ ]
  ```


### Technologies to support caches

```
    ----------------------------
    |  [       CPU        ]    |
    |    |           ↑         |
    |    |           |         |
    |    |           ↓         |
    |    | [ TAG ][ DATA ]     |  Cache
    |    | [ TAG ][ DATA ]     |
    |    | [ TAG ][ DATA ]     |
    |    | [ TAG ][ DATA ]     |
    |    |           ↑         |
    -----|-----------|----------
         |           |
         ↓           ↓     ; delivered to Cache?


    TAG - 28-bit address (he'll change this later)

CPU will write data to [ DATA ] area via [ TAG ].
```
+ Cache entry may contain multiple memory words.
  ```
  [____][____][____][____]      ; 16 bytes - 128 bits


  Address
  [________________][__|__]
         |            ↑  ↑ Byte offset in 4 byte "chunk"
         |            offset of word within cache line
         ↓
         Conceptually: address of 128-bit memory unit.
  ```
