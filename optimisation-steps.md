time -p; getrusage();

file size
https://justine.lol/sizetricks
https://codegolf.stackexchange.com/questions/215216/high-throughput-fizz-buzz/236630#236630

more important to understand how CPU and memory work than language involved
in an OS, you will get given a zero-page due to security concerns


recording information:
we have the option of constructor/destructor pairs
if we want to determine best possible time if all caches align etc. 'hunt for mininum', e.g. record mininum time execution in loop iteration or re-run if smaller time yielded
alternatively, we could develop a statistical breakdown of values (could see moments when kernel switches us out etc.)


agnerfog optimise website 'what's a creel'

threading
simd

caching
https://akkadia.org/drepper/cpumemory.pdf
1. know cache sizes to have data fit in it
2. know cache line sizes to ensure data is close together (may have to separate components of structs to allow loops to access less cache lines) 
i.e. understand what you operate on frequently. may also have to align struct 
3. simple, linear access patterns (or prefetch instructions) for things larger than cache size 

inline assembly (raw syscalls from github)
HAVE TO INSPECT/VERIFY ASSEMBLY IS SANE FIRST THEN LOOK AT TIMING INFORMATION
inspecting compiler generated assembly loops, look for JMP to ascertain looping condition
due to macro-op fusion (relevent to say Skylake), e.g. cmp-jmp non-programmable instructions could be executed by the cpu
similarly, instructions that only exist on the frontend but exist programmatically e.g. xmm to xmm might just be a renaming in register allocation table
also due to concurrent port usage, can identify parts of code as relatively 'free'
struct access typically off a [base pointer]
in assembly, 1.0f might be large number e.g. 1065353216
in assembly loop, repeated instructions may be due to loop unrolling
we might see:
* superfluous loading of values off stack
* more instructions required, e.g not efficiently using SIMD 
(often this exposes the misconception that compilers are better than programmers; so better to handwrite intrinsics)


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
