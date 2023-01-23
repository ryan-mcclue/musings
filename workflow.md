# LEETCODE EVERYDAY

IMPORTANT: premature MICROOPTIMISATION (i.e. fine-tuning things) is root of all evil.
overall optimisation should always be at forefront 

TODO: add reddit and hackernews to daily link trawl

TODO: merge 'verify' and 'plotting' into misc/ folders of repositories

# Workflow
Preparation Mentality:
## Git usage: TODO(Ryan): collate into bash aliases 
* possible git notes to append to commits: $(git notes add -m 'Acked-by: <tyler@tylercipriani.com>')
* use gcd function enter habit to know if out of date
* git commit aliases showing diff
* find parents of hash: `git rev-list --parents -n 1 <hash>` 
* locate file content: `git log -S'content' -- file_name | tr '\n' ' '`
* locate file added/deleted/modified/renamed: `git log --diff-filter=A -- foo.js`
* previous commit messages: `git show --color -s --format='%C(yellow)(%as)%C(reset) %s' -5 | tr '~' ' '`
**merge commit with previous commit**: `git rebase -i HEAD~n`
a standard rebase will apply commits on top of 
interactive rebase to perform squashing on most recent
**merge conflict resolution**: `git mergetool`

Use the approach the particular system API follows. You can always use your own approach atop

If wanting to create a new implementation of a function, 
perhaps don't just delete it entirely, rather change its name to func_old()

seems have CI for deploying or testing

Linux has vdso (virtual dynamic shared object) which implements vsyscalls, i.e. some kernel functionality, e.g. clock_gettime()

ingress (act of entering), egress used for traffic

C is good as in the beginning you always want the results. 
In the end all you want is control (writing OS instead of libc will also allow for more powerful API)
Embrace the complexity that C provides (as down the road, you will want that complexity to create varied things)
You will have the control to solve your problem
To be a good programmer, you have to want to learn how to do things
You want compiler errors and things to be explicit
C++ hides a lot of things. 
Oftentimes, the language allows you to do things that make you think your clever (intertwined with 'one-liners').
Extra typing is not bad, it makes things more explicit
Crashes make you fix things

Spacing good for searching and to avoid a+++++b (a++ + ++b)


IMPORTANT: In C, this becomes a constant expression and so can be used in a switch
Also, allow for or'ing of individual flags.
In addition, composition of flag types
typedef u32 MATCH_FLAGS;
typedef u32 S8_MATCH_FLAGS;
enum
{
  MATCH_FLAG_FIND_LAST = (1 << 1),
};
enum
{
  S8_MATCH_FLAG = (1 << 2)
};

typedef enum NODE_TYPE
{
  NODE_TYPE_LIST,
  NODE_TYPE_COUNT,
} NODE_TYPE;

typedef struct Node Node;
struct Node
{
  NODE_TYPE type;
};


```
u32 vals[2 * 3] = {
           1, 2, 3
           4, 5, 6
}
```

NAMING (mirrors a directory structure): 
```
// structs/functions/global prefixes in a file should have same name as the file they reside in
obj_create(); obj_destroy(); etc. // would go in obj.[ch]

enum MY_ENUM
{
 ME_VALUE1,
 ME_VALUE2,
};
```

ALLOCATE STYLE (MORE GENERAL, SIZEOF STYLE)
```
u32 *mem = malloc(sizeof(*mem) * 10);
```

If discriminated union size disparity an issue, consider:
```
struct SomeStruct
{
  CommonHeader *header;
  u32 value;
};
some_func(CommonHeader *header)
{
  if (header->type == SOME_TYPE)
  {
    SomeStruct value = (SomeStruct *)header;  
  }
}
```
struct padding largest value wants to be on a byte boundary that is divisible by itself
trailing members may have padding also to ensure consecutive structs in array are aligned as well
(some machines may crash on unaligned access, or raise an exception which will trigger an interrupt)
IMPORTANT: On machines with no unaligned accesses, memory arenas may create issues 

each memory page given by OS has rwx flags. 
this gives some sandboxing between process memory
(compiler warning should detect stack smashing when trying to overwrite stack areas)
(address sanitizer should pick up when trying to overwrite heap page areas)

IMPORTANT: memory access is very slow. actual math operations etc. are very fast.
e.g. SIMD perform 4 multiplies in 1 cycle, reading something from memory 50 cycles
So, when optimising, look at reducing memory accesses and see if those memory accesses are in the cache
(smaller cache faster lookups. larger cache, less liklihood of cache misses)
(IN GENERAL, COMPUTE DON'T LOOKUP)
```
struct BareBonesNoRedundancies
{
  f32 width, height;
  // f32 area;
};
Remove values that can be computed. Reduces possibility of bugs
As math is effectively 'free', we can reduce memory size also

If value is very expensive to compute, then wrap dependent values in function, e.g. set_width()
```

IMPORTANT: BUFFER ITERATIONS, USE STRIDE FOR GREATER FLEXIBILITY
```
void colour_iterate(u8 *colours, u32 count, u32 stride);
struct Something
{
  u32 id;
  u8 colour[3];
};
Something arr[10];
colour_iterate(&arr[0].colour, 10, sizeof(*Something));
```

IMPORTANT: A GOOD PROGRAMMER WANTS TO WRITE CODE. 
WHEN PEOPLE SAY TOO HARD, THIS IS AN OPPORTUNITY TO LEARN
BUILD YOUR MOUNTAIN THAT WILL NOT AGE. YOU WILL BE A EXPERT THAT CAN FIX IT, BECAUSE YOU WROTE IT
WHEN YOU WRITE A PIECE OF TECHNOLOGY DON'T VIEW IT AS STANDALONE, VIEW IT AS CREATING AN EVER HIGHER MOUNTAIN THAT WILL ALLOW YOU TO CREATE GREATER THINGS IN THE FUTURE
IN CONTRAST, AN APPLICATION SHOULD BE A SMALL HOUSE; TECHNOLOGY STACK A MOUNTAIN
LEARN BY IMPLEMENTING

Think about your code running for decades

TODO: with multithreading, be aware of cache byte line invalidation (which is an action of cache coherency)

compiler assumes will not go into undefined behaviour.
so anything that puts in UB state is considered unreachable (so x/0 is akin to `__builtin_unreachable()`)?
u16 will be promoted to i32 if it fits (bugs with comparison)
AS-IF (a.k.a equivalence) rule means a bug may appear before it has happened due to reordering

uninitialised data has an indeterminant state. so, c[0] != c[0]

struct padding is undefined, i.e. compiler's don't have to do it? 
```
memset(ptr, 0, sizeof(*ptr));
ptr->a = 10;
ptr->b = 20;
ptr->c = 30;
the memset() lets the compiler know we are okay overriding padding bytes
so can use a single 64bit write
IMPORTANT: want the compiler to always be writing largest size possible
```
with pointers, does not know if point to same memory
e.g. *array, *count. They could be the same, so can't optimise into a memset
however, it does assume no aliasing if the pointer types are different, e.g. int* and float*
unsigned char aliases all types (exception to aforementioned rule)

in C memory objects are not contiguous
so, &a == &b will be optimised away (avoid by casting to integers) 
(however a != b will work even if pointers, it's the address of that causes issues)
are C fat pointers even worth caring about? only really C++ member functions etc.

TODO: if requiring software square root, use Carmack's square root function?

strive for value orientated (less pointers), 
i.e. use structs by value; so return them and use as arguments.
removes aliasing and x86 passes small structs into registers, so never hit memory
and if optimised and inline function, goes away entirely
(is this the case for 32bit arm?)
(in C++ this in justified due to hidden copy constructors)

error handling, have the function log internal errors such as errno errors.
at the call site, just set .valid field in return struct to false
so, for a series of consecutive calls that could return errors, only check the last one
(all functions will check the .valid field)
```
file = open_file("f");
img = open_img(file);
tex = open_tex(img);
if (tex.valid) {}
```
Use stb approach for library design (might be easier to start out sparse and when shipping, merge together)
Have struct argument for large number of configuration options
Request allocators, or memory; don't allocate
In fact, don't call any OS specific calls. request hem

Avoid using most of libc as it's old, bad design (strtok global state, rand locking) and generally slow.
There are some useful, e.g. memcpy, fixed ints and math

Best to store length of string, so don't have to traverse it with strlen() and negatively affect the cache
std::string allocates on heap (in fact, a lot of C++ have implicit constructors, more so from implicit conversions)

TODO: best practices and use cases of logging

IMPORTANT: With these approaches, we feel like writing in a completely new language

C has moved ahead of no typedefs, forward declarations
C empty function argument is vararg
C has designated initialisers (gcc extension has this as well)

C like performance like vegan meat. If you want it, go for the real thing
C not literally a high level assembly language as compilers do a lot of transformations
Similarly, assembly is not a direct translation, as the CPU may reorder etc.
Compiler follows AS-IF rule, i.e. as long as EVENTUALLY equals the same. (is this only a C compiler, hence why it's fast?)
So, if not observable (e.g. volatile, io, graphics, etc.) could perform instruction reordering/removal

TODO: volatile vs atomics?
atomic means we either read all of it, or none of it (i.e. don't read if some other thread is writing to it)
architectures have specific atomic instructions?
like, a val++ is not atomic as something could be inserted in between
so doing atomic_exchange_rel(shared_pointer, a); will nothing will interfere?
(so, not enough to just say volatile as volatile will allow assignments to be reordered?)
atomics related to release and acquire?
release says everything before this point has to be done, i.e. won't be reordered upwards 
acquire means this won't be reordered downwards, i.e nothing after this will be done, before this completes
full fence is release and acquire, i.e cannot move up or down
atomics are not just instructions to compiler to not reorder, but also hardware instructions related to cache flushing etc.

C standard as at least 6 pages of undefined behaviour 
could be more, in fact anything not explicit defined is undefined.
there could be more undefined behaviour waiting to be noticed. 
it's just because C is 50 year old language that many corner cases explored

compiler assumes that code that is undefined behaviour cannot be reached and so will optimise away
wrapping is undefined for signed values, so can optimise this away in that case
`x = (x * 2) / 2;`
shifting more than 32 is undefined, so can optimise away if
`u32_val <<= x; if (x >= 32) ... `
```
switch (index)
{
  case 0: func[0];
  case 1: func[1];
}
this gets mapped to if (index) etc.
however, if put __builtin_unreachable() in default case, can remove this
so, this is useful for optimising range checks
GET INTO HABIT OF USING LIKELY AND UNREACHABLE?
```

coroutine is function that can be suspended at any point, and resume later

if consuming a list each iteration, use doubly linked list to reset (e.g. event consumption)

for any list, stack, queue etc. will wrap in a struct that has first and last pointers

although sh in containers (alpine linux), the ubiquity of bash (if git on windows, then have bash) use its features

if global, decide if actually global or thread-local

TODO: investigate analysing core dump files (when becomes necessary)

Updating compiler symlinks: $(sudo ln -vfns /usr/bin/gcc-10 /usr/bin/gcc)

TODO: breakpoint/error function print out line information only in debug builds 

Files from templates are developed on each project, thereby iteratively improving

TODO: Andrew Reece (rethinking doxygen youtube live streams)

TODO: doesn't asynchronous programming just make things harder to debug?
is performance of say linux io_uring any better than synchronous?

TODO(Ryan): Investigate useful POSIX C functions, e.g. nftw() (netfw.h), getaddrinfo() (netdb.h)
Also, some require the definition of cryptic macros to be included

TODO: aaron codes, code yao (leetcode walkthroughs)

TODO: way to reduce transparency of active window dynamically?

DIFFERENCE IN APPROACH TO WORKING ON TEAM AND STAGE OF PROJECT
on a team, 'proper' commit should change one thing and have accompanying tests.
So, write explorative code on branch and merge when done

have a _COUNT at the end of every enum

perhaps start with this:
https://github.com/TheSandvichMaker/netgame
and improve with glenn fielder?
glenn fielder networking
codersblock
javidx9 mmo
(possible informative videos: https://www.youtube.com/watch?v=jmHQa5P7hC8, https://www.youtube.com/watch?v=9OjIDko1uzc)

techlinked + michel tunnell (this week in linux)

does mesa stack include open-source drivers for NVIDIA. so, support opengl and vulkan?

sudo and pipes doesn't work.
to allow gdb to attach: $(sudo bash -c "echo 0 > /proc/sys/kernel/yama/ptrace_scope")
sudo -i; sudo vim /etc/sysctl.d/10-ptrace.conf
IMPORTANT(This seems to work): sudo /sbin/setcap CAP_SYS_PTRACE=+eip /usr/bin/gdb

TODO: GO THROUGH https://www.rfleury.com/ articles and implement!!!!
mr4thdimension (allen webster) youtube videos https://mr4th.com/  

TODO: With strncpy and such, get some indication if didn't write entire buffer, i.e. input buffer too small

TODO: variable sized data structures that use memory arena with no holes

TODO: use sanitisers from get go (may catch when mallocs less than a page)

TODO: structure code for cross-platform from get go 

TODO: terminator tab autoopen for project (include file in project source)

TODO: avoid using magic numbers for various buffer sizes

TODO: prefix discriminated union fields

TODO: write more comments on code that is 'exploratory', 
e.g. ASSUMES static size, IGNORES clearing other fields, etc.

Good error messages say what happened and why, 
provide reassurance, are empathetic, 
and help users fix the problem.

When working with array, always check if length > 0

For production, probably want to `dlopen()` to provide fallbacks for missing libraries 

TODO: some sort of code review on merge requests

TODO: writing expansive tests (perhaps look at zeux github?)

always in software development will have things that aren't working optimally. know they are there.
will later go back and fix them when required (e.g. when in quality improving mode). just know this will always happen

Working with modern software is going to be a pain at some point.
Have to stay strong and not belittle situation, rather accept it and move on.
At times, have to get off high horse and roll up your sleeves.

Linux issues: 1. runtime configurability (difficult part not API, but what to do when settings aren't exact, i.e. what to fall back on when abstraction not there). 2. multiple binaries
Although possible to dynamically load core libraries like xlib/wayland/alsa and support
a minimum set of APIs, this is essentially what SDL does.
However, system/user configurability is huge in linux. There is simply too much
runtime variability to test for all users.
Furthermore, multiple CRT to support, e.g. muslc for Alpine

One way to solve variability is to use the Steam Runtime, which is a collection of .so files and configuration that is less subject to user configuration

Linux desktop suffers from binary compatibility nightmare, e.g. 15 binaries to support all. 
This is because libraries like glibc are happy to break abi to get minor improvements, e.g spec says this so let's do it even though no one cares

C STL has one version for each compiler + os. 
also, some compilers might not fully implement the STL feature

With an undeclared identifier in a unity build, just reorder #includes in main file

Cross platform naming, e.g. With libc,  libraries is more of a discipline thing 

There are no absolutes, just tradeoffs for solving your problem, e.g raw vs smart pointers

Often issue is not hard code, it's because the problem is messy, i.e.  Unwritten assumptions,  
external constraints, users want lots of generic functionality. 
So often interesting at the same time, very annoying. 

Frameworks change all the time,  fundamentals not so much, e.g how things glue together

compression orientated program think about concept compression, rather than code compression.
e.g. might be at such a small code size that there is no concept to really compress

combinations: 2ⁿ-1, max: 2ⁿ
introducing negatives we effectively remove one bit
numbers represented in twos complement to handle addition/subtraction of negatives and positives.
not because of representation, e.g. sign bit would work fine
when saying complement, it's with respect to negative number handling:
ones ➞ invert all bits in positive number to get negative. therefore have -0
twos ➞ adding a positive and its negative will get a 2 in each place. think about the MSB as the negative place, hence why -1 is all 1s

FPGA (matrix of CLBs) reprogrammable (as oppose to ASIC) IC with HDL.
Better for parallel processing?

dave's garage, what's a creel

Switching between laptop and desktop as uni student meant merge conflicts were common.
So, know how to fix with $(git mergetool) and have it as a rebase

Github issue and pull request template files (https://embeddedartistry.com/blog/2017/08/04/a-github-pull-request-template-for-your-projects/?mc_cid=a5a0d2dbbf&mc_eid=UNIQID)

Terminal + Editor + Window manager

Important to have linux source so as to view Documentation folder
$(make kernelversion)
https://www.youtube.com/watch?v=Jl_0W4o0xao

(Serial Advanced Technology Attachment)
important that these are sustained speeds, so for small file sizes expect a lot less as seconds smaller
note that storage space advertised with S.I units, whilst OS works with binary

RAM potentially hundreds of cycles (CAS latency and cache retrieval process)
(important to note that RAM CAS is only say 20% of total latency as will 
check traverse cache and than copy it to cache)
https://sites.utexas.edu/jdm4372/2011/03/10/memory-latency-components/
In general:
check if in L1. If not go check in L2 and mark least recently accessed L1 for
move to L2. bubbles up to L3 until need for memory access which will go to memory
controller etc.

so, ram frequency gives max. throughput
however latency of ram also important

(cache latency from wikichip)

generic is referencing Ubuntu specific kernel compilation (could also have -lowlatency etc.) 
(generic does not include a lot of modules in kernel to alleviate RAM, so use modprobe)
(-41 is build number, i.e how many times compiled)
(the compiler, linker, libc are all specific to linux OS used, e.g. compiled with different settings, different versions, etc.)

$(grub-install -V)
/etc/default/grub (GRUB_CMDLINE_LINUX_DEFAULT=""); $(update-grub); $(cat /proc/cmdline)
(acpi_enforce_resources=lax boot parameter to allow decode-dimms to find ram chip)
Tentative to make boot changes, e.g. kernel parameters due to previous issues whereby
performing a grub install on a loopback device affected the system's existing EFI GUID.
To alleviate these concerns, carry copy of ubuntu on USB to mount and backup regularly

-----
Don't restrict right side of bell curve
Let your aces be aces
Being an ace involves having an opinion
Most influential software written largely by one person, e.g Linux, Unix, git etc. Then a team is assigned to maintain it. Fallacy about solo programmer productivity requiring large teams.
Design by committee pushes design to middle of bell curve as opposing views average out

Cpu try to guess what instructions ahead (preemptive). 
Cost of incorrect reflushing expensive. 
So want to get rid of conditional jumps. 
Ideally replace with conditional movs or arithmetic branch less techniques.
Endianness (register view), twos complement (-1 all 1s)
Branch less programming is essentially SIMD

What simd instructions available to cpu?
If variable clock speed, cpu could detect not using all cores and increase single core clock

Flops calculated with best instruction set?
Not memory bound is best case for hyper threading
Intel speeds optimised for gpr arithmetic, boolean and flops
Why is Intel shr instruction so slow?
Intel deliberately makes mmx slow
-----

Polymorphism is a single object that can be interpreted as having various types. This can simply be a struct with unions and a type field.

Never use setters/getters unless actually doing something. You're spending your entire day typing. If needing, replace variable name with name_ to see where it was used.

You need to be self critical to be a good engineer

Caches are a way of minimising roundtrip time of RAM by putting memory as close to the core that thinks will need it
L1 closest, 1/2 cycles, 32k. Wikichip for more info
Cpu will go to caches first

L1 can supply 2 cache lines per clock
Instructions per clock, number of work components, e.g. number of add components, cache line per cycles, cache latency, agu (address resolver units) impose restrictions
A cache miss is simply stalling for an instruction. 
However, this may not be an issue if we do other work, e.g. complex algorithm takes many instructions hiding memory access for out of order cpu. 
If hyper threading with two schedulers, if cache miss on one, just switch to the other.
Can really only know if a cache miss incurs a performance penalty by looking at raw numbers from vtune, etc. Because of the scheduler, it's not as simple as just looking at memory sizes
So, due to complex overlapped/scheduling nature of modern CPUs can really only know if cache miss incurs penalty with vtune
Uop website displays table for instructions

Currently good that most things are little endian with 64 byte cAche lines, however some hardware guys Is going to come along and change it back to big endian

Should be code of ethics in software to not create bugs/inconveniences for users that couldve been avoided

An instruction of throughput 1 means issued every clock. As many instructions take longer than 1 cycle, each core requires a scheduler to see if it can execute something.
View cpu as sections where there is some distance to communicate.

Making some thing good takes time. However, if you have crazy design practices it will also take longer
You have to be reality based when programming. That is in an engineering sense, to design something that solves the problem you have
People become attached to a way of programming which doesn't focus on solving the problem. They want to build rube-goldberg machines
Selectively attacking problems seriously means you have a functional program quicker, whereby you can actually decide if those other problems need to be addressed. 
Can defer hard decisions later as they will be made better as you will have more technical expertise and more context to work with

Testing is important. If you don't write tests, your software doesn't work. However, write higher level system tests, not excessive unit tests. 
More efficient and this is where bugs are likely to be. You often have to remove code, so having unit tests just increases the volume of code you have to write. 
Huge drain on productivity. Maybe for NASA. A new paradigm should weigh up cost-benefit. Almost always the cost is ignored and people gobble them up

To make computers better to use, have to simplify them on all levels

Faster cpu like Apple's m1 are irrelevant if software bad

Floating point math faster than integers

my style of programming and problems enjoy solving found in embedded, e.g your constrained with the silicon not like in web where you just build another data centre

Compiler works on file by file, so knows nothing about calls across files. 
Therefore it generates object files which are partially executable machine code with unresolved symbols. 
Linker merges these object files and also adds extra header information so that the OS can load our executable (or more specifically a kernel, e.g. linux)

Complete code coverage on the one hand is very thorough, however don't get a lot of engineering output. 
Furthermore, most bugs appear in between systems not in units. 

Best way to test is to release on early access.
This checks hardware and software, user may be running adobe acrobat which hogs cpu so instruct them to kill it before running your game. 
Or maybe 20000 chrome plugins. This is something a hardware lab can't tell you

Process is allocated virtual memory space
OS has mapping table that converts these to physical addresses. 
Part of our processes address space is pre-populated by the kernel program loader, e.g. 
linux-vdso, environment variables, etc.
Kernel tunables: sudo sh -c 'echo kernel.perf_event_paranoid=0 >> /etc/sysctl.d/local.conf' (sysctl)
User tunables: ulimit -a
In virtual address space, have user space and kernel space address ranges. 
A virtual address is mostly relating to page table indexes and the last bit is a static offset (as for security addresses are randomized)

To make an installer just fwrite your executable and then data files appended with footer. 
Inside the exe, fread the exe and fseek based on the static offset of the appended resources
Bake resources in for reliability only really

Packed files better as less OS operations performing expensive file handles etc.

Programming about solving problem. overlooked by design philosophies. If you don't have any functionality, you don't have a structural problem

const is only useful if you find it catches bugs for you (maybe for globals instead of using #defines)
however, in terms of optimsations, const is useless as you can cast const away.
therefore, for me, const is mostly just a waste of typing.
however, have to use for strings in C++
In a similar vein, VLAs useful here (note that sizeof(array) and sizeof(pointer) for calculating string array count)

distinct areas of memory in assembly are stack, heap and data (globals)

direction of stack growth is often determined by CPU. if selectable, then OS. eg. x86 is downwards
ulimit -s for stack size (main executable will have stack size listed in headers)

good practice to assign variable for syntatical reasons, i.e. more readable, e.g
Controller *controller = &evdev_input_device.controller;

getconf can list POSIX system configurable variables

don't go through a ton of unecessary stdlib. 
malloc probably more optimised for small memory size requests
relative to overall code base, interfacing with system calls is not that much code (and can be reused)

if we try to access memory from an invalid page (not reserved or comitted) will get segfault.
only have to worry about errors that don't manifest themselves on every run of the program.

on x86, writing by 32bit faster than 8bit as less instructions
in general, the fastest way is to use the widest possible register that can be operated on at once
the speed of accessing the memory from cache is pretty cheap for nearby regions

don't make changes for conceptual cleanliness. end of the day, want to make performant, bug free code
in the shortest amount of time.

when programming some days you are off. this just means you're going to be debugging a lot

we want it to be clear what our code can and cannot touch. global variables make this hard (however, can add _ to see where they are all used)
however, as many OSs are rather janky and most code will live outside this, it is ok to have some globals here
globals are fine in development. can repackage into a structure later.

clock speed not as relevent as improvements in microarchitecture and number of cores means can be more efficient under less duress.
also, lower clock speed may be because want to draw less power.

short build times (under 10 seconds) are incredibly important to not decentivness making changes and testing them

function overloading, generalised operator overloading and default arguments are c++ features that can't easily be implemented with gcc extensions

note that >> will typically (implementation defined) perform arithmetic shift (fill in with 1's) on signed, so not always the same as a divide.
similarly, sign-extension just fills in the new MSBs with 1's

for large cross-platform projects, best to differentiate with filenames, rather than ifdefs.
this also gives the ability to have different control flows across the different platforms (essential)

for a game, better to have the game as a service to the OS (not the other way round).
this is becuase the game does not need to know/perform the myriad of possible operations the OS can perform.

most modern cpus have a floating point unit, making them faster than ints (same latency), 
e.g. a multiply is one instruction where ints is two (multiply and shift)
x87 is the FPU instructions for x86 (also have SSE instructions which is want you ultimately want)
however, for multiplayer games, optimisers can give different results when using floating point,
e.g a platform that has operator fusion like a MULADD may give different result when rounding then a 
platform that has do it separate. (fixed point could solve this)

Programming Mentality:
always important to know when coding what is your goal. premature optimisation and design are bad.
your goal dictactes the quality of the code you write, e.g. allowed to be janky as first pass on API.
write usage code first.

often when having a variable lengthed array, ask ourselves do we really need that?

asserts are part of debug program that are used to check that things work that should always work.
use them for a condition that must be true that is not explicitly present

don't think about memory management, but rather memory usage. if having to worry about freeing etc. done something wrong.

when writing 'spec' code, no need to handle all cases; 
simply note down the edge cases you should handle later on
stop yourself thinking about whether the code is messy or not. 
only care once problem is solved!

it's not the programming practice but the dogma that gets you. when you start to name things it almost
always becomes bad. almost all programming practices have a place, just not used often
so RAII people, in case of things that must be released, e.g DeviceContext, ok to use a constructor/destructor
pair. I'll throw you a bone there

streaming i/o is almost never a good choice (hard drive slowest, more errors)

compression orientated programming is you code what you need at the time (breaking out into function, combining into struct, etc.) 
over time the code marches towards a better overall quality

amdahls law gives the time taken for execution given a number of cogives the time taken for execution given a number of cores.
for this formula (indeed any formula) we can obtain some property by seeing as function parameter approaches infinity. in this case, the parallelising part drops out.
brooks law says that simply adding more people to a problem does not necessarily make it faster. if requires great deal of coordination/communication actually slows down.

solving a problem: 1. decide what you are doing (this can't be open-ended.) 2. organise groups to achieve this
by making these boundaries, we are presupposing that each part is separate, e.g tyres team and engine team; assume tyres and engine cannot be one piece.
therefore, the boundaries define what products you can make, i.e. you produce products that are copies of yourself or how you are structured
so, in software if we assign teams for say audio, 2d, 3d we would expect individual APIs for each.
the org chart is the asymptote, so it's the best case that we make a product as granular as our org chart. it could be far worse and even more granular 
therefore, communication between teams is more costly than communication within teams.
takeaway is that low-cost things can be optimised, high-cost can't be (further away on the org chart)
note that communication in code could just be someone checking something in and you pulling it
what we are seeing now with modern software is the superposition of orgcharts due to use of legacy codebases
now we see org charts in software, where people are artifically creating inheritence hierarcies that limit how the program works
this is very bad. the reason it's done is for people to create mental models that help them solve the problem as they can't keep the complexity in their head. 
it may be necesary to solve the problem, however it shouldn't be looked at as good.
however, because it's done due to lack of understanding, the delegation/separation is not done with enough information. so you limit possibilities of the design space.
so although, libraries, microservices, encapsulation, package managers, engines may be necessary due to our brain capacity 
(until neuralink or we figure out a better way to do them) they are not good! 
we may use hash map, but only in a particular way
They limit optimisation as we have already decided separation
so always be on the lookout for times when you don't have to do these
most people just download hundreds of libraries because they know it works and they won't be worse than any one else.
WE MUST BE LEAN AND FLEXIBLE IN ORGCHARTS IN COMPANY AND IN SOFTWARE TO INCREASE DESIGN.
some old codebases need to be retired


DO THE 'MOST CERTAIN' THING FIRST. THIS COULD EITHER BE THE IMPLEMENTATION OR THE USAGE CODE
choose data structures around solving problem

some software is scaffolding, i.e. not shipped with the final product, e.g. editor for games

To make anything alternate over time, just multiply by sine(time);

Data hiding hides what the CPU is doing, which is what we care about

Require machine-specific documentation files to understand system we are on
System specific ctags template projects, e.g. linux kernel, glibc, etc.
If using library, have ctags for that project

ALMOST ALWAYS CAST TO FLOAT WHEN DOING DIVISIONS LEADING TO FLOAT

Minimum value starts at max

Spreading out randomness: `final_value += contrib * sample`

If debug code (or code that will not be in release) use compile-time macros

Use GLOBAL and global_prefix 
If casting is occuring, always be explicit about it!
Prefixing functions with sdl2_func() or linux_func() only if intending to be cross-platform

With error handling, bad practice is to allow a lot of errors, which brings in error classes etc.
Instead, if it's something that is actually an error, e.g. missing file, write the code to explicitly handle it.
Handling the error in a sense makes it no longer an error, rather a feature of the program

if function is expecting a range between, should we clamp to it?

Refactoring Mentality:
Refactoring is essential. You must know what you are trying to achieve so you have some notion of progress, e.g. adding a constraint to the system. 
(Replacing variable type in scope gives us all locations where variable was used, including macros. This us where dynamic lnguages fail as you don't know if you broke something) 

You can abstract/encapsulate anything at anytime, so why not do it later when you know what you are doing?
We want file format to be simple and binary, unlike json which is general purpose and string

modifying (parse as pointer arg), returning (result struct).
it is best to put simple types are arguments rather than a group struct to allow for maximum code reuse.
only put group struct as arguments if must be put together
(in general don't pack so don't force user to create a struct when they may already have the values)

if can go functional without sacrificing, saves you complexity down the road.
oftentimes simply utilising elements of functional programming is good, e.g. no global state, only operating on parameters etc.

writing code guides you to the right design, e.g. made same call -> put into function; 
require args in function -> struct;
many related functions together -> organise related functions into own file; (if thinking could be moved to another file, make comment sections outlining code blocks to ease this process later on)
etc. (these are simple, compression changes), 
complex api -> transient struct, overload functions for internal/external use
There are also large changes that are more difficult, e.g. 
sections of single value interpreted differently -> pack 2 variables into 1 (_ technique useful here)
same operations performed on pairs of variables -> vectors (as oppose to working with them as scalars)
vectors are particularly useful as without them repeated actions quickly become intractable
It can get messy at times, but always know that a clean solution is out there and you will refine towards it
Work threw the error tree one at a time
Important to throw in asserts for underlying assumptions.
Also for debugging be aware of 'copy-pasta', e.g. copying variables will have same name for two parameters as didn't replace it

we don't want to orient our code around objects (if anything, algorithmic oriented). 
its about how you arrive at some code that determines how good it is

excessive pre-planning and setting waypoints is really only useful if you've done the problem before
(which in that case you're not really designing)
instead, we become a good pathfinder, learning to recognise the gradient of each day of the journey.
when write the simplest thing and loft it out into good design later 
(in this explorative phase, if we make an changes for efficiency
reasons we have just introduced the possibilities of bugs with no benefit)

only break into function when you know what you want, 
e.g. called multiple times or 
code finalised and improved readability 
(in which case a tradeoff is made between understanding functionality and semantics)

Don't be scared of mass name changing!! Before doing so, see all places where name is used
Don't be scared of long list of compiler errors. Work your way through them

Refactoring with usage code: just write out structures that satisfy the usage code.
If major rewrite use #if 0 #endif to allow for successful compiling

refactoring just copy code into function that gets it to compile.
later, worry about passing information in as a parameter
basic debug and release compiler flags
When refactoring, utilise our vimrc <C-F> all files
Also, just pull code out into desired function and let the compiler errors guide you
Returning multiple values, just return struct
To reduce large number of function parameters, put into struct


Debugging Mentality:
debugging stepping through pass-by-pass. 
inspecting all variables and parameters and verifying state of particular ones.
make deductions about state of variables, e.g overflowed, uninitialised, etc.
drop in asserts
draw it out

being able to draw out debug information is very useful. 
time spent visualising is never wasted (in debugger expressions also)

When debugging, look through variables and see if anything looks ridiculous

When in debugger, go iteratively progress through variable values in function and see if they look right
We can isolate some area of the code and say this is probably the problem
Then investigate relevent sub-functions, etc.
This can be a long process with seemingly little gains.
The issue could be subtle, e.g. signed/unsignedness size, function called rarely


Configuration files should be copied, not generated (becomes too messy)
Symlink to template files from projects

To begin, I ensured that I had a debugger 
from which I could easily step through the application's execution.
In code, I was able to programmatically set system and user breakpoints.

(mocking of syscalls for unit testing with file i/o)

For handling non-fatal errors, single line check. 
For fatal errors, nest all preceding code 
(I have learnt to not be afraid of indentation in this manner).
(error handling in general, i.e. reduce 'errors' by making them part of normal execution flow)

When performing the common task of grouping data, a few practices to keep in mind.
Use fixed sized types to always know about struct padding 
(in fact, I like to extend this to all my code)

If wanting multiple ways of accessing grouped data, use union and anonymous structs.
Use an int to reference other structs, e.g. `plane_index` 

If the data being grouped can only exist together (e.g. points), use vectors.
Put all structs related typedefs inside their own header file for easy access.

As floats are an approximation, when comparing to 0.0f (say for a denominator check) 
or negative (say for a square root) use a tolerance/epsilon less-than/greater-than check.
In fact whenever dividing should always ask oneself "can the value be zero?"
To be clear about float to int casting, use a macro like truncate/round (think about what if uneven divide)
Due to mixed integer and float arithmetic going to float, calculate integer percentages `val * 100 / total`
There is no need to overload the division operator as can do `(* 1.0f / val)`

For easy substitution, use single letter prefix names like `output_h` and `output_w`.
Convention for variable arrays, e.g. `Planes plane[1]`, `planes` and `plane_count (use ARRAY_COUNT macro here)` 
Put for loop statements on separate line to help not be afraid of indentation.
Iterate over pixel space and then convert to say, world space for calculations (normalisation and lerp)
Aspect ratio correction is simply rearranging a ratio. 
If we determine one side is larger, scale other.
Use "\r" ASCII code to print a status indicator.
Only use const for char * string literals stored in the data segment.

Endianness comes into play when reading/writing from disk (e.g. file type magic value) and working directly with `u8 *` (e.g. iterating through bytes of a u32) 


MARKETING APP:
f5bot.com, https://github.com/lawxls/HackerNews-personalized
I'm notified when keywords related to "human wants thing, my app can do thing" appear on HN, Reddit and Lobsters.
If I can then contribute with information to that discussion, I'll also leave a link to my app.
Don't just self plug, people (myself included) appreciate more detailed information on how they can solve their own personal problem, 
instead of being thrown into "here's an app, figure it out" 

even parity is to make it even, i.e. so if 5 1's, even parity will add a 1 
