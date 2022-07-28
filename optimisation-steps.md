time -p; getrusage();

more important to understand how CPU and memory work than language involved
in an OS, you will get given a zero-page due to security concerns


recording information:
we have the option of constructor/destructor pairs
if we want to determine best possible time if all caches align etc. 'hunt for mininum', e.g. record mininum time execution in loop iteration or re-run if smaller time yielded
alternatively, we could develop a statistical breakdown of values (could see moments when kernel switches us out etc.)



threading
simd

caching
https://akkadia.org/drepper/cpumemory.pdf
importance of knowing cache line sizes


inline assembly (raw syscalls from github)
inspecting compiler generated assembly loops, look for JMP to ascertain looping condition

comparing unoptimised assembly to 'wc' see noticeable speed increase.
example of non-pessimisation

Following the basic principles of non-pessimisation, 
I make a note of the huge amount of cruft in the CRT.
The output buffering, hidden `malloc()` 'optimisations' (uncommitted memory, encounter expensive page faults later; prefer reliability/clarity over edge-performance benefits), 
OS line ending conversions, non-obvious use of mutexes etc.
Whilst these may seem like minor inconveniences, they can be insidious for performance, e.g.
`rand()` has a huge call-stack that if we replace with a simple xor shift, 
results in 3x speed up.
Although easy to criticise, it may be the situation that the CRT had to be that way 
because of C standards.
To isolate use of the CRT, wrap in functions so we can hopefully replace with system calls, 
intrinisics, etc.
To avoid the compiler having to generate a large export table of all functions, 
make them `static`
To avoid large amounts of linking, have a unity build.
