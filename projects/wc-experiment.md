optimisation requires specialised knowledge of platform and is time consuming
performance-aware programming works towards optimisation, just not all the way

also, algorithmic optimisation is not necessarily performance aware programming (i.e. might involve math etc.)
We are learning about CPU mental model

when timing code, repeat; this will give time to show what it's like when cache is primed

count operations/cycle

IMPORTANT: theoretical increases like 4x with 4 cores etc. only seen if inside cache

1. reduce number of instructions (waste, SIMD)
2. increase speed of instructions (same instructions can be faster) (IPC, cache, multithreading)

don't think about source language. think about what it becomes, e.g. templates, garbage-collection are not a zero-cost
it's only about what the source turns into, is when we can talk about its speed

main source of modern program slowdown is wasteful instructions
consider python. if we compile interpreter from source to inspect what assembly is run, find that incredible amount of waste
to first determine operator, then more waste to store result. all that was required was a single 'add' instruction
instruction stream is hugely widened for bytecode interpreter to do its work
literally just naive 'add' program see 100x slowdown
so, we don't need to do 'optimisation' just come back to our senses
could offload to numpy, or use JIT?
NOTE: 10x overhead considered good for interpreter (this would be with bytecode) 
loop unrolling can remove say `cmp` overhead, but may increase instruction cache usage
even if stuck in a high-level, can gain some performance benefits by rearranging code
furthermore, could offload sections to C

strictly, waste is if it was necessary for the computation.
not if it might be used sometime in another problem, as this would mean technically nothing is waste

modern CPUs are superscalar/overlapped, i.e. instruction level parallelism allowing for multiple instructions per cycle
this is acheived as the CPU will look for instructions it can execute at the same time
so, instructions must not be serially dependent
the CPU does not have complex logic in ascertaining dependency chains. 
it only looks at operands. so, assumes series of 'add' on same register are serially dependent
`add a, input[i]; add a, input[i + 1]`
we can help the CPU however, noting that say a summation can be broken up into multiple dependency chains
`add a, input[i]; add b, input[i + 1]`
`TODO: quadPtrScalar faster as less expensive microps for address generation?` (compiler might not be smart enough as we have disabled SLP vectorisation?)
so, a CPU will have a max. IPC value
generally, if can reduce dependency chain, always applicable
branch misprediction is costly because of this, as have to look ahead in instruction stream to make things pipelined

JIT varied term. JIT debugging, is debugger automatically opening when program crashes
JIT compilation first compiles to something like bytecode. then at runtime, converts to machine language
this is powerful as can generate varied codepaths for same function based on run time, 
e.g. if paramater small inline, if large do something else, if called a certain way do this etc.
languages that use JIT like Javascript are not very slow because of JIT, rather language design decisions that mean it can't be optimised like C (like garbage collection)
Rust has a strong-compile language model like C
Java JVM incorporates JIT in bytecode interpretation

Not as simple to say if 4 add units then unroll 4 times. 
To determine loop unrolling factor must analyse port usage
ports perform multiple operations, e.g. add or cmp. but can only do 1 at a time
so, there might be port contention

Given a codebase, not really a constructive question to ask why written in a language
What's done is done. Just be performance-aware. Perhaps was better in another language, but that decision is made. 
If I know how fast something can run, strive to make it as fast as I can

Make decisions you know it will make a difference on say, Skylake chip
Know performance characteristics of microarchtictures and how they apply to say the pipeline execution

performance analysis one of 2 things:
1. determine behaviour of microarchitecture, i.e. peak fastest:
   if only take mean, median we don't see what it converges to
   we want to take minimum cycles, to see what will happen if 'all the stars align'
2. see what is typical
   given a mix of what is cache state, what else in pipeline, other cores doing 
   look at mean/median 

times where compiler want produce what is optimally possible, e.g. staggered parallelism

IMPORTANT: sse will still end up doing 4 add instructions internally (via a SIMD adder unit, not a scalar unit)
its main benefit is removing all the front-end work for CPU, i.e.
less instruction decoding, less dependency determinations etc.
Will still expect almost 4x speed increase

Lots of elbow grease organising data with SIMD
Can also do 16 bit lane size or even 8 bit  

Can combine IPC with SIMD
With just IPC and SIMD multipliers, can literally get 1000x increase compared to Python 


However, won't get these performance benefits if bad cache performance
So, in general we have the main problem to be optimised, i.e. the 'add'
However, we also have important overheads to this:
  * loads (reading from memory)
  so `add a, input[0]` is dependent on load
  first see if particular memory location already moved into L1 cache
  * stores (these are equivalent to loads in terms of performance)
So, want to have cache size/layout in back-of-mind to know if exceeding
We have to keep things in size of cache so that compuation performance improvements actually matter and not just waiting on loads
IMPORTANT: Peak performance when all data in L1

Multi-threading gives us literally distinct instruction streams
Smaller buffer sizes could mean overhead of adding more threads not advantageous
Multi-threading can help with cache performance as well, as effectively get bigger L1 cache, i.e. 32K x 4 = effective 128K L1 cache
(this is why if have 4 threads an approximate x4 increase is actually minimum)
On chips optimised for single thread execution, a single core occupies majority of memory bandwidth. 
So, adding more cores won't increase memory bandwidth that much (visible in situations exceeding cache size)
Threads are becoming almost as a great a multiple as waste.
Server machines literally have 96 cores
Cores are rapidly increasing in new machines. SIMD, IPC remaining steady
So, biggest speed decreases are waste, cache and threads

This great speedup from Python to C just for a naive add loop. 
So, for larger more complex programs that can utilise more hardware, even greater speedups!

Python speed up:
Using builtins faster like sum() as get rid of iteration overhead
reduce type deduction
call out to C library like numpy
integrate C directly like cython (fastest possible with SIMD etc.)
(in effect, on really need to know enough C to write optimised loops to insert into a higher level language)


1. Operational, e.g: First step is optimising math operations (as easiest. this is core competency)
2. Input: how did the data get here, e.g. points for math operation (could be JSON file, which is not favourable for performance)
(probably optimising use of file, rather than OS actually opening file)
So, time how long input takes and 'math' takes  

Break down problem into parts, and find what are performance bottlenecks
Learn what we expect for parsing etc. Then make estimate for theoretical maximum


'clean code':
(should be accompanied with, your code will become a lot slower if you do them)
polymorphism (no ifs/switches)
  -- the vtables are roughly 1.5x slower than switch
  -- in perspective, this is like erasing 3-4 years of hardware gains
black box (don't know internals of anything)
  -- switch statements help align everything by function, rather than class
  -- so, we can see what commonalities we have and optimise (another reason against having classes in distinct files)
  -- knowing what we are doing specifically is key to getting optimal performance 
  -- TODO: seems that a lookup table is faster if being called repetively as values in cache
functions should be small
functions should do one thing



----------------------------------------------------------------------------------------------------
time -p; getrusage();

callbacks less CPU intensive than polling

Saying one instruction is faster than the other ignores context of execution.
e.g. mul and add same latency, however due to pipelining mul execution unit might be full
TLS vs atomics, e.g. TLS is series of instructions determined by OS and compiler.
Atomics depend on how other cores are run and synchronising necessary with other cores
So, must measure which is faster for particular situation

use $(time) for single line, use $(ctime)

Can pin a thread to a core

Can run programs in kernel space with eBPF 

Frequent context-switching will give terrible cache coherency

adding `restrict` also useful to prevent aliasing and thereby might allow
compiler to vectorise say array loops

Before cpus increased in single thread execution speed. Now more cores. It's a topic of research to convert single threaded into multithreaded for emulation. 
This is why emulation of something like the GameCube (powerpc) is slow. 
Furthermore, due to hardware irregularities that programs relied on may take hundreds of instructions to emulate
If actually a simple translation, then should run close to native speed. This is reality of emulating hardware with hardware

CISC gives reduced cache pressure for high-intensive, sustained loops

log2(n) number of bits for decimal
https://en.algorithmica.org/hpc/cpu-cache/associativity/

using genetic algorithm/machine learning to optimise for us
https://zeux.io/2020/01/22/learning-from-data/

Cpu try to guess what instructions ahead (preemptive). 
Cost of incorrect reflushing expensive. 
So want to get rid of conditional jumps. 
Ideally replace with conditional movs or arithmetic branch less techniques.
Endianness (register view), twos complement (-1 all 1s)
Branch less programming is essentially SIMD

If variable clock speed, cpu could detect not using all cores and increase single core clock

Not memory bound is best case for hyper threading
Intel speeds optimised for GPR arithmetic, boolean and flops
Intel deliberately makes mmx slow

Low cache associativity means fast lookup but a lot of misses and 
thus eviction policy like LRU

Count cycles to counteract possible thermal throttling
Hyperthreads useful only if different execution unit
Cpu reads memory from cache and ram in cache lines (due to programmer access patterns).
Each item in cache set is cache line size

if apple computers use RISC ARM in M1, why CISC necessary? (only because on Intel?)
emphasis of CISC simplify assembly (e.g. more addressing modes), thereby reducing size of binary? (reduce instructions) and increase cache coherency
RISC will require less transistors to implement complex hardware but will make
optimising harder for compiler? single cycle instructions (reduce cycles per instruction)

when looking at a pointer, to optimise compiler must know whether
it can assume it points to a local var or not.
so, easier to eliminate aliasing with non-pointers

when viewing from application in a sandboxed environment like a phone, 
total RAM less than installed as portion reserved for kernel

simplistically RAM 50ns and HDD 10µs?
faster to read than write

2.5bln * 8 (simd) * n (execution units) * 2 (cores)
assuming instructions have throughput of 1
so 64 / 4 gives how many floats per second from L1 cache
in general, not streaming from memory the entire time
(would probably hit a cache bandwidth limit)

// undefined behaviour if not true
ALWAYS_INLINE void assume(bool cond)
{
if(!(c)) {__builtin_unreachable()};
}
times when manual inlining is required: 
https://www.youtube.com/watch?v=B2BFbs0DJzw

Unless your in web where everything takes years to load, as single threaded performance is largely stagnant you will have to utilise parallelism if want performance. 
This is very difficult and like single threaded code generic libraries which can actually be very specific will create bloated in performing code based.
Multithreading is building up a new discipline to single threaded. There are a lot of pitfalls for performance (balancing want things local, however must share to utilise)
If you care about performance for anything, you should care about cache misses
Memory bandwidth and caches are major reasons for a cup attaining performance. You have to think about where does memory actually live and how is it transferred around

Optimiser allows for lexical scoping of stack variables. 
However, for optimiser to inline will have to get rid of pointers to prevent aliasing

stack and heap memory are the same physical thing, so only more efficient if memory was hot, i.e. recently touched

Computer faster than you think, e.g instructions, clock cycles, cores. Very large
With M2 drive should be quick

may be easier for optimising compiler to work with things passed by value as oppose to pointer.
so, if need to modify something may have to return the value in a functional programming style.
however, this is error prone.
compiler cannot optimise pointers, e.g. if setting two values to same pointer, compiler cannot say that both the values are the same
as another pointer may point to the same address.
this is aliasing.

we see contiguous memory in virtual memory, however in physical memory it is almost certainly going to be fragmented

compilers can auto-vectorize loops for us (and other operations if we perform say 20 of them).
so, floats will be twice as fast as doubles (more space, even though same latency and throughput)

optimising is a very precise process. only do when you have code that is working and you know will keep.
games by their very nature are about responsiveness, so optimisation and low-latency is important.
I like programming with this as a mentality.

with pure compiler optimisations, i.e. code we have not optimised ourselves, a 2x increase is not unexpected.
(code we optimised not as much)

optimise for worst case (looking out on whole world) 
not best case (in front of wall, don't render what is behind). 
we care about highest framerate, not lowest.

virtually never use lookup tables as ram memory is often 100x slower 
(so unless you can't compute in 100 of instructions)

1. Optimisation (this is rarely done due to time involved)
Do back-of-envelope calculations; look at algorithm to see if wasteful; 
benchmark to see if hitting theoretical calculations and then use timings etc. 
to isolate why our code isn't hitting these theoretical numbers
Very importantly need to know what the theoretical maximum is, 
which will be dependent on the program, 
e.g bounded by number of bytes sent to graphics card, kernel stdout etc. 
2. Non-pessimisation (do this all the time)
Don't introduce unecessary work (interpreter; complex libraries; constructs like polymorphism/classes people have convinced themselves are necessary) for the CPU to do to solve the problem
3. Fake optimisation (very bad philosophy)
Repeating context-specific statements 
e.g. use memcpy as it's optimised 
(however the speed of something is so context specific, so non-statement), 
arrays are faster than linked lists (again, so dependent on what your usage patterns are)
IMPORTANT: In programming, preface the suitability of something to a particular environment/context

When many people say too much effort involved in optimisation, 
they are generally thinking of point 1

sometimes things should be running faster than they should, however only so much we can replace
e.g. the structure of many OSs are based on legacy code, so simply outputting to stdout may go through shell then kernel etc.
also, may have to deal with pessimised libraries. in these cases:
* isolate bad code, i.e. draw hard boundary between your code and theirs by caching calls to them
* do as little modification to the data coming in from them (no need to say put in a string class etc.)

People think that it's slow, but it won't crash (because of interpreter)
Performance is critical in getting people excited about what you do, 
e.g. windows ce was laggy, iphone performant but less features changes the market 
low latency is more desirable, even if less features

count number of math ops in function and control flow logic that is evaluated
branches can get problematic if they're unpredictable

inspecting measurements of microarchitecture consider latency and throughtput 
(how much does it cost to start again)
so, FMUL may have latency of 11, however throughput of 0.5, so can start 2 every cycle, 
i.e. issue again (cpus these days are incredibly overlapped even if single-threaded)
so throughput is more of what we care about for sustained execution, e.g. in a loop

however, these numbers are all assuming the data is in the chip.
it's just as important to see how long it takes to get data to the chip.
look at cache parameters for microarchitecture; 
how many cycles to get from l1 cache, l2 cache, etc. 
when get to main memory potentially hundreds of cycles

bandwidth of L1 cache say is 80 bytes/cycle, 
so can get 20 floats per cycle (however, based on size of L1 cache, 
not really sustainable for large data)

http://igoro.com/archive/gallery-of-processor-cache-effects/

(could just shortcut this and just see if flops is recorded for our chip)

so, using these rough numbers we should be able to look at an algorithm 
(and dissect what operations it's performing, like FMULs), know how much data it's taking,
and give a rough estimate as to how long it could optimally take 
(will never hit optimal however)
IT'S CRITICAL TO KNOW HOW FAST SOMETHING SHOULD RUN

REASONS SOFTWARE IS SLOW:
1. No back-of-envelope calculations (people aren't concerned that they are running up to 1000 times slower thn what the hardware is capable of)
These calculations involve say looking at number of math ops to be performed in the algoirthm and comparing that to the perfect hardware limit
2. Reusing code (20LOC is a lot; use things that do what you want to do, but not in the way you want them to be done; 
often piling up code that is ill-fitting to what the task was, e.g. we know this isn't a regular ray cast, it's a ray cast that is always looking down)
3. When writing, thinking of goals ancillary to task 
(not many places taught how to actually write code; all high level abstractions about clean code
there are thinking about templates/classes etc. not just what does the computer actually have to do to do this task?)
(there is no metric for clean code; it's just some fictional thing people made up)

WHENEVER UNDERSTANDING CODE EXAMPLES:
1. COMPILE AND STEP INTO (NOT OVER) IN DEBUGGER AND MAKE HIGH LEVEL STEPS PERFORMED
look at these steps for duplicated/unecessary work (may pollute cache). (perhaps even asking why was the code written the way it was)
could we gather things up in a prepass, i.e. outside of loop?
if allocating memory each cycle that's game over for performance.
do we actually have to perform the same action to get the same result, e.g. a full raycast is not necessary, just segment on grid
O(n·m) is multiplicative, not linear O(n). 
big oh is just indication of how it scales. could be less given some input threshold
(big oh ignores constants, hence looking at aymptotic behaviour, i.e. limiting behaviour)
now, once code reduced, look at minimising number of ops 

cpu front end is figuring out what work it has to do, i.e. instruction decoding

e.g simd struct is 288 bytes, 4.5 cache lines, able to store 8 triangles

understanding assembly language is essential in understanding why the code might not be performing well

branch prediction necessary to ensure that the front-end can keep going and not have to wait on the back-end

execution ports execute uops.
however, the days of assembly language registers actually mapping to real registers is gone.
instead, the registers from the uops are passed through a register allocation table (if we have say 16 general purpose registers, table has about 192 entries; so a lot more) in the back-end
this is because in many programs, things can happen in any order. so to take advantage of this, the register allocation table stores dependency chains of operations
(wikichips.org for diagram)
from execution port, could be fed back into scheduler or to load/store in actual memory

when looking at assembly, when we say from memory, we actually mean from the L1 cache

xmm is a sse register (4 wide, 16 bytes); m128 is a memory operand of 128 bits
ymm is 8 wide
1*p01 + 1*p23 is saying issue 1 microop on either port0 or port1 and one microop on either port2 or port3
so, we could issue the same instruction multiple times, i.e. throughput of 0.5

microop fusion is where a microop doesn't count for your penalty as it's fused with another. with combined memory ops, e.g. `vsubps ymm8, ymm3, ymmword ptr [rdx]` this is the case 
so, if a compiler were to separate this out into a mov and then a sub, not only does this put unecessary strain on the front-end decoder it also removes microop fusion as they are now separate microops
(important to point out that I'm not the world best optimiser, or the worlds best optimisers assistant, so perhaps best not to outrightly say bad codegen, just say makes nervous)

godbolt.org good for comparing compiler outputs and possible detecting a spurious load etc.

macrop fusion is where you have an instruction that the front-end will handle for you, e.g. add and a jne will merge to addjne which will just send the 1 microp of add through 

uica.uops.info gives percentage of time instruction was on a port 
(this is useful for determining bottle-necks, e.g. series of instructions all require port 1 and 2, so cannot paralleise easily)
so, although best case say is issue instruction every 4 cycles, 
this bottleneck will give higher throughput

some levels of abstraction are necessary and good, e.g. higher level languages to assembly





Optimise: gather stats -> make estimate -> analyse efficiency and performance

file size
https://justine.lol/sizetricks
https://codegolf.stackexchange.com/questions/215216/high-throughput-fizz-buzz/236630#236630

more important to understand how CPU and memory work than language involved
in an OS, you will get given a zero-page due to security concerns


likely() macros for branch prediction compiler optimisations 
(https://akkadia.org/drepper/cpumemory.pdf, pg 56)

recording information:
We want to understand where slow with vtune, amd uprof, arm performance reports 
Next, determine if IO bound, memory bound etc.

To determine performance must have some stable metric, e.g. ops/sec to compare to
e.g measure total time and number of operations
Hyper-threading useful in alleviating memory latency, e.g. one thread is waiting to get content from RAM, 
the other hyper-thread can execute
However, as we are not memory bound (just going through pixel by pixel and not generating anything intermediate; 
will all probably stay in L1 cache), we are probably saturating the core's ALUs, so hyper-threading not as useful

Inspecting the assembly of our most expensive loop, we see that rand() is not inlined and is a call festival. This must be replaced
Essentially we are looking for mathematical functions that could be inlined and aren't that are in our hot-path.
When you want something to be fast, it should not be calling anything. If it does, probably made a mistake
Also note that using SIMD instructions, however not to their widest extent, i.e. single scalar 'ss'. Want to replace with 'ps' packed scalar

we have the option of constructor/destructor pairs
if we want to determine best possible time if all caches align etc. 'hunt for mininum', e.g. record mininum time execution in loop iteration or re-run if smaller time yielded
alternatively, we could develop a statistical breakdown of values (could see moments when kernel switches us out etc.)

(IMPORTANT save out configuration and timing information for various optimisation stages, e.g. ./app > 17-04-2022-image.txt)


agnerfog optimise website 'what's a creel'

threading
Observe CPU percentage use is not close to 100%
For multithreading, often have to pack into 64 bit value to perform single operation on it,
e.g. `delta = (val1 << 32 | val2); interlocked_add(&val, delta)`

When making multi-threaded, segregate task by writing prototype 'chunk' function, e.g. `render_tile`
Then write a for loop combining these chunk functions
Before entering the chunk function, good to have a configuration printout, e.g. num chunks, num cores, chunk dim, chunk size, etc.

When dividing a whole into pieces, an uneven divisor will give less than what's needed.
so `(total + divisor - 1) / divisor` to ensure always enough.
We will want this calculation to be in the last dividing operation, e.g. tile_width then tile_count calculated, so use on tile_count
Associated with this calculation is clamping to handle adding extra exceeding original dimensions
For getting proper place in chunk, call function wrapper for pointer location per row

(May have to inline functions?)
Next we want to pass each chunk onto a queue and then dequeue them from each logical core?
So, have a WorkOrder that will store all information required to perform operation on chunk, i.e. all parameters in `render_chunk` function
(may also store entropy for each chunk, i.e. random number series)
Then a WorkQueue that contains an array of WorkOrders with total number equalling number of chunks
So, the original loop iterating of chunks now just populates the WorkOrders 
Now in a while loop that runs while there are still chunks to execute, we call the `render_chunk` function and pass in the WorkQueue
The `render_chunk` function will increment the next_work_order_index and return true if more to be done

When spawning the actual worker thread functions, have same while loop calling the `render_chunk` function as for core 0
(the amount of threads to spawn would think should be equal to number of logical cores? however exceeding them may increase performance?)
(this debate of manually prescribing the core count applies to the chunk size as well. perhaps the sweet-spot for my machine in balancing context switching and drain out
is to manually prescribe their size as oppose to computing them off the core count)
(Collating information into the WorkQueue struct helsp for printing out configuration)
(Setting up this way, we can easily turn multithreading off)

As creating threads will require platform specific, put prototypes in main.h and the implementations in linux_main.cpp
Then include linux_main.cpp based on macro definition of platform in the build script at the bottom of main.cpp

hyperthreading, architecture specific information becomes more 
important when in a situation where memory is constrained in relation to the cache
(hyper-threads share same L1-L2 cache)

volatile says code other than what is generated by this compiler run, could modify this value.
it's required for multithreaded, as compiler may not re-read value that it may have cached in a register if changed elsewhere
when incrementing volatiles, must use a locked_add_and_return_previous_value (could return new value, just be clear)


simd
clamp can be re-written as min() and max() combination, which are instructions in SSE
Although looking at the system monitor shows cpus maxed out, we could be wasting cycles, e.g. not using SIMD

Define lane width, and divide with this to get the new loop count
Go through loop and loft used values e.g. lane_r32, lane_v3, lane_u32
(IMPORTANT at first we are only concerned with getting single values to work, later can worry about n-wide loading of values)
(TODO the current code has the slots for each lane generated, rather than unpacked. look at handmade hero for this unpacking mode)
If parameters to functions, loft them also (not functions? just parameters? however we do random_bilateral_lane() so yes to functions?)
If using struct or struct member references, take out values and loft them also, e.g. sphere.radius == lane_r32 sphere_r; 
(group struct remappings together)
Remap if statement conditions into a lane_u32 mask and remove enclosing brace hierarchy
(IMPORTANT you can still have if statements if they apply to lanes, e.g. if mask_is_zeroed() break;)
(TODO for mask_is_zeroed() we want the masks to be either all 1's or 0's)
(call mask_is_zeroed() on all masks to early out as often as possible to get a speed up)
Once lofted all if statements, & all the masks into a single mask 
(it seems if there is large amounts of code inside the if statements, you don't want to do it this way and rather check if needing to execute?)
(IMPORTANT to only & dependent masks, e.g. if there is an intermediate if like a pick_mask or clamp, then don't include it, but do the conditional assign directly on this mask)
Then enclose remaining assignments in a conditional assignment function using this single mask? 
(conditional_assign(&var, final_mask, value); this uses positive mask to get source and negative mask to get dest?)
(also discover the work around to perform binary operations on floating point numbers)
So, by end of this all values operated upon should be a lane type? (can have some scalar types if appropriate)
We may have situation where some items in a lane may finish before others.  So, introduce a lane_mask variable that indicates this. 
To indicate say a break, we can do (lane_mask = lane_mask & (hit_value == 0));
For incrementing, will have to introduce an incrementor value that will be zeroed out for the appropriate lane item that has finished.
Have horizontal_add()?
Next once everything remapped create a lane.h. Here, typedef the lane types to their single variants to ensure working before adding actual simd instructions
Also do simd helper functions like horizontal_add(), mask_is_zeroed() in one dimension first
Wrap the single lane helper functions and types in an if depending on the lane width set
(IMPORTANT any functions that we are to SIMD, place here. 
if it comes that we want actual scalar, then rename with func_lane prefix) 

for simd typically have to organically transition from AOS to SOA

Debug in single lane, single threaded mode (easier and debugger works)
However, can increase lane width as needed (threading not so much?)

For bitwise SIMD instructions, the compiler does not need to know how we are segmenting the register, e.g. 4x8, 8x8 etc. 
as the same result is obtained performing on the entire register at once.
So they only provide one version of it, i.e. no epi32 only si128
Naming convention have types: `__m128 (float), __m128i (integer), __m128d (double)` 
and names in functions: `epi32/si128 (integer), ps (float), pd (double)`
Overload operators on actual wide lane structs
(IMPORTANT remember to do both orders, e.g. (val / scalar) and (scalar / val))
Also have conversion functions

Lane agnostic functions go at bottom (like +=, -=, &=, most v3 functionality)
(IMPORTANT it seems we can replace logical && and || with binary for same functionality in simd)


(IMPORTANT simd does not handle unsigned conversions, may have to cut off sign bit, e.g. >> 1)

process of casting type to pointer to access individual bytes or containing elements (used in file reading too)

(IMPORTANT masks in SIMD will either be all 1's or 0's. perhaps have a specific name for this to distinguish?)

(IMPORTANT seems that not all operations are provided in SSE, like !=, so have to implement with some bitwise operations)

SIMD allows divide by zeros by default? (because nature of SIMD have to allow divide by zeroes?)


To get over the fact that C doesn't allow & floating point, reinterpret bit paradigm `*(u32 *)&a` as oppose to cast
(IMPORTANT in SIMD cast is reinterpreting bits, so the opposite of cast in C)



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
I make a note of the huge amount of cruft in the C STL.
The output buffering, hidden `malloc()` 'optimisations' (uncommitted memory, encounter expensive page faults later; prefer reliability/clarity over edge-performance benefits), 
OS line ending conversions, non-obvious use of mutexes etc.
Whilst these may seem like minor inconveniences, they can be insidious for performance, e.g.
`rand()` has a huge call-stack that if we replace with a simple xor shift, 
results in 3x speed up.
Although easy to criticise, it may be the situation that the CRT had to be that way 
because of C standards.
To isolate use of the CRT, wrap in functions so we can hopefully replace with system calls, 
intrinisics, etc.
Although generally okay to use STL, it forces you to use their patterns (e.g. memory allocation, locks etc.).
This is true for STLs in general across all languages.
Some may have bloatness from other areas, e.g. C++ templates
To avoid the compiler having to generate a large export table of all functions, 
make them `static`
To avoid large amounts of linking and ∴ increase compilation time, have a unity build. 
Furthermore, garunteed ability to inline functions (as with multiple translation units, possible one might only have function declaration and not definition)
(issues may occur with slower incremental builds when including 3rd party libraries; yet, can still work around this possibly using ccache)
