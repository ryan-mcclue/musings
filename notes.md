<!-- SPDX-License-Identifier: zlib-acknowledgement -->

telecommunications standards:
* GSM (global system for mobile communications) - uses SIM (subscriber identification module) cards to authenticate and authorise access
I suppose eSIM would make SIM-locking easier to overcome 
Phone number linked/contained within SIM, however can be reassigned
* CDMA (code division multiple access) - uses ESN (electronic serial number) 

Loss of generational knowledge (chip manufacturing, BIOS grey beards) 
Machine learning due to quantity of computation available. Localised improvements, overall degradation (rendering, input etc. far more involved) 
Wrong perception that performance takes too much time, so we want do it but we could do it. However, if they have never done it, how could they say they could?
Most startups are just finding a niche, rather than writing revolutionary software

Count cycles to counteract possible thermal throttling
Huperthreads useful only if different execution unit
Cpu reads memory from cache and ram in cache lines (due to programmer access patterns) . Each item in cache set is cache line size
Booles and Babbages of the victorian era
Electrickery
Low cache associativity means fast lookup but a lot of misses and thus eviction policy like LRU
Tim Berners-Lee envisioned a universal place for information exchange. Unfortunately the freedom of creativity has resulted in much of the Web being intrusive and hindering
Gplv3 extends to the system you're running on?
Contempt culture (language wars)-change-systemd
Original Unix borne out of want for simplicity over competing systems
Terminal type writer something you should ask your grandfather about
Service is like a collection of daemons.  Need for services came from growth of Web and need for handling databases,  lots of connections etc. Something like launchd (an unit system) allows services to be started not at boot time.  Also listen for software hardware changes.  So,  we extend from simple bootstrapping to handling events.  So,  now more than service manager,  a system manager
As kernel more dynamic,  e.g change IRQ for device at runtime,  can think of a system layer as an intermediary between kernel and users pace.  Have things like network manager. This avoids having 15 disparate packages each with own file format config
Systemd a collection of binaries,  so not exactly violating Unix philosphy
Some disparity between systemd idea and its inplementation
Major component of systemd is service manager,  I.e init system to bootstrap user space


Distinction between game companies like Activision Blizzard, EA, Take-Two and publishing labels 
like Rockstar, Treyarch, 2K


Udp (head of line blocking) +
client server (p2p unreliable as internet path optimised for cost/closest exchange point) 
+ dedicated (peoples home Internet don't normally have high upload rates); Mix of cloud (flexible to just turn up and down, high egress bandwidth charge) and bare metal (fixed bandwidth rate set into price) 
Matchmaking,  host migration difficult as hard to measure what user has good connection,  
e.g. Whats there NAT type?


Software laws,  e. G.  Have 4k in name,  but image quality 1080p, Elon musk making false claims, 
Most dash cams same SoC,  and image sensor,  just different housing 
Garmon products to use must sign up for garmin account and agree to collection of data


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


what are laws for misinformation, e.g. tesla, '4k' dash cams, stats misleading: M1 chip faster (only faster than Intel based macs),
we don't track in 'traditional' ways, tiktok keylogger technology exists but not using

TODO: favourites tab for morning viewing
videos: common sense skeptic; techquickie; handmade podcast; network next

TODO: gather in-depth notes in ~/Pictures to a wallpaper-aware.md.temp

chefgood: 20 x $209
mymusclechef: 20 x $219

dinner + lunch: $220
breakfast: $100
rent: $345
phone: $70

leftover: $265

salary: ≈$52,000 

fortnightly: $1400
+
quarterly: $4000
=
weekly: $1000 

how does quantum computing work?

communicating with busy people:
https://threadreaderapp.com/thread/1562510420644343810.html

battery balancing relevent to multiple cells
TP4056 Lithium Battery Charging Board?

is distinction between a unicast (device to device), multicast (device to some devices), broadcast (device to all devices)
only discernable in packet format, i.e. must be parsed by network card first?
are signal strength changed?

thread local storage could be implemented hyper-threaded cpu?

memory model: visibility and consistency of changes to data stored in memory
necessary for understanding multiple thread execution 

hardware (from machine code to execution on the CPU):
* sequential consistency (ideal as easy to understand, however doesn't maximise hardware speed)
* x86-TSO (total store order). 
each processor reads from shared memory
all processors agree upon the order in which their write queues are written to memory
write queue is FIFO, so write queue is flushed in same order as seen by the processor
however, when the write queue is flushed is up to the CPU
therefore, to enforce stronger memory ordering, architecture supplies memory barriers/fences 
(in a sense to enforce sequentially consistent behaviours)
to ensure write queue is flushed before any future reads occur
(without fences, I suppose write queue length would determine whether certain synchronisation problems occur more frequently)
* ARM (most relaxed)
each processor reads and writes from its own copy of memory
writes propagate to other processors independently, i.e not all update at same time
furthermore, the order of the writes can be reordered
furthermore, processors are allowed to delay reads until writes later in the instruction stream

language (from source to machine code)
so, C11 memory model is in relation to rearranging of loads/stores in assembly for optimisations? (so is a weak model?)
mainly in reference to the behaviour of atomics to synchronise programs
https://research.swtch.com/plmm



Before cpus increased in single thread execution speed. Now more cores. It's a topic of research to convert single threaded into multithreaded for emulation. 
This is why emulation of something like the GameCube (powerpc) is slow. 
Furthermore, due to hardware irregularities that programs relied on may take hundreds of instructions to emulate
If actually a simple translation, then should run close to native speed. This is reality of emulating hardware with hardware


creative commons is a set of licenses that make explicit requirements for users, e.g. CC BY-NC, CC BY-SA, etc.
unlike MIT, Apache includes a user non-litigation, so users can't initiate litigation if the creators decide to patent something

240V/50Hz mains.
oscilloscope default noise is mains (200MHz, 1Gsamples/sec as oppose to multimeter which is maybe 10samples/sec so really only applicable for perhaps a logic gate or 0.1hz square wave)
can debug PWM, I2C
Ensure BNC connector is plugged in correctly (affect probe compensation; similar to banana plugs in multimeter not being plugged in correctly)
Continous triggering enables us to view from start point, i.e. static image not free flowing. Will show before and after trigger point, i.e. start in centre of screen
No real issue with oscilloscope blowing up if testing battery powered/isolated DC power supply.
With USB powered, ground must be on ground! Otherwise will short USB and that port will probably break

Normal-mode triggering the best of both worlds

Using RS232 (recommended standard) decoder functionality (there is also SPI/I2C decoding).
https://www.youtube.com/watch?v=SarsWOCMvjg&t=76s
Also investigate PWM  

IMPORTANT: ensure offset dials are correct first, i.e. at 0 so 0 is centre
with menu, end-arrows can still be pressed even if not visible
change to 24mega points for memory when just wanting wave length (not zooming in?)

math -> decoder on; event table on (make sure zoomed out enough to view multiple packets (this will increase memory automatically?); increase baud also)

single-shot triggering contact bounce first then ringing as stablises (makes it a balancing act between selecting triggering level for button press and release)

Multimeter measure power consumption of MCU? (stm32 nucleo boards have convenient IDD jumper)

verify signal ringing (e.g. clock signal), i.e. inspect ramp-up/down (measure time to completely bottom out)

As well as headers, just insert hook onto a male-male wire

algorithm interview questions important not to belittle question

computer science papers
https://blog.acolyer.org/

QUESTIONS:
Understand memory alignment and cost of unaligned accesses? (is it due to common programmer workflows and less transistors required?)

Understand cycle hits for accessing harddrive?
Are cache cycle timings cumulative?

May compile for different architectures in embedded for different product lines e.g. low-end fit bit, high-end fitbit

Compile with different compilers to see performance benefits at end.
Also possible may have to use a particular compiler for specific hardware.

In reality, don't want an RTOS if timing very critical.
Most MCU don't even have multiple cores so adding software overhead (or are there additional hardware to emulate cores?)
Even without RTOS, will most likely have some timer and separation of tasks.
Main overall benefit of RTOS is consistent driver interfaces across multiple mcus (useful for say complex bluetooth/ethernet stacks etc.)
Most mcus are overpowered for what they do, so using an RTOS is probably a good idea.


tamper response usually done with a button on the board that gets activated when case opens
this will trigger an interrupt handled by RTC (real time clock)

cpu has modes like Stop mode that is a power saving mode.


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

security so vast and not something I want to devote time to:
https://leveleffect.referralrock.com/l/JOHNHAMMON07/


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

Borrowing money: clear you're the right person to give the money to. Clear you understand what you're doing and the game process is figured out. Proof of you will complete it. 
Here is the game, why I'm good at it, why people will like it and why I'm going to succeed at developing it
Interview ability to explain problems you have encountered on projects
