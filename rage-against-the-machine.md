# Rage Against the Machine

https://c3.handmade.network/blog/p/8486-the_case_against_a_c_alternative

  instead of feature, we introduce a patented terminology like user stories to have someone teach you them. slows down development
  business logic .... just means high-order operations in say main()

  Incessent unit-testing, why not test startup assembly then. Falls apart... What not to test, e.g. assume that hex_to_bin() simple enough to work?
  Introducing formulas to determine whether or not to automate something....


certain 'design pattern' enforce really long names to conform to pattern. 
any competent programmer can read use-case specific functions with clearer names
"test_CommandHardware_CheckForMsg_Should_GetCharAndAddToPacker_When_BytesAvailable"

file for every test, file for every class. leads to awful build times

How can software better serve humanity? e.g. bloatware causes slowness for aus post workers

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

reject the idea of TDD driving good design. accept that tests validate design.
tdd and bdd have good elements in them, but the dogma is not effective.

code shouldn't take longer than 10 seconds to compile.

in general we don't add security threats that weren't already present, e.g. loading from shared object could just as easily override binary if have write priveleges to both

even though a 'new' language won't crash, it can still have buffer overruns.
NullPtrExceptions 

c++ struct functions implicitly have a this pointer.
virtual functions will result in a vtable (array of function pointers) to be generated for that struct.
therefore, virtual function will first go to vtable, then lookup in function, so double indirection (so not a zero-cost abstraction)
normal function call just call 0x1234, however with vtable mov rax, qword ptr [rsp + 20] etc. (dereferencing pointers)

an engine makes things that aren't likely to be difficult easy (except for linux...)
important to know low-level to write new tools. we don't have a wheel yet.

for any non-trival task, scripting languages become a hindrance with no static type checking, no real debugger, slow, not as capable.
there are complications in software we have wrongly convinced ourselves are necessary, e.g. scripting for hotloading
hotloading C is far superior, as C is more powerful and can use same debugger (using Lua is a downgrade)
build systems can be useful as they allow for incremental builds (however, in negatively reinforces people to only make small changes)
(speed increase may only be noticeable for large, complex code base)
they are also useful for managing cross compilation (libraries have to be pulled in and compile also)
the idea of incorporating a scripting language into a game was a failed experiment in 2005s.
things like a visual based interface is fine as it is constrained

with closed source it is often the case that a company employs someone to oversee the experience of the software and have q+a 
therefore, better quality with this higher layer of checking than open source.

the best bet in safeguarding security is to reduce the attack surface, which means to reduce the number of lines of code.

Don't restrict right side of bell curve
Let your aces be aces
Being an ace involves having an opinion
Most influential software written largely by one person, e.g Linux, Unix, git etc. Then a team is assigned to maintain it. Fallacy about solo programmer productivity requiring large teams.
Design by committee pushes design to middle of bell curve as opposing views average out

templates add complexity in the debugger (no actual names). 
only really useful if you save a ton of code (not much code to just write each implementation)
if templates are necessary then just use meta-programming as it is much more powerful.

many programs treat memory as an infinite resource. allocating memory is introducing a failure case and making
not a fan of allocation festivals.
we can create our application with minimal failure cases (cannot do this with the platform layer).

uml and diagrams in general are a waste of time (its just code you would write and often fails to capture subtleties)
you should become more proficient at reading code and understanding its relationship.

oh no, we had a security bug in our development version! (printf and friends. printf %f defaults to double)

understanding history is important; c runtime library way of packing return fail information is the reason for inverse truthniess

downloading unverified external tool, always good to get more viruses on my machine...
unless playing at EVO with some maxed out razer device, not feasible to hit that hard
I guess RTFM is the answer to that...

build tools are more of a hindrance! always asking yourself what flags are being passed (linker and compiling separate steps), what files are it picking up, what is the CWD, etc.

Go against merge requests from strangers and just auto let a group of trusted people.
This avoids the problem where you work for hours only to have it blocked

Many online communities are anti-engineering in that they don't embrace criticism. 

Do anything on web takes a lot longer than it should dealing with a myriad of software with different odd conventions. Lack of functionality/integration with hardware will lead to collapse
Many features lacking like type system, try to emulate. Many features have like garbage collection try to avoid.

Scripting languages can cause heap fragmentation. Why just use a real language as we want robustness (scripting languages dependent on interpreter speed) and type checking

Fundamental lack of awareness that there is a better way to program. We all make slow because of lack of time however to say it can't be done is a fallacy.
The cultural differences with these people make it a fool's errand to try and get these people to program correctly
e.g. time visual studio users think is fast is less than 10 seconds?!

Const rarely finds bugs that I have, i.e..writing to a variable a shouldn't. In saying that, you should use features of language that helps you catch bugs. 

Apple store is hardly a free marketplace. They can just block your app for any reason

DRM and engine usage make using games in the future difficult (e.g. museum)

Also, the excessive testing is pushed by web where the poor languages dictate heavy testing. Testing first makes no sense as the app may change

Audiophiles think they hear things that aren't there

Much like the food industry has organic vs processed, we need a term for games that are made by people who love games and care about the experience as opposed to 
large companies concerned with making money (indie vs triple a)

Sometimes crashing is good because it signifies the serious problem that can be rectified immediately rather than some other Insidious hidden bug
The problem is not mapping id/pointer (or whatever std:: c++ people would use) to an entity correctly (i.e. correspondence problem).
the symptom of this problem will be different depending on the implementation, e.g. pointer will crash program.

Crazy nuttiness of command line parsing UNIX. No one remembers single commands except privileged ls. 
Plus sign turning off, minus on, etc.

The flip side of high level languages is loss of capability in controlling the cpu
Run time languages are slower and more complex
Most scripting languages aren't designed for modern hardware, e.g. simd, threads

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

Successful ideas are higher level languages to assembly and compile time type checking
Success is solving a problem people have talked themselves out of solving
Why does Twitter need 4000 employees? Spacex is roughly the same and the put rockets on mars!
They make problems difficult by the engineering culture they create. 18000 classes overflowed java function pointer buffer, they are deluded in thinking this way
Over concerned with Uml, state diagrams, acronyms, etc. Nightmarish distractions/unproductive

High level don't worry about implementation details - chrome c++ entering a 1 char created 25000 memory allocations with std::strong
Abstractions - more code makes program as a whole more difficult to understand. Abstractions are just hiding low level which is the most important part and will constrain the system. Java huge call stack to make a http request. Make lightweight abstractions only
Functional programming - Haskell around since 1990 hasn't taken over. Imperative more clear. Functional breaks down over non trivial work
Data hiding - poor cache performance, redundant code
Excessive inheritance - poor cache performance
Exceptions - constant cognitive tax on remembering what throws what and huge verbosity doing so. Also don't know what program will throw during some time
Commenting - write good comments and garden them as they can rot easily
Re-use - only effective when used to small extent, humans are bad at understanding layers upon layers. You don't make something better by making it more complicated
All aforementioned ideas can be contextually useful

The backbone of web tcp/ip has scaled tremendously well over 60 years. 
The web software stack is opposite. 
Browsers rev every 15 minutes, everything is worse than it was before, JavaScript slower than anything before it.
Nodejs, php etc. Bourne out of good idea to mAke a particular type of software development rapid. 
However they fail to effectively utilise system resources to solve a problem that would be easy in C.
Often people are only concerned with how fast they can write this, without caring about quality.
In the web sphere google, Facebook etc. Know their competitors aren't going to be concerned with quality, 
e.g. gmail is incredibly slow and littered with bugs (typing in a name gives wrong result as it hasn't finished time to server) 
so they use these technologies that help to get something working quickly but will be janky. 
I want to write software where quality matters and software is enjoyable to use. 
Web needs to acknowledge this. They have different thinking, e.g. network latency so we don't have to care about performance, no you need more effort to avoid this latency!
Although different tradeoffs made for different contexts, on a whole programming is programming.
The web is almost impossible to write good software as have to deal with complexities that don't have to be there. Best minds are being put to make people click ads

Why is it big news regarding command prompt. Battlefield orders of magnitude complex and runs faster
Msdos could see all physical memory; Modern cpus have mmu

must be able to judge the quality of something from your own criteria, not simply on social cues/norms

memory safety not of that much concern for programming. however, for security I suppose it is?
wish security wasn't an issue
https://github.com/KULeuven-COSIC/Starlink-FI/

various daughterboard accoutrement nomenclature, e.g. arduino shield, beaglebone cape, raspberry pi hat etc.

discordant packaging naming in ubuntu 'dummy packages', e.g. qemu is package name, however binaries are named differently.

still installing an OS is a cross-your-fingers exercise. A single USB presents multiple boot options.
choosing between uefi 1.0 and 1.0 one will give you WIFI during install
ubuntu forums have contemporary posts mentioning install issues
this finicky nature means just have barebones things and install separate OS in virtual machine
chose xcfe4 for gnome-repeat-bug and easy shortcuts

the lack of type systems in Javascript and Python leads to extensions with it

Vulkan like filling in a centrelink form,  so much setup

Any overarching idelogoy like everything is objects, everything is data is pretty arbitrary. 
Certain approaches are useful for particular problems.
I'm sensiiltive to Input lag in text editor,  so use vim.  Intellisense pop ups slow

Although large groups can help maintain, the introduction of many 
can create imperfections
This extends to all software companies.  Do a lot of good things, 
but will always have things that are dysfunctional at, e.g documentation, 
disparate build systems, non-uniform design practices
Don't restrict right side of bell curve
Let your aces be aces
Being an ace involves having an opinion
Most influential software written largely by one person, e.g Linux, Unix, git etc. 
Then a team is assigned to maintain it. 
Fallacy about solo programmer productivity requiring large teams.
Design by committee pushes design to middle of bell curve as opposing views average out

Loss of generational knowledge, i.e. people forgetting gensis of influential software written by individuals 

Growth of machine learning due to quantity of computation available. 
Localised improvements, overall degradation (rendering, update processing etc. far more involved) 
Wrong perception that performance takes too much time, so we want do it but we could do it. 
However, if they have never done it, how could they say they could?
Growth of startups are just finding a niche, rather than writing revolutionary software

Testing nomenclature so diverse, yet could be boiled down to a handful of terms, e.g.
Regression testing just re-running tests on new changes, which is something you do implicitly

JSON is not even robust enough to allow trailing commas that even C89 arrays support ...

Now have 'memory leak' finder tools for interpreted languages like Javascript...

IPv6 classic example of second-system effect 

Windows have to enable hidden file extensions to not enter a .txt file

Although I'm in favour of an OS wrapper, these don't give you all the control you need and you end up having to write your own OS specific code

Unfortunately can't even use modern GNU AVR assembly with numbered labels, advanced macros and location counter

C stdlib BUFSIZ macro vastly different across OSs, e.g. 1K, 8K, 512bytes

Concept of non-daemonic and daemonic 'threads' in python

w3m-img installs to non default path /usr/lib. Have to resort to X11 python ueberzug

difference in size between C and CPP header files for vulkan SDK is huge, leads to much slower compile times.
contains some useful things, but not that useful

Unfortunately software like npm is poorly designed so will inevitable get 'fast/lightweight' variants

Many tutorials on low-level like Vulkan, X11 can be wrong so need to understand spec

Fibres are green threads, i.e. not OS or hardware threads so not actually faster.
They just allow interleaved execution which is used in web to not freeze UI on large linear execution

Unfortunate trend on Internet for people to ask why aren't you using what I'm using, e.g. VScode, as oppose to asking
why are you doing things your way
Tech news propagates shallow information fast. In aggregate, most of it not important, e.g. new bright blueberry distro release 

The distinction between new as in fashion and new as in forward progress can be overlooked in tech.
Unfortunately, most is the former

Despite Wayland (and Mir) being far more sane (merging display server and client into one), still not fully supported

Being able to sift through low-quality tech news essential, e.g. 
'new' database library, css library, VR headset, distro, container library etc.
Amazon, Tesla, Twitter, Netflix endeavours
Cancer, alzheimer trial drugs
Fusion energy

Have to update Ubuntu version if want to easily get compiled version of modern gcc

Unfortunate mnemonic repetition of nonsensical statements in programming, 
e.g. manual memory management is hard (overly simplistic and misleading)

Auto, i.e. type inference is good when the type is not actually known.
However, often used in replace of a name that is too long to type, leading to unclear types and over complication

Virtually every week a new 'container-specifier' project appears on Github

C grew out of existing code. All new languages trying to be a top-down design on the opinions of a single programmer (BDFL) 
Just have a simple language with ideal metaprogramming/code generation facilities so the programmer can decide, not the language designer

Whilst github codespaces seems cool, the reason for having some many containers is that the build systems for each project is incredibly complex and requires many dependencies

Sometimes have to rephrase something for the compiler to generate more optimal code,
e.g. div /= 2; div /= 2; will generate more efficient instruciton that div >> 2 for avr_gcc
