<!-- SPDX-License-Identifier: zlib-acknowledgement -->

set -e (this is important so that ctime incomplete timings are given)
ctime -begin tra.ctm
code here
ctime -end tra.ctm

discovering for linux docs are code and for development may have to add to groups (uinput) and/or modify policies (bluez)
-------------------------------------------------------------------
for long range, LoRa or sigfox
essentially tradeoffs between power and data rate
ieee 802.11 group for WANs (wifi - high data rate), 802.15 for WPANs; 802.15.1 (bluetooth - le variant - heavily used in audio), 802.15.4 low data rate (zigbee implements this standard)
-------------------------------------------------------------------

# Modern Software is Slow
People think that it's slow, but it won't crash (because of interpreter)
Performance is critical in getting people excited about what you do, e.g. windows ce was laggy, iphone performant but less features changes the market 
low latency is more desirable, even if less features
editors for games like building scaffolding; not shipped with game
noise is randomness. white noise is complete randomness. blue noise (harder to generate) is randomness with limitations on how close together points can be (more uniform)
not really optimisation, just code change to generate a better distribution
receptors on retina are arranged in blue noise, so the eye as to less anti-aliasing

count number of math ops in function and control flow logic that is evaluated
branches can get problematic if they're unpredictable

inspecting measurements of microarchitecture consider latency and throughtput (how much does it cost to start again)
so, FMUL may have latency of 11, however throughput of 0.5, so can start 2 every cycle, i.e. issue again (cpus these days are incredibly overlapped)
so throughput is more of what we care about for sustained execution, e.g. in a loop

however, these numbers are all assuming the data is in the chip.
it's just as important to see how long it takes to get data to the chip.
look at cache parameters for microarchitecture; how many cycles to get from l1 cache, l2 cache, etc. when get to main memory potentially hundreds of cycles
bandwidth of L1 cache say is 80 bytes/cycle, so can get 20 floats per cycle (however, based on size of L1 cache, not really sustainable for large data)

(could just shortcut this and just see if flops is recorded for our chip)

so, using these rough numbers we should be able to look at an algorithm (and dissect what operations it's performing, like FMULs), know how much data it's taking,
and give a rough estimate as to how long it could optimally take (will never hit optimal however)
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
O(n·m) is multiplicative, not linear O(n). big oh is just indication of how it scales. could be less given some input threshold
now, once code reduced, look at minimising number of ops 

cpu front end is figuring out what work it has to do, i.e. instruction decoding

e.g simd struct is 288 bytes, 4.5 cache lines, able to store 8 triangles

understanding assembly language essential in understanding why the code might not be performing well

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

microop fusion is where a microop doesn't count for your penalty as it's fused with another. with combined memory ops, e.g. `vsubps ymm8, ymm3, ymmword ptr [rdx]` this is the case 
so, if a compiler were to separate this out into a mov and then a sub, not only does this put unecessary strain on the front-end decoder it also removes microop fusion as they are now separate microops
(important to point out that I'm not the world best optimiser, or the worlds best optimisers assistant, so perhaps best not to outrightly say bad codegen, just say makes nervous)

godbolt.org good for comparing compiler outputs and possible detecting a spurious load etc.

macrop fusion is where you have an instruction that the front-end will handle for you, e.g. add and a jne will merge to addjne which will just send the 1 microp of add through 

uica.uops.info gives percentage of time instruction was on a port (this is useful for determining bottle-necks, e.g. series of instructions all require port 1 and 2, so cannot paralleise easily)
so, although best case say is issue instruction every 4 cycles, this bottleneck will give higher throughput

# Optimisation
1. Optimisation (this is rarely done due to time involved)
Do back-of-envelope calculations; look at algorithm to see if wasteful; benchmark to see if hitting theoretical calculations and then use timings etc. to isolate why our code isn't hitting these theoretical numbers
Very importantly need to know what the theoretical maximum is, which will be dependent on the program, e.g bounded by number of bytes sent to graphics card, kernel stdout etc. 
2. Non-pessimisation (do this all the time)
Don't introduce unecessary work (interpreter; complex libraries; constructs like polymorphism/classes people have convinced themselves are necessary) for the CPU to do to solve the problem
3. Fake optimisation (very bad philosophy)
Repeating context-specific statements e.g. use memcpy as it's optimised (however the speed of something is so context specific, so non-statement), arrays are faster than linked lists (again, so dependent on what your usage patterns are)

When many people say too much effort involved in optimisation, they are generally thinking of point 1

sometimes things should be running faster than they should, however only so much we can replace
e.g. the structure of many OSs are based on legacy code, so simply outputting to stdout may go through shell then kernel etc.
also, may have to deal with pessimised libraries. in these cases:
* isolate bad code, i.e. draw hard boundary between your code and theirs by caching calls to them
* do as little modification to the data coming in from them (no need to say put in a string class etc.)

(intel gives us 64 bit index to memory, so effectively give us circular buffers for free?)

# Software Architecture
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
so although, libraries, microservices, encapsulation, package managers, engines may be necessary due to our brain capacity (until neuralink or we figure out a better way to do them) they are not good! They limit optimisation as we have already decided separation
so always be on the lookout for times when you don't have to do these
most people just download hundreds of libraries because they know it works and they won't be worse than any one else.
WE MUST BE LEAN AND FLEXIBLE IN ORGCHARTS IN COMPANY AND IN SOFTWARE TO INCREASE DESIGN.
some old codebases need to be retired


# Modern Program Woes
STM32CubeMX (this justs generates code, IDE is full fledged) to download, must get link with email
Once installed, a series of pop-up menus just keep appearing spontaneously as it has to download more to satisfy a simple create project
To download qtcreator well known bug that it selects the wrong mirror, 3MBsp to 30kbs. 
Have to decipher command line arguments and mirror parsing, e.g. ./qt-unified-linux-x64-4.3.0-1-online.run --mirror http://ftp.jaist.ac.jp/pub/qtproject  

# Debugger Woes
Disable optimisation to prevent lines being removed which the debugger won't pick up on.
mention QEMU simulator debugging issues

# Embedded Workflow
Attainment of documentation
The CPU architecture will have an exception (a cpu interrupt) model. Here, reset behaviour will be defined.
the 32 bit arm cortex-m4 has FPU (a application, m for microcontroller, r high performance real time)
TODO(Ryan): avr vs arm vs rsic-v vs x86 vs powerpc vs sparc vs mips
(what motivations brought about these architectures?)
as often harvard archicture (why?)
Von Neumann, RAM (variables, data, stack) + ROM (code/constants) + I/O all on same CPU bus.
Harvard has ICode bus for ROM, and a SystemBus for RAM + I/O. 
This allows operations to occur simultaneously. So, why use Von Neumann?
it is labelled as an evaluation board
different boards use different ICDI (in-circuit debug interfaces) to flash through SWD via usb-b
e.g. texas instruments use stellaris, stm32 ST-link
is fixed point used anymore?
TODO(Ryan): Why is a floating pin also called high impedance?
To avoid power dissipation and unknown state, 
drive with external source, e.g. ground or voltage.
Pull-up resistor connected to voltage, pull-down to ground.

^^^^^^^^ Embedded
-------------------------------------------------------------------------------------------------------------------------------------

# Encounters
so niche and difficult it is neck beard inducing, i.e. lonely person

gravatar requires more memory than retro games on the SNES etc.

gpus have introduced a whole host of undocumented NDA annoyances

# Coding Mentality
when writing 'spec' code, no need to handle all cases; 
simply note down the edge cases you should handle later on
stop yourself thinking about whether the code is messy or not. 
only care once problem is solved!

only break into function when you know what you want, 
e.g. called multiple times or 
code finalised and improved readability 
(in which case a tradeoff is made between understanding functionality and semantics)

templates add complexity in the debugger (no actual names). 
only really useful if you save a ton of code (not much code to just write each implementation)
if templates are necessary then just use meta-programming as it is much more powerful.

short build times (under 10 seconds) are incredibly important to not decentivness making changes and testing them

don't think about memory management, but rather memory usage. if having to worry about freeing etc. done something wrong.

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

# Optimisations
optimise for worst case (looking out on whole world) 
not best case (in front of wall, don't render what is behind). 
we care about highest framerate, not lowest.

virtually never use lookup tables as ram memory is often 100x slower (so unless you can't compute in 100 of instructions)


focus on craftmanship, knowledge of hardware, low-level, what to focus on is what is 
important rather than what editor/language using etc.

being able to draw out debug information is very useful. time spent visualising is never wasted (in debugger expressions also)

immediate mode and retained mode are about lifetimes. for immediate, the caller does not need to know about the lifetime
of an object.

another design boundary is resource acquisition. could be 'loading screen' (when game stops and simply loads resources)
or streaming (more attainable with modern hardware)

input and update/render (update and render together for cache coherency. input is not performance critical so don't bother)
render is really 'render prep' as it is just filling in buffers

With UML if doing sufficiently complex to the same level as a blueprint, may as well have just written the program
the idea of iterative design would be followed by literal architects, however too costly and time consuming.
with software, we have the ability to do this.
1. design (urban planner): separation of code (mental clarity and division of labour)
design metrics are temporal coupling (physics outputs data to renderer, so format is important), 
layout coupling (renderer inherits from opengl), idealogical coupling (threading, width, memory), fluidity (one change to system causes a major crash)
2. programming (architect)
3. compiler (builder)

Most design patterns are just utility classes rather than a way to architect a program

I don't want to fight the language (Java). Higher level languages should allow you to easily express cpu instructions

as the working directory can change, better to use absolute file paths from the binary directory

reject the idea of TDD driving good design. accept that tests validate design.

code shouldn't take longer than 10 seconds to compile.

in general we don't add security threats that weren't already present, e.g. loading from shared object could just as easily override binary if have write priveleges to both

we can create a psuedo vtable in C and not have it be C++ and all weird. we can see it and do what we want with it.

tdd and bdd have good elements in them, but the dogma is not effective.

compression orientated programming is you code what you need at the time (breaking out into function, combining into struct, etc.) 
over time the code marches towards a better overall quality

debugging stepping through pass-by-pass. 
inspecting all variables and parameters and verifying state of particular ones.
make deductions about state of variables, e.g overflowed, uninitialised, etc.
drop in asserts
draw it out

unlike windows msdn, linux documentation is mostly source code (not good as if not fast/easy, not used). so, essential to have something like
ctags and compile from source (sed -e "s/-Werror//g" -i *.make)

functional programming although more readable is very restrictive (you can't use many of the cpu features)
however, if can go functional without sacrificing, saves you complexity down the road.
oftentimes simply utilising elements of functional programming is good, e.g. no global state, only operating on parameters etc.

streaming i/o is almost never a good choice (hard drive slowest, more errors)

we see contiguous memory in virtual memory, however in physical memory it is almost certainly going to be fragmented

asserts are part of debug program that are used to check that things work that should always work.
use them for a condition that must be true that is not explicitly present

many programs treat memory as an infinite resource. allocating memory is introducing a failure case and making
another trip to the platform layer. we just want one input stream and one output stream.
not a fan of allocation festivals.
we can create our application with minimal failure cases (cannot do this with the platform layer).

many javascript/python people will just willy-nilly do .slice() which actually copies the array, O(n)
amortize is reduce over a period of time; so must consider start and end point. (for array appending, converges to 2n)

unless playing at EVO with some maxed out razer device, not feasible to hit that hard

often when having a variable lengthed array, ask ourselves do we really need that?

always important to know when coding what is your goal. premature optimisation and design are bad.
your goal dictactes the quality of the code you write, e.g. allowed to be janky as first pass on API.
write usage code first.

mostly use c but have access to cpp features when we want them, e.g. function overloading (same name, different signature)

even though a 'new' language won't crash, it can still have buffer overruns

note that >> will perform arithmetic shift, so not the same as a divide.

most modern cpus have a floating point unit, making them faster than ints (same latency), e.g. a multiply is one instruction where ints is two (multiply and shift)
x87 is the FPU instructions for x86 (also have SSE instructions which is want you ultimately want)
however, for multiplayer games, optimisers can give different results when using floating point,
e.g a platform that has operator fusion like a MULADD may give different result when rounding then a 
platform that has do it separate. (fixed point could solve this)

sound output varies significantly across different hardware. so, to detect possible sound bugs we may need
to invest in a good piece of sound hardware

in linux, x11 is mixer for drm, pulse a mixer for alsa (more flexibility with pulse, 
e.g. can disable it if only needing one application at time, or can redirect it to another sound card)
audio is a mess in linux

uml and diagrams in general are a waste of time (its just code you would write and often fails to capture subtleties)
you should become more proficient at reading code and understanding its relationship.

for large cross-platform projects, best to differentiate with filenames, rather than ifdefs.
this also gives the ability to have different control flows across the different platforms (essential)

for a game, better to have the game as a service to the OS (not the other way round).
this is becuase the game does not need to know/perform the myriad of possible operations the OS can perform.

due to disconnected nature of x11 and scheduling, must vsync. sleeping isn't an option due default to round-robin
scheduling policy (switching to real-time scheduling is not suitable for general-os).
we could improve by setting niceness (however must be sudo)
as not real time, we can't control a lot of things, e.g. could be USB latency, adobe photoshop in background, etc.
on say raspberry pi could program GPIO pins to have a accurate poll for a joystick. All we can do is not make
things work.

sound on linux is absurd (tribal knowledge). even the people on the mailing list don't know how it works.
constantly saying, don't do this, use a library...
often times, source code is the documentation, so good luck spending months on how to operate low level.
return when threading is involved.

compilers can auto-vectorize loops for us (and other operations if we perform say 20 of them).
so, floats will be twice as fast as doubles (more space, even though same latency and throughput)

important to know hardware as if we don't have 64bit registers, may have to emulate them which is expensive

with pure compiler optimisations, i.e. code we have not optimised ourselves, a 2x increase is not unexpected.
(code we optimised not as much)

oh no, we had a security bug in our development version! (printf and friends. printf %f defaults to double)

when programming some days you are off. this just means you're going to be debugging a lot

c++ struct functions implicitly have a this pointer.
virtual functions will result in a vtable (array of function pointers) to be generated for that struct.
therefore, virtual function will first go to vtable, then lookup in function, so double indirection (so not a zero-cost abstraction)
normal function call just call 0x1234, however with vtable mov rax, qword ptr [rsp + 20] etc. (dereferencing pointers)

an engine makes things that aren't likely to be difficult easy.
important to know low-level to write new tools. we don't have a wheel yet.

optimising is a very precise process. only do when you have code that is working and you know will keep.
games by their very nature are about responsiveness, so optimisation and low-latency is important.
I like programming with this as a mentality.

we want it to be clear what our code can and cannot touch. global variables make this hard (however, can add _ to see where they are all used)
however, as many OSs are rather janky and most code will live outside this, it is ok to have some globals here
globals are fine in development. can repackage into a structure later.

clock speed not as relevent as improvements in microarchitecture and number of cores means can be more efficient under less duress.
also, lower clock speed may be because want to draw less power.

we want our code to be real (to hardware) so that it can be robust and efficient; so no scripting language
(many applications like MS Word have built-in scripting languages, which means that they can contain malware)
for any non-trival task, scripting languages become a hindrance with no static type checking, no real debugger, slow, not as capable.
there are complications in software we have wrongly convinced ourselves are necessary, e.g. scripting for hotloading
hotloading C is far superior, as C is more powerful and can use same debugger (using Lua is a downgrade)
build systems can be useful as they allow for incremental builds (however, in negatively reinforces people to only make small changes)
(speed increase may only be noticeable for large, complex code base)
they are also useful for managing cross compilation (libraries have to be pulled in and compile also)
the idea of incorporating a scripting language into a game was a failed experiment in 2005s.
things like a visual based interface is fine as it is constrained

power of whitespace in shell! IFS and quoting!

x11 is more complicated than it needs to be. like many OS commands, it should do a lot of the dirty work for you, as no-one knows all the minutiae.
it's bad to think that OSs are getting harder to work with. however, it is unfair to pick on X11, as their are other apis (even modern)
that are just as bad, even worse.
x11 is particular annoying in that if it crashes, even launching a virtual terminal may not work as keys are'nt registered

don't make changes for conceptual cleanliness. end of the day, want to make performant, bug free code
in the shortest amount of time.

c runtime library way of packing return fail information is the reason for inverse truthniess

with closed source it is often the case that a company employs someone to oversee the experience of the software and have q+a 
therefore, better quality with this higher layer of checking than open source.
however, the same can be acheived in open source. to prevent bugs entering, open source should not be open for contributions.
in open source you just have to create a feature that makes lots of changes to introduce backdoors
in essence, this is a problem for closed source and open source. the best bet in safeguarding is to reduce the attack surface, which
means to reduce the number of lines of code.

if only eclispe cdt debugger was good. it terminates for no reason when running (can sometimes be fixed by switching to run mode and then back to debug). sometimes have to restart to fix. 
run-to-line doesn't work when inside sub-routine. memory browser crashes on entering address.
doesn't give information on stack overflows.

firefox will unexpectanctly say we updated in the background and you must restart or tab crashed etc.

using a dirty rectangle (part of screen to draw too) can be bug-prone

prefer using stdint over less expressive char *

reason for windows reordering memory order to control how it looks in the registers

pride cometh before the fall!
six of one, half a dozen of the other.
child's play with training wheels
by the seat of our pants
have our cake and eat it too
ad infinatum
give me a inch and i'll take a mile
downloading unverified external tool, always good to get more viruses on my machine...
I guess RTFM is the answer to that...
not going to go 50 rounds arguing
pie in the sky
but unfortunately I don't rule the world
learn to crawl before we can walk

it's not the programming practice but the dogma that gets you. when you start to name things it almost
always becomes bad. almost all programming practices have a place, just not used often
so RAII people, in case of things that must be released, e.g DeviceContext, ok to use a constructor/destructor
pair. I'll throw you a bone there

modifying (parse as pointer arg), returning (result struct).
it is best to put simple types are arguments rather than a group struct to allow for maximum code reuse.
only put group struct as arguments if must be put together
(in general don't pack so don't force user to create a struct when they may already have the values)

changing . to -> is the most useless thing you'll do in C

faster to write 32bit than 8bits

don't go through a ton of unecessary stdlib. malloc probably more optimised for small memory size requests

stack and heap memory are the same physical thing, so only more efficient if memory was hot, i.e. recently touched

use of set -x in bash scripts will exit if subprocess returns an error, even if 2>/dev/null. Cryptic!

may be easier for optimising compiler to work with things passed by value as oppose to pointer.
so, if need to modify something may have to return the value in a functional programming style.
however, this is error prone.

if we try to access memory from an invalid page (not reserved or comitted) will get segfault.
only have to worry about errors that don't manifest themselves on every run of the program.

direction of stack growth is often determined by CPU. if selectable, then OS. eg. x86 is downwards
ulimit -s for stack size

compiler cannot optimise pointers, e.g. if setting two values to same pointer, compiler cannot say that both the values are the same
as another pointer may point to the same address.
this is aliasing.
if we are just reading from pointers than it is fine.
(however some compilers may assume no alaising by default. we want to turn this off and manually markup up with restrict if this is the case)
however, still better to just past pointers first as if the compiler doesn't inline, we are incurring a large cost (i.e. we are relying on optimiser to inline)

const is only useful if you find it catches bugs for you (maybe for globals instead of using #defines)
however, in terms of optimsations, const is useless as you can cast const away.
therefore, for me, const is mostly just a waste of typing.

distinct areas of memory in assembly are stack, heap and data (globals)

getconf can list POSIX system configurable variables

input devices can poll or send interrupts. typically these days they are polled.

good practice to assign variable for syntatical reasons, i.e. more readable, e.g
Controller *controller = &evdev_input_device.controller;

build tools are more of a hindrance! always asking yourself what flags are being passed (linker and compiling separate steps), what files are it picking up, what is the CWD, etc.

^^^^^^^^^ Handmade Hero
-------------------------------------------------------------------------------------------------------------------------------------

Programming about solving problem.overlooked by design philosophies. If you don't have any functionality, you don't have a structural problem
Computer faster than you think, e.g instructions, clock cycles, cores. Very large
Many online communities are anti-engineering in that they don't embrace criticism. In reality, only core devs would be able to answer your question thoroughly
Go against merge requests from strangers and just auto let a group of trusted people.
Do anything on web takes a lot longer than it should dealing with a myriad of software with different odd conventions. Lack of functionality will lead to collapse
Many features lacking like type try to emulate. Many features have like garbage collection try to avoid.
Script cause heap fragmentation?
Script can't share with anything
Why just use a real language as we want robustness and checking
Loading thousands of dlls, spurious socket checks, sql server setup etc. With M2 drive should be quick
C standards and visual studio blocking us
Fundamental lack of awareness that there is a better way to program. We all make slow because of lack of time however. The cultural differences make it a fool's errand to try and get these people to program correctly
The time visual studio conceives of is less than 10 seconds?!
Before cpus increased in single thread execution speed. Now more cores. It's a topic of research to convert single threaded into multithreaded for emulation. This is why emulation of something like the GameCube (powerpc.altivec) is slow. Furthermore, hardware doesn't completely mirror spec. There will be irregularities like cache, register clearing, etc. That these programs rely on. So, you have to emulate a lot more than just the spec, but all the irregularities that the software ran on. Therefore in emulation may take hundreds of instructions, where is the actual device took one. If actually a simple translation, then should run close to native speed. This is reality of emulating hardware with hardware
Const rarely finds bugs that I have, i.e..writing to a variable a shouldn't. In saying that, you should use features of language that helps you catch bugs. Const rarely ever optimises as const-cast is a thing
Apple store is hardly a free marketplace. They can just block your app for any reason
Shipping on linux is very time consuming and not worth it monetarily
Exe contains header for sections and relocation tables
Packed files better as less OS operations performing expensive file handles etc.
Linker can read resource files as well as a object files. To make an installer just fwrite your executable and then data files appended with footer. Inside exe just check last bytes to see where data files are
Bake resources in for reliability only really
A file for every class will lead to awful build times
Conceptual resource is what we assign meaning to a collection of bits, e.g a bitmap buffer. A physical resource is like a file, which may contain conceptual resources in it
Most OSs give 64bit control over files, it's the filesystem that imposes the limits
Memory mapped files say if you write/read to a designated area in memory, do the same to a file on disk, I.e.range of memory to range of bytes on file
Process is allocated virtual memory space? We can configure the system value of this? OS has mapping table that converts these to physical addresses. In part of our processes address space is partitioned up by what OS thinks it will need on our behalf, e.g. gpu, hardware, file handles, dlls we can call into including kernel. This means max size of memory mapped file onx86 is about 2gb as our user process address space is smaller than max.
So, can talk about our process address space over memory.
Malloc is just reserving virtual memory range? Uses physical only when necessary?
Passion for games evident in prosperity points, e.g. in a museum. Drm makes this difficult as well as using an engine
Best way to test is to release on early access. This checks hardware and software, user may be running adobe acrobat which hogs cpu so instruct them to kill it before running your game. Or maybe 20000 chrome plugins. This is something a hardware lab can't tell you
Old days made intentional defects on CD so that reading a sector multiple times gives different data. This determined if using legit
Complete code coverage on the one hand is very thorough, however don't get a lot of engineering output. Furthermore, most bugs appear in between systems not in units. Also, the excessive testing is pushed by web where the poor languages dictate heavy testing. Testing first makes no sense as the app may change
Now checks for drm are distributed throughout code not a single easily patchable if
Compiler works on file by file, so knows nothing about calls across files. Therefore it generates object files which are partially executable machine code with unresolved symbols. Linker merges these object files and also adds extra header information so that the OS can load our executable (or more specifically a kernel, e.g. linux)
Sgx is an instruction set where the private key is stored on the cup directly. So, to break would require hardware hacking rather than software as it is currently. The provided public key is signed by Intel, so you can't forge one. In reality, NSA would rubberhose Intel for this key. This is very bad as now corporations determines who can buy/run what software. Drm gives up control of computer. Perhaps only until the video game industry is collapsing could this sacrifice be considered
OS determines usage of particular instructions, like sgx?
Program running in its own ring level? So no privilege escalation. Rootkit could be run in ring 0?
Use escrow to determine if selling cpu is blacklisted?
Ios already imposes users to sign things before running. This could be future
Before performance characteristics of graphics different to today's cache usages, memory footprint, instruction parallelism, multithreading, fast floating point multiplication. Then they were just minimising number of operations linearly
Can turn a calculation problem into a decision problem, e.g move vertically or horizontally based on error calculation
Floating point math faster than integers
Implicit function does not give an exact point, e.g equation of circle. Useful for determining error when the equation is arranged to equal zero and compare abs() sign. Also consider that drawing a circle is symmetrical, so only have to compute a quadrant and apply to others
Zii is having zero or one as state and offsetting to a value needed. This is more efficient then having to pull in values into the cache for initialization. Also, most OSs give you a zero page anyway for security concerns
my Style of programming and problems enjoy solving found in embedded, e.g your constrained with the silicon not like in web where you just build another data centre
Audiophiles think they hear things that aren't there
-------------------------------------------------------------------------------------------------------------------------------------
In virtual address space, have user space and kernel space address ranges. A virtual address is mostly relating to page table indexes and the last bit is a static offset (as for security addresses are randomized)
Gdb uses ptrace which acts as a parent process.
Much like the food industry has organic vs processed, we need a term for games that are made by people who love games and care about the experience as opposed to large companies concerned with making money (indie vs triple a)

You can abstract/encapsulate anything at anytime, so why not do it later when you know what you are doing?
We want file format to be simple and binary, unlike json which is general purpose and string

Sometimes crashing is good because it signifies the serious problem that can be rectified immediately rather than some other Insidious hidden bug
The problem is not mapping id/pointer (or whatever std:: c++ people would use) to an entity correctly (i.e. correspondence problem) .the symptom of this problem will be different depending on the implementation, e.g. pointer will crash program.

Crazy nuttiness of command line parsing UNIX. No one remembers single commands except privileged ls. 
Plus sign turning off, minus on, etc.
To make computers better to use, have to simplify them on all levels
Faster cpu like Apple's m1 are irrelevant if software bad
Optimiser allows for lexical scoping of stack variables. However, for optimiser to inline will have to get rid of pointers to prevent aliasing
Games about mechanical genre, e.g rts, fps. However think about what emotions you want to impart, e.g discovery, respect their intelligence, challenging, empowering, etc.
It's annoying with all gpu 'not documented' like raspberry pi. Hopefully risc-v will alleviate this
When learning proceed with little, verifiable steps. (MUST HAVE CONCRETE IDEA OF WHAT ARE VERIFYING!) 
x.func() for bash function names?
Can preserve information on reset by setting register values that will be read on reset?
Not really bare metal as gpu bootloader. We can ignore having to set uuid for Linux partition
The flip side of hugh level languages is loss of capability in controlling the cpu

Refactoring is essential. You must know what you are trying to achieve so you have some notion of progress, e.g. adding a constraint to the system. (Replacing variable type in scope gives us all locations where variable was used, including macros. This us where dynamic lnguages fail as you don't know if you broke something) 
Run time languages are slower and more complex

Normal mapping is storing normal, I.e direction. Very good interior for low res, only difference is contour edges
Every new idea was a frontier that people needed to figure out how to use.
For say matrix multiplication, the underlying math doesn't change but how we interpret the numbers does change based on use case
Scripting once, deploying everywhere is broken. You must test what you write on each machine. Computers aren't wonderful abstractions we wish they were

C was created to solve the problem of a portable high level assembly language for UNIX. C++ is a frankenstein languAge with bjarne just adding features in, e.g. he just wanted c with classes. Go and rust were designed for specific purposes, although I don't care about safe memory features or want garbage collection.
Complexity creates bugs. C++ incredibly complex. People just stick to a subset of c++, which may be different to yours. 
Exceptions defer error handling and also bring about an ethos of more errors when they should just be states of your program.
Defer statement handles destructors with error handling and is cleaner.
Static duck typing, e.g. at compile time check type of operands is clean way
C++ lambda functions, default arguments are useful also. Basic namespaces (not like boost crazy nesting) as well. 
Most languages aren't designed for modern hardware, e.g. simd, threads (jai is)

Linux desktop suffers from binary compatibility nightmare, e.g. 15 binaries to support all. This is because libraries like glibc are happy to break abi to get minor improvements, e.g spec says this so let's do it even though no one cares

In virtual address space, have user space and kernel space address ranges. A virtual address is mostly relating to page table indexes and the last bit is a static offset (as for security addresses are randomized)
Gdb uses ptrace which acts as a parent process.
Much like the food industry has organic vs processed, we need a term for games that are made by people who love games and care about the experience as opposed to large companies concerned with making money (indie vs triple a)

The backbone of web tcp/ip has scaled tremendously well over 60 years. The web software stack is opposite. Browsers rev every 15 minutes, everything is worse than it was before, JavaScript slower than anything before it.
Nodejs, php etc. Bourne out of good idea to mAke a particular type of software development rapid. However they fail to effectively utilise system resources to solve a problem that would be easy in C.
Often people are only concerned with how fast they can write this, without caring about quality.
In the web sphere google, Facebook etc. Know their competitors aren't going to be concerned with quality, e.g. gmail is incredibly slow and littered with bugs (typing in a name gives wrong result as it hasn't finished time to server) so they use these technologies that help to get something working quickly but will be janky. 
I want to write software where quality matters a nd software is enjoyable to use. Web needs to acknowledge this. They have different thinking, e.g. network latency so we don't have to care about performance, no you need more effort to avoid this latency!
Although different tradeoffs made for different contexts, on a whole programming is programming.
The web is almost impossible to write good software as have to deal with complexities that don't have to be there. Best minds are being put to make people click ads
Testing is important. If you don't write tests, your software doesn't work. However, write higher level system tests, not excessive unit tests. More efficient and this is where bugs are likely to be. You often have to remove code, so having unit tests just increases the volume of code you have to write. Huge drain on productivity. Maybe for NASA. A new paradigm should weigh up cost-benefit. Almost always the cost is ignored and people gobble them up
High level don't worry about implementation details - chrome c++ entering a 1 char created 25000 memory allocations with std::strong
Abstractions - more code makes program as a whole more difficult to understand. Abstractions are just hiding low level which is the most important part and will constrain the system. Java huge call stack to make a http request. Make lightweight abstractions only
Functional programming - Haskell around since 1990 hasn't taken over. Imperative more clear. Functional breaks down over non trivial work
Data hiding - poor cache performance, redundant code
Excessive inheritance - poor cache performance
Exceptions - constant cognitive tax on remembering what throws what and huge verbosity doing so. Also don't know what program will throw during some time
Commenting - write good comments and garden them as they can rot easily
Re-use - only effective when used to small extent, humans are bad at understanding layers upon layers. You don't make something better by making it more complicated
All aforementioned ideas can be contextually useful
Most times you upgrade cup makes little difference due to software on it

Successful ideas are higher level languages to assembly and compile time type checking
Success is solving a problem people have talked themselves out of solving
Why does Twitter need 4000 employees? Spacex is roughly the same and the put rockets on mars!
They make problems difficult by the engineering culture they create. 18000 classes overflowed java function pointer buffer, they are deluded in thinking this way

Over concerned with Uml, state diagrams, acronyms, etc. Nightmarish distractions/unproductive

Even if running slow, problem only using like one core if badly designed program
Can use nsight to determine why incorrectly sampling pixels?
Never use setters/getters unless actually doing something. You're spending your entire day typing. If needing, replace variable name with name_ to see where it was used.
You need to be self critical to be a good engineer
Making some thing good takes time. However, if you have and design practices it will also take longer
You have to be reality based when programming. That is in an engineering sense, to design something that solves the problem you have
People become attached to a way of programming which doesn't focus on solving the problem. They want to build rube-goldberg machines
Selectively attacking problems seriously means you have a functional program quicker, whereby you can actually decide if those other problems need to be addressed. Can defer hard decisions later as they will be made better as you will have more technical expertise and more context to work with
Many new languages these days are just stylistic changes to existing languages. Rust is different  in that it tries to force you to program in a way that is 'more safe'

Valid code can cause the graphics (ATI driver ?) driver to crash your program (a program using driver can crash it with say out of bounds access. This cascading could crash you)
We can see the start address of the thread is what called it
Graphics card crash proof by checking all calls and setting up a trap in a launcher executable that will flush all open files etc.and close and restart application
Polymorphism is a single object that can be interpreted as having various types. This can simply be a struct with unions and a type field.
C++ templates are very weak in implementing meta programming. Just write code that generates code. More powerful and readable. Templates are fine for simple type substitution saving key strokes, however more complicated things you want to do with them generates a rat's nest of code, like most things in c++
Polymorphism and meta/generic programming are fine, however how they are implemented in c++ is bad. Any example with c++ class inheritance or templates becomes extremely convoluted just to do simple things like a clone function for each type. Ctrp is inverting inheritance, I.e. it gets all of you
Virtual functions have a hidden function pointer that points to what the overridden function is.
Like a an switch enum for different functions
When you make a feature that is supposed to empower the programmer but makes what was once simple tedious, you've made a mistake

Caches are a way of minimising roundtrip time of RAM by putting memory as close to the core that thinks will need it
L1 closest, 1/2 cycles, 32k. Wikichip for more info
Inclusive caches will fill them all with data. Cpu will go to caches first
Whether a cache miss incurs a performance penalty is determines by what the cpu could execute at that time based on dependency chain of operations.
Cache line is how much data cache moves aroud at a time. As it may evict other data, how your data is arranged matters
L1 can supply 2 cache lines per clock
Instructions per clock, number of work components, e.g. number of add components, cache line per cycles, cache latency, agu (address resolver units) impose restrictions
Know cache line and size of cpu. Break up things into these sizes and access them locally. Access memory linearly/simply for cache prefiller in the case of giant memory accesses. Finally, can use explicit prefetch instructions.
A cache miss is simply stalling for an instruction. However, this may not be an issue if we do other work, e.g. complex algorithm takes many instructions hiding memory access for out of order cpu. If hyper threading with two schedulers, if cache miss on one, just switch to the other.
Can really only know if a cache miss incurs a performance penalty by looking at raw numbers from vtune, etc. Because of the scheduler, it's not as simple as just looking at memory sizes
Uop website displays table for instructions
Ps3 1 core multiple spus? Fpus? Had manual caching
Currently good that most things are little endian with 64 byte cAche lines, however some hardware guys Is going to come along and change it back to big endian
Should be code of ethics in software to not create bugs/inconveniences for users that couldve been avoided
Borrowing money: clear you're the right person to give the money to. Clear you understand what you're doing and the game process is figured out. Proof of you will complete it. Here is the game, why I'm good at it, why people will like it and why I'm going to succeed at developing it
Brian enjoys playing gAmes if it thinks it is learning something
Why is it big news regarding command prompt. Battlefield orders of magnitude complex and runs faster
Msdos was uniprocessing. OS may move memory to disk if waiting for long disk up io e.g.
Logical address to physical is virtual memory. Binary will have sections that give an initial memory allocation size. This will increase with dlls requesting more memory as well as the application itself
Msdos could see all physical memory
Virtual memory; paging uses fixed sizes memory blocks, segmentation variable. Frames are sections of physical memory
Modern cpus have mmu

Programming about solving problem.overlooked by design philosophies. If you don't have any functionality, you don't have a structural problem
Computer faster than you think, e.g instructions, clock cycles, cores. Very large
Many online communities are anti-engineering in that they don't embrace criticism. In reality, only core devs would be able to answer your question thoroughly
Go against merge requests from strangers and just auto let a group of trusted people.
Do anything on web takes a lot longer than it should dealing with a myriad of software with different odd conventions. Lack of functionality will lead to collapse
Many features lacking like type try to emulate. Many features have like garbage collection try to avoid.
Script cause heap fragmentation?
Script can't share with anything
Why just use a real language as we want robustness and checking
Loading thousands of dlls, spurious socket checks, sql server setup etc. With M2 drive should be quick
C standards and visual studio blocking us
Fundamental lack of awareness that there is a better way to program. We all make slow because of lack of time however. The cultural differences make it a fool's errand to try and get these people to program correctly
The time visual studio conceives of is less than 10 seconds?!
Before cpus increased in single thread execution speed. Now more cores. It's a topic of research to convert single threaded into multithreaded for emulation. This is why emulation of something like the GameCube (powerpc.altivec) is slow. Furthermore, hardware doesn't completely mirror spec. There will be irregularities like cache, register clearing, etc. That these programs rely on. So, you have to emulate a lot more than just the spec, but all the irregularities that the software ran on. Therefore in emulation may take hundreds of instructions, where is the actual device took one. If actually a simple translation, then should run close to native speed. This is reality of emulating hardware with hardware
Const rarely finds bugs that I have, i.e..writing to a variable a shouldn't. In saying that, you should use features of language that helps you catch bugs. Const rarely ever optimises as const-cast is a thing
Apple store is hardly a free marketplace. They can just block your app for any reason
Shipping on linux is very time consuming and not worth it monetarily
Exe contains header for sections and relocation tables
Packed files better as less OS operations performing expensive file handles etc.
Linker can read resource files as well as a object files. To make an installer just fwrite your executable and then data files appended with footer. Inside exe just check last bytes to see where data files are
Bake resources in for reliability only really
A file for every class will lead to awful build times
Conceptual resource is what we assign meaning to a collection of bits, e.g a bitmap buffer. A physical resource is like a file, which may contain conceptual resources in it
Most OSs give 64bit control over files, it's the filesystem that imposes the limits
Memory mapped files say if you write/read to a designated area in memory, do the same to a file on disk, I.e.range of memory to range of bytes on file
Process is allocated virtual memory space? We can configure the system value of this? OS has mapping table that converts these to physical addresses. In part of our processes address space is partitioned up by what OS thinks it will need on our behalf, e.g. gpu, hardware, file handles, dlls we can call into including kernel. This means max size of memory mapped file onx86 is about 2gb as our user process address space is smaller than max.
So, can talk about our process address space over memory.
Malloc is just reserving virtual memory range? Uses physical only when necessary?
Passion for games evident in prosperity points, e.g. in a museum. Drm makes this difficult as well as using an engine
Best way to test is to release on early access. This checks hardware and software, user may be running adobe acrobat which hogs cpu so instruct them to kill it before running your game. Or maybe 20000 chrome plugins. This is something a hardware lab can't tell you
Old days made intentional defects on CD so that reading a sector multiple times gives different data. This determined if using legit
Complete code coverage on the one hand is very thorough, however don't get a lot of engineering output. Furthermore, most bugs appear in between systems not in units. Also, the excessive testing is pushed by web where the poor languages dictate heavy testing. Testing first makes no sense as the app may change
Now checks for drm are distributed throughout code not a single easily patchable if
Compiler works on file by file, so knows nothing about calls across files. Therefore it generates object files which are partially executable machine code with unresolved symbols. Linker merges these object files and also adds extra header information so that the OS can load our executable (or more specifically a kernel, e.g. linux)
Sgx is an instruction set where the private key is stored on the cup directly. So, to break would require hardware hacking rather than software as it is currently. The provided public key is signed by Intel, so you can't forge one. In reality, NSA would rubberhose Intel for this key. This is very bad as now corporations determines who can buy/run what software. Drm gives up control of computer. Perhaps only until the video game industry is collapsing could this sacrifice be considered
OS determines usage of particular instructions, like sgx?
Program running in its own ring level? So no privilege escalation. Rootkit could be run in ring 0?
Use escrow to determine if selling cpu is blacklisted?
Ios already imposes users to sign things before running. This could be future
Before performance characteristics of graphics different to today's cache usages, memory footprint, instruction parallelism, multithreading, fast floating point multiplication. Then they were just minimising number of operations linearly
Can turn a calculation problem into a decision problem, e.g move vertically or horizontally based on error calculation
Floating point math faster than integers
Implicit function does not give an exact point, e.g equation of circle. Useful for determining error when the equation is arranged to equal zero and compare abs() sign. Also consider that drawing a circle is symmetrical, so only have to compute a quadrant and apply to others
Zii is having zero or one as state and offsetting to a value needed. This is more efficient then having to pull in values into the cache for initialization. Also, most OSs give you a zero page anyway for security concerns
my Style of programming and problems enjoy solving found in embedded, e.g your constrained with the silicon not like in web where you just build another data centre
Audiophiles think they hear things that aren't there
Interview ability to explain problems you have encountered on projects
Unless your in web where everything takes years to load, as single threaded performance is largely stagnant you will have to utilise parallelism if want performance. This is very difficult and like single threaded code generic libraries which can actually be very specific will create bloated in performing code based.
Multithreading is building up a new discipline to single threaded. There are a lot of pitfalls for performance (balancing want things local, however must share to utilise)
If you care about performance for anything, you should care about cache misses
Memory bandwidth and caches are major reasons for a cup attaining performance. You have to think about where does memory actually live and how is it transferred around
View cpu as sections where there is some distance to communicate.
An instruction of throughput 1 means issued every clock. As many instructions take longer than 1 cycle, each core requires a scheduler to see if it can execute something.
