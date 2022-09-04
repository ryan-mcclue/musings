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
For example, the extra overhead of introducing a hashmap (memory allocations/copies) will result 
in this being slower for small lists (also way no dynamic memory allocations in ISR)

## Sorting Squared Array
Insertion/bubble sort preferable for small lists.
Medium we want divide-and-conquer merge/quick.
Large we want radix
A naive approach will achieve loglinear from divide-and-conquer sorting.
However, we can take advantage of the fact that the array is already sorted 
and achieve a linear solution by ... 

## Tournament Winner
In cases space and size parameters different
Can join linear operations populate and min/max determination



premptive scheduler will interrupt based on priority
non-premptive cannot interrupt task unless task manually relinquishes (high response time)
within these we have priority scheduling, i.e. assigning priority to task as oppose to say estimated execution time
round-robin has each task run for a predefined time slice and swapped out if necessary  
(Seems most are round-robin like, with variations on determining time slice quanta)
