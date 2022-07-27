# Matching Pairs
Stretchy buf and interning

Given an array, I'm tasked with finding pairs within this array. 
In this context, a pair is any two numbers that add up to a target sum.
One first iteration, a quadratically scaling solution is found.
However, as we know that an item's match is distinct, we can obtain a linearly scaling solution with a hash map.
A basic arbitrary hash function could just be `key >> 4 + 12`. 
Assuming the size is a power of 2, we can use the size as a mask on the key `(mask - 1)`
