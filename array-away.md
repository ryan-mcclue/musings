# Array Away

## Matching Pairs
A quadratically scaling solution is intuitive
However, as we know every match is unique, linearly scaling solution obtained with a hash map.
C++ STL implementation of hash tables are sets (just keys) and maps
Unordered variants are raw hash maps
Ordered use self-balancing red-black-tree yielding logarithmic time
Simplest hashing function `(x >> 4 + 12) & (size - 1)`
Important to keep in mind we are executing on a physical machine and that
Big-Oh is a 'zero-cost abstraction' world.
For example, the extra overhead of introducing a hashmap (memory allocations/copies) 
will result in this being slower for small lists (also no dynamic memory allocations in ISR)
This is why C++ STL uses hybrid introsort

## Sorting Squared Array
Quadratic insertion/bubble sort preferable for small lists
Loglinear divide-and-conquer merge/quick for medium
Linear radix for large

## Tournament Winner
In cases space and size parameters different
Can join linear operations populate and min/max determination
