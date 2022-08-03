<!-- SPDX-License-Identifier: zlib-acknowledgement -->

QUESTIONS:
Shipping on linux is very time consuming and not worth it monetarily?
why writing by words faster?
Linux desktop suffers from binary compatibility nightmare, e.g. 15 binaries to support all. 
This is because libraries like glibc are happy to break abi to get minor improvements, e.g spec says this so let's do it even though no one cares

the constellation we can't see as it's blocked by the sign is the zodiac sign for that month
not new territory to regain interest in the past, e.g. renaissance grew interest in ancient greek astrology
zodiac latinized of zodiakos, meaning circle of animals (all zodiac names latinized greek)
vernal is beginning of astronomical year; daylight starts getting longer until summer solstice.
hence, spring is when zodiac starts
aries (march 20th) -> ram
taurus (april) -> bull
gemini (may) -> twin brothers
cancer (june) -> crab
leo (july) -> lion
virgo (august) -> woman
libra (september) -> scales of justice
scorpio (october) -> scorpion
sagittarius (november) -> archer's arrow
capricorn (december) -> goat 
aquarius (january) -> water bearer
pisces (feburary) -> opposing fish

IS IT POSSIBLE THAT AN ASSEMBLY INSTRUCTION LIKE RDTSCP COULD BE TRAPPED BY PROGRAM LOADER?

With an undeclared identifier in a unity build, just reorder #includes in main file

TODO(do the STL of other languages suffer the same issues as in C?):

NOTE(importance of reading programming papers.... after handmade finished)


vector math routines (obtaining cross product from column vector form)
when drawing vectors in a physical sense, 
keep in mind they are rooted at the origin (even if drawings show them across time)
whenever doing vector addition/subtraction, remember the head-to-tail rule (their direction is determined by their sign).
could also think that subtract whenever you want to 'go away' from something
dot product transpose notation useful for emulating matrix multiplication
unit circle, x = cosθ
dot product allows us to project a vector's length onto a unit vector 
dot product allows us to measure a vector on any axis system we want by setting up two unit vectors that are orthogonal to each other
simple plane equation with d=0 will be through origin (altering d shifts the plane up/down)
cross product gives vector that is orthogonal to the plane that the two original vectors lie on (length is |a|·|b|·sinθ). So, really only works in at least 3 dimensions
with units, e.g. for camera, start with arbitrary 'unit' defintion. later move onto more physical things like metres
by applying a scaling factor to direction vector, can move along it
world space coordinates. camera position is based on these. the camera will have its own axis system which we determine what it should be and then use cross product based on what we want
understanding dot product equivalence with circle equation
for multiplication of vectors, be explicit with a hadamard function
(IMPORTANT have reciprocal square root approximation which is there specifically for normalisation. 
much faster cycle count and latency than square root)

noise is randomness. white noise is complete randomness. blue noise (harder to generate) is randomness with limitations on how close together points can be (more uniform)

pride cometh before the fall!
six of one, half a dozen of the other.
have our cake and eat it too
ad infinatum


Crazy nuttiness of command line parsing UNIX. No one remembers single commands except privileged ls. 
Plus sign turning off, minus on, etc.

To make computers better to use, have to simplify them on all levels

Faster cpu like Apple's m1 are irrelevant if software bad
Optimiser allows for lexical scoping of stack variables. However, for optimiser to inline will have to get rid of pointers to prevent aliasing

Games about mechanical genre, e.g rts, fps. However think about what emotions you want to impart, e.g discovery, respect their intelligence, challenging, empowering, etc.

It's annoying with all gpu 'not documented' like raspberry pi. Hopefully risc-v will alleviate this

The flip side of hugh level languages is loss of capability in controlling the cpu
Run time languages are slower and more complex
Most scripting languages aren't designed for modern hardware, e.g. simd, threads

Refactoring is essential. You must know what you are trying to achieve so you have some notion of progress, e.g. adding a constraint to the system. 
(Replacing variable type in scope gives us all locations where variable was used, including macros. This us where dynamic lnguages fail as you don't know if you broke something) 

Scripting once, deploying everywhere is broken. You must test what you write on each machine. Computers aren't wonderful abstractions we wish they were

C was created to solve the problem of a portable high level assembly language for UNIX. 
C++ is a frankenstein languAge with bjarne just adding features in, e.g. he just wanted c with classes. 

Go and rust were designed for specific purposes, although I don't care about safe memory features or want garbage collection.

Complexity creates bugs. C++ incredibly complex. People just stick to a subset of c++, which may be different to yours. 

Exceptions defer error handling and also bring about an ethos of more errors when they should just be states of your program.

roughly 90% of games played on PC games are pirated.
however intensive anti-piracy makes it harder for people to play your game
yet, in line with this is, there is no code of ethics in computer science for wasting peoples time. so, get more respect from community?
drm just introduces another way for the software to waste people's time (however, may come a day when it's required...)



The backbone of web tcp/ip has scaled tremendously well over 60 years. The web software stack is opposite. 
Browsers rev every 15 minutes, everything is worse than it was before, JavaScript slower than anything before it.
Nodejs, php etc. Bourne out of good idea to mAke a particular type of software development rapid. 
However they fail to effectively utilise system resources to solve a problem that would be easy in C.
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

Program running in its own ring level? So no privilege escalation. Rootkit could be run in ring 0?

Interview ability to explain problems you have encountered on projects
Unless your in web where everything takes years to load, as single threaded performance is largely stagnant you will have to utilise parallelism if want performance. This is very difficult and like single threaded code generic libraries which can actually be very specific will create bloated in performing code based.
Multithreading is building up a new discipline to single threaded. There are a lot of pitfalls for performance (balancing want things local, however must share to utilise)
If you care about performance for anything, you should care about cache misses
Memory bandwidth and caches are major reasons for a cup attaining performance. You have to think about where does memory actually live and how is it transferred around
View cpu as sections where there is some distance to communicate.
An instruction of throughput 1 means issued every clock. As many instructions take longer than 1 cycle, each core requires a scheduler to see if it can execute something.
