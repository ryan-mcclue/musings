# IMPORTANT: FOR ALL TOPICS INCLUDED, HAVE SOME MENTION ON WALLPAPER

# Local
Sap is a fluid that transports nutrients throughout tree
Gum trees named as sap is gum like, as oppose to say resin like. 
Will also typically be smoothed bark
Eucalyptus type of gum tree, endemic (localised but consistent) to Australia
Eucalyptus oil droplets from the forests combine with water vapour to scatter short wavelength
rays of light that are predominantly blue (ROYGBIV in descending wavelengths)

Australia 5 time zones:
Christmas Island (UTC+7), Perth (UTC+8), Adelaide (UTC+10:30), Canberra (UTC+11), Norfolk Island (UTC+12)

Daylight savings incorporates the literal increase in sunlight to timezone 
So in Spring, the clock springs forward.
We lose an hour, i.e 23 hours in that day
In the Fall, the clock falls back.
We gain an hour, i.e 25 hours in that day

When using daylight saving time, will be AEDT as oppose to AEST, i.e. different time zones
Adelaide, south-east onwards observe daylight savings
WA not observed as large part of state is close to tropics, negating effect of tilt of earth

Gregorian calendar (Pope Gregory) does not correspond exactly to one solar year. 
So, every 4 years Feburary has an extra day (28 to 29) known as a leap year.
Also have leap seconds.
Gregorian calendar primarily used, however Chinese calendar has each month start on a new lunar phase

Solar eclipse when sun is eclipsed by moon
Lunar eclipse when moon is eclipsed by earth
New moon waxes and full moon wanes as it orbits about 27 days 

Prime meridian is through Greenwich, England.
Longitude is vertical lines, indicating east or west from prime meridian.
International Date Line is when you cross over 180° longitude, i.e. from UTC+12 (east) to UTC-12 or vice versa
Latitude is horizontal from equator.
Places on the equator have equal time of daylight and night time
Tropics are at 23.5°. They are at this amount as this is how much they are tilted from the equator
North line is cancer, south line is capricorn.
The tropics are the region between the tropic lines.
They are hot, due to the sunlight they recieve all year round
So, tropics are actually hotter than equatorial regions
Northern Artic and Southern Antarctic are latitude circles. 
They represent what areas of sunlight will hit.
So in summer, will have 24 hour days, whilst in winter 0hour days.

Hot air has lower pressure, and so rises. 
As higher pressure moves to low pressure, colder denser air will fill its place. 
This is wind
Wind direction from where it originates
Trade winds are winds that reliably blow from the east (easterlies) just north and south of equator 
As tropics hotter, higher pressure flows to equator. As earth rotates, wind is eastwards
Warmer surface water is thusly transported eastwards across Pacific.
Colder, nutrient rich water travels to ocean surface
La nina (warmer water near Australia, leading to higher evaporation and more rainfall; possible coral bleaching) <->
neutral <-> El nino (Spanish little boy; higher temperatures as clearer skies)

Along the moon line, there is a very slight pull to the moon.
The resultant force vectors around the earth are directed towards the centre.
This increases water pressure, and results in oceans being squeezed.
Similar to squeezing a balloon, get bulges at either side.

Solstices mark the astronomical (as oppose to calendar meteorological) start of summer (sun reaches highest point in sky; longest daylight of year) and winter.
Equinox is start of spring and autumn (equal number of daylight and night time)

Timestamp is calendar and time of day and UTC (Coordinated Universal Time) offset

Earth rotates eastward, so sun rises in the east no matter what hemisphere.

# Desktop
## Chips
CPU contains a clock.
Each tick marks a step in the fetch-decode-execute cycle.
The signal will be sent along the address bus as specified by program counter and 
instruction or data will be returned along the data bus.

Von-Neumann has instructions and data share address space.
The Von-Neumann bottleneck occurs when having multiple fetches in a single instruction, e.g. ldr
Harvard has instructions and data with separate address spaces
In reality, all CPUs present themselves as Von-Neumann to the user, however for efficiency
they are modified Harvard at the hardware level, i.e. pipeline/cache stage.
Specifically, will have separate L1 cache for instructions and data.
(also have uOP cache considered L0)
Therefore, when an architecture is described as Harvard, almost certainly modified Harvard.

Endianness only relevent when interpreting bytes from a cast
Can really only see usefulness of Big Endian for say converting string to int
Little Endian makes recasting variables of different lengths easier as starting address does not change

Although the instruction set supports 64bits, many CPUs address buses 
don't support entire 16 exabytes.
As we have no need for 16 exabytes (tera, peta, exa), the physical address size may be
39bits, and virtual size 48bits to save on unused transistors.

Direct-mapped cache has each memory address mapping to a single cache line
Lookup is instantenous, however high number of cache misses.
Fully-associative cache has each memory address mapping to any cache line.
The entire cache has to be searched, however low number of cache misses.
Set-associative cache divides cache into fully-associative blocks.
An 8-way cache means the number of cache lines in a block.
This is best in maintaining a fast lookup speed and low number of cache misses.

Cache sizes will stay relatively small due to the nature of how computers are used.
At any point, there is only a small amount of local data the CPU will process next.
So, beyond the empirical sweet spot of approximately 64MB, 
having a larger cache will yield only marginal benefits for cost of SRAM and die-area, 
i.e. law of diminishing returns.

Check if in L1. If not go check in L2 and mark least recently accessed L1 for
move to L2. Bubbles up to L3 until need for memory access which will go to memory
controller etc.

Alignment ensures that values don't straddle cache line boundaries.

CISC gives reduced cache pressure for high-intensive, sustained loops as less instructions required.
Instructions will have higher cycle count.
Also typically a register-memory architecture, 
e.g. can add one value in a register and one in memory together (as oppose to load-store)

TDP (thermal design power) maximum amount of heat (measure in watts) at maximum load
that is designed to be dissipated (bus sizes of chips smaller, so TDP getting lower)
However, value is rather vague as could be measured on over-clock and doesn't take into account ambient conditions

Clock frequency will often be changed by OS scheduler in idle moments or thermal throttling

Hardware scheduler allows for hyperthreading which is the sharing of execution units.
Therefore, hyperthreading not a boon in all situations.
AMD refers to this as simultaneous multithreading.

Microarchitecture will affect instruction latency and throughput 
byway of differening execution and control units.
For Intel CPUs, i3-i7 of same generation will have same microarchitecture.
Just different cache, hyperthreading, cores, die size etc. 

Codec is typically a separate hardware unit that you interact with via a specific API
HEVC (H.265; high efficiency video coding) newer version of H.264
VP9 is a open source Google video coding format

When people say vector operations, they mean SIMD.
SSE registers are 128bits (4 bytes) XMM, AVX are 256bits (8 bytes) YMM

Average CPU die-size is 100mm².
GPU much larger at 500mm² as derives more benefits from more control units, i.e. parallelisation
Common transistor size is 7nm. Low as 2nm
Silicon atom is 0.2nm
Gleaning a Moore's law transistor graph, see that average CPU a few billion transistors
and high end SOCs around 50 billion transistors.

Memory model outlines the rules regarding the visibility of changes to data stored in memory,
i.e. rules relating to memory reads and writes 
A hardware memory model relates to the state of affairs as the processor executes machine code:
* Sequential Consistency: 
Doesn't allow instruction reordering, so doesn't maximise hardware speed
* x86-TSO (Total Store Order):
All processors agree upon the order in which their write queues are written to memory
However, when the write queue is flushed is up to the CPU
* ARM (most relaxed/weak)
Writes propagate to other processors independently, i.e not all update at same time
Furthermore, the order of the writes can be reordered
Processors are allowed to delay reads until writes later in the instruction stream

A software memory model, e.g a language memory model like C++ will abstract over
the specific hardware memory model it's implemented on.
It will provide synchronisation semantics, e.g. atomics, acquire, release, fence etc.
These semantics are used to enforce sequentially consistent behaviour when we want it.
However, using intrinsics, we can focus only on hardware memory model.

A cache controller implements cache coherency by recording states for each cache line.
MESI is baseline used. Has states:
* Modified: Only in this cache and dirty from main memory
* Exclusive: Only in this cache and clean from main memory
* Shared: Clean and shared amongst other caches 
* Invalid
Intel uses MESIF (Forward same as shared except designated responder), 
while Arm uses MOESI (Owned is modified by possibly in other caches)
Cache coherency performance issues are difficult to debug, 
e.g. one value changed in cache line invalidates it, even though another value in cache line
remains unchanged

## Legalities
Anti-trust laws don't prevent monopolies, they prevent attempts to monopolise by 
unfair means, e.g. Microsoft browser market, Apple app store etc.

Technically, any digital work created is automatically protected by copyright. 
So, without a license, people would have to explicitly ask for permission to use

Permissive (MIT, BSD, Apache, zlib) gives users more freedom to say relicense, 
include closed source software, etc.
They generally just enforce attribution
Apache like MIT except must state what files you have changed 

Weak copyleft (LPGL) applies to files of library not your entire codebase, 
i.e. must still release your version of the library used
So, dynamic linking makes this easier for keeping your source closed
If statically linking, must make a few extra steps to ensure the LGPL parts are available, 
e.g. publish object files

Copyleft (GPL) enforces the developers usage of the code.
So, any derivative software must release whole project as GPL, i.e infects your software (and restricts choice of libraries to GPL)
Subsequently encounter more licensing restrictions.

Creative commons licenses are composed of various attributes. The default is attribution.
They are typically used in artworks, e.g. images, audio files
Other elements are optional and can be combined together, 
e.g no derivative, no financial, must share under same license.

Public domain means no license, so could claim as yours

## Bootstrap 
UUID/GUID (universally/globally) 16 bytes. 
Typically generated by concatenating bits of MAC address and timestamp

UEFI firmware interface made to standardise interface between OS and firmware 
for purposes of booting

Called /dev/sd as originally for SCSI 
(small computer system interface; standards for transferring data between computers and devices) 
The preceding letter indicates the order in which it's found, e.g. /dev/sda first found 
The preceding number indicates the partition number, e.g. and /dev/sda1, /dev/sda2

UEFI use of GPT (GUID partition table) incorporates CRC to create more 
recoverable boot environment
over BIOS MBR (located in first sector of disk)
Furthermore, UEFI has more addressable memory in 64-bit mode as oppose to only 16-bit mode
Also, UEFI supports networking
The ESP (EFI system partition) will have EGI entries that point to a UUID of where to boot
one of these will be grub binary like shimx64.efi
NOTE: The bootloader is the EFI OS loader and is part of the OS that will load the kernel

ACPI interface to pass information from firmware to OS.
This firmware will have hardware information baked into it set by manufacturers 

## Formats 
Inodes store file metadata.
The metadata stored by an inode is determined by filesystem in use, 
except for filename which is never stored 
Typical metadata includes size, permissions, data pointer
NOTE: FAT32 won't store permissions, last modification time, no journaling or soft-links

A filename maps to an inode.
Therefore a directory is a mapping of filenames to inodes

A hardlink references an inode, and is therefore impervious to file name change, deletion, etc.
A softlink is to a file name

Journaling is the process of regularly writing operations that are to be performed in RAM 
to disk area of memory known as journal.
then apply these changes to disk when necessary
this overhead makes them slower, but more robust on crashes as can read journal to ascertain
whether certain operations finished performing

FAT for ESP (because FAT simple, open source, low-overhead and supported virtually everywhere)
vFAT is driver (typically for FAT32)

EXT4 for system (supports larger file sizes)
(NTFS microsoft proprietary)
Most filesystems will use a self-balancing tree to index files

## Unix
Ubuntu distro as compared to debian more user friendly.
For example, automatically includes proprietary drivers like WiFi, 
has PPAs to allow installation of 3rd party applications,
and install procedure just works.
Also updates more regularly than Debian and provides LTS, so know regular backports provided

Linux is a monolithic kernel, i.e. drivers, file system etc. are all in kernel space.
So, more efficient, not as robust to component failure
Windows uses a hybrid kernel, moving away from original microkernel due to inefficiencies
Using Ubuntu generic kernel (could also have -lowlatency etc.) to not include a lot of
modules in kernel to free up RAM usage

Xfce as default Ubuntu GNOME had bug with multiple keyboards. 
Furthermore, Xfce codebase was readable when inspecting X11 code.
In addition, Xfce automatically provided GUI shortcut creation

.deb and .rpm are binary packages. 
Annoyances arise due to specifying the specific library dependency for each distro version
Flatpaks and Snaps are containerised applications that include the specific libraries and runtimes
AppImages combine the 'shared' libraries and runtimes of Flatpaks and Snaps into a giant file. 
This file can be copied and run on any distro
If packages is being actively maintained, preferable to use .deb as faster and simpler

Linux DRM (direct rendering manager) -> X11 (display server) -> xfce (desktop environment)  
Linux ALSA (advanced linux sound architecture) -> pulseaudio (sound server) 

Fstab (File System Table) describes filesystems that should be mounted, when they should be mounted and with what options.

SystemV ABI:
rdi, rsi, rdx, rcx, r8, r9 (6 integer arguments)
xmm0, xmm1, xmm2, xmm3, xmm4, xmm5, xmm6 (7 floating arguments)
Remaining arguments pushed right-to-left on stack
rax return and syscall number
Stack 16-byte aligned before function call 
SSE2 is baseline for x86-64, so make efficient for __m128

A premptive scheduler will swap processes based on specific criteria.
Round-robin means each process will run for a designated time slice
CFS is a premptive round-robin scheduler. 
Time slices are dynamic, computed like `((1/N) * (niceness))` 
Processes are managed using a RB-Tree. 
Therefore, cost of launching a process or a context switch is logarithmic
Kernel will have an internal tick rate that updates waiting threads.
Lowering this will increase granularity however will increase CPU time 
and hence battery time as more time spent in kernel code.
Windows scheduler uses static priorities, so one intensive process can dominate CPU

System level refers to inbetween kernel and userspace, e.g. network manager
Systemd is a collection of system binaries, e.g. udev
Primarily, systemd is a service manager
A service extends the functionality of daemons, e.g. only start after another service, restart on failure after 10s etc.
The kernel will launch systemd init service that will then bootstrap into userspace 
(hence alllowing for the aforementioned service features)

Kernel offers various methods of process isolation, e.g. chroot, cgroups etc.
(chroot cannot access files outside its designated tree)
A container will utilise one of these options provided by the kernel to acheive:
 * cannot send signals to processes outside container
 * has own networking namespace
 * resource usage limits

ELF (Executable and Linkable Format)
Header contains type, e.g. executable, dynamic, relocatable and entrypoint
A section is compile time:
* .text (code)
* .bss (uninitialised globals)
* .data (globals)
* .rodata (constants)
A segment is a memory location, e.g .dseg (data), .eseg (eeprom) and .cseg (code/program)

Typically stored as crt0.s, this will perform OS specific run-time initialisation.
The conditions assumed here will be outlined in ABI, e.g. argc in rdi
Some functions include setting up processor registers (e.g. SSE), MMU, caches, threading
ABI stack alignment, frame pointer initialisation, C runtime setup (setting .bss to 0),
C library setup (e.g. stdout, math precision)
The program loader will load from Flash into RAM then call _start (which is crt entrypoint)

## Components
Storage device sizes are advertised with S.I units, whilst OS works with binary so
will show smaller than advertised (1000 * 10³ < 1024 * 2¹⁰)
Also, storage device write speeds are sustained speeds.
So, for small file sizes expect a lot less

A flip-flop is a circuit that can have two states and can store state.
Various types of flip-flops, e.g. clock triggered, data only etc.
A latch is a certain type of flip-flop.
Called this as output is latched onto state until another change in input.

Registers and SRAM stored as flip-flops. 
DRAM is a single transistor and capacitor

SRAM (static) is fast, requires 6 transistors for each bit.
So, 3.2billion transistors for 64MB cache. Sizeable percentage of die-area
SRAM more expensive, faster as not periodically refreshed.

DRAM (dynamic) is 1 transistor per bit refreshed periodically.
SDRAM (synchronises internal clock and bus clock speed).
SDRAM. LPDDR4 
(low-power; double pumping on rising and falling edge of clock, 
increasing bus clock speed while internal typically stays the same, amount prefetched etc.)


DIIM (Dual In-Line Memory Module) is form factor with a wider bus
SODIMM (Small Outline)

CAS (Column Address Strobe), or CL (CAS latency) is time between RAM controller
read and when data is available.
RAM frequency gives maximum throughput, however CL affects this also.
In addition, RAM access is after cache miss, so direct RAM latency is only a percentage
of total latency as time taken to traverse cache and copy to it.

NAND and NOR flash are two types of non-volatile memory
NOR has faster read, however more expensive per bit and slower write/erase
NOR used in BIOS chips (firmware will be motherboard manufacturer, e.g. Lenovo)
A NAND mass storage device will require a controller chip, i.e. a microcontroller
How the controller accesses the NAND flash, i.e. the protocol under which its 
Flash Translation Layer operates, will determine what type of storage it is:
* SD (secure digital)
* eMMC (embedded multimedia card): Typically SD soldered on motherboard
(For SD/MMC protocol, will have a RCA, i.e. Relative Card Address for selecting card)
* USB (universal serial bus) 
* SSD (solid state drive): Parallel NAND access, more intelligent wear leveling and block sparring
3D VNAND (Vertical) memory increases memory density by vertically stacking NAND flash

Form factors include M.2 keying and PCIe (Peripheral Component Interconnect)
Interface includes SATAIII, NVMe (non-volatile memory host controller) and PCIe
SATA (Serial Advanced Technology Attachment) SSD is the lowest grade SSD.
A single form factor may support multiple interfaces, so ensure motherboard has
appropriate chipset

Each CPU socket has memory banks that are local to it, i.e. can be accessed from it directly.
NUMA (non-uniform memory access) means that accessing memory from a non-local bank will not
be the same speed. A NUMA-aware OS will try to mitigate these accesses.

RAID (redundant array of independent disks) is method of combining multiple disks 
together so as to appear like one disk called an array.
Various types, e.g. RAID0 (striping) some parts of file in multiple disks, 
RAID1 (mirroring) each disk is duplicate so could give speed increase etc.

Battery will have two electrodes, say lithium cobalt oxide and graphite.
When going through a charging/discharging cycle, ions move between electrodes.
So, charging cycles will affect the atomic structure of the electrodes and hence
affect battery life.

Circuits based on conventional current, i.e. + to -
Cathode is terminal from which conventional current flows out of, i.e. negative

LiPo (lithium-ion polymer) uses polymer electrolyte instead of liquid.
Standard lithium-ion has higher energy density, cheaper, not available in small sizes 
and more dangerous due to liquid electrolyte
LiPo more expensive, shorter lifespan, less energy, more robust 
LiPo battery is structured to allow a current to be passed to it to 
reverse the process of oxidation (loss of electrons), i.e. is rechargeable

Battery 51Watt/hr, which is A/hr * V is not a fixed value, 
e.g. 1A/hr could run 0.1A for 10 hours 

Petrol cars still use lead-acid as they have lower internal resistance and 
so can give higher peak current then equivalent LiPo (just not for as long)

HDMI(High Definition Multimedia Interface)-A, C (mini), D (micro) carry audio and visual data
DisplayPort has superior bandwidth to HDMI

USB-A,USB-B,USB-B(mini)
USB-C is USB3.0

3.5mm audio jack (3 pole number of shafts (internal wires), 4 pole for added microphone)

Ethernet CAT backwards compatible. 

Telephone cable called RJ11

IEC (International Electrotechnical Commission) power cords used for 
connecting power supplies up to 250V, e.g. kettleplug, cloverleaf

DC barrel jack

Touch screen types need some external input to complete circuit
Resistive works by pressure pushing down plastic<-electric coating->glass
Unresponsive, durable, cheap
Capacitive contains a grid of nodes that store some charge.
When our finger touches charge flows through us and back to the phone, 
changing the electric current read. 
We are good conductor due to impure water ion in us.
So, things electrically similar to our fingers will work also like sausages, banana peels

1080i/p
1080 references vertical height in pixels
Interlaced means display even and odd rows for each frame.
Due to modern high bandwith not used anymore.
Progressive will display each row sequentially for a given frame

4k means horizontal resolution of approximately 4000 pixels. 
standard different for say television and projection industry, e.g. 3840 pixels

Screen density is a ratio between screen size and resolution measured in PPI (Pixels Per Inch) 

A voltage applied to ionised gas, turning them into superheated matter that is plasma.
Subsequent UV is released.

LCD (Liquid Crystal Display) involves backlight through crystals.
IPS (In-Plane Switching) and TFT (Thin Film Transistor) are example crystal technologies.
For an LED monitor, the LED is the backlight, as oppose to a fluorescent.
However still uses LCD backlight, so really LED LCD.

Quantum science deals with quanta, i.e. smallest unit that comprises something
They behave strangely and don't have well defined values for common properites like position, energy etc., e.g. uncertainty principle
A 'quantum dot' is a semiconductor nanoparticle that has different properties to larger particles as a result of quantum mechanics.
QLED/QNED (Quantum NanoCell) adds a 'quantom dot' layer into 
the white LED backlight LCD sandwich.

OLED is distinct. 
It produces own light, i.e. current passed through an OLED diode to produce light. 
LTPO (Low Temperature Polycrystalline Oxide) is a backplane for OLED technology.

E-ink display uses less power than LCD as only uses power when arrangment of colours changes.

HDR (High Dynamic Range) and XDR (Extreme Dynamic Range) increase ability
to show contrasting colours. 

5.1 means 5 speakers, 1 subwoofer
In order of ascending levels of audible frequencies 20Hz-20000Hz have devices
woofer, subwoofer, speaker and tweeter.

# Phone
ARM core ISA e.g. ARMv8
Will then have profiles, e.g. M-Profile ARMv8-M 
R-Profile for larger systems like automotive

ARM holdings implements profile in own CPU, e.g. Cortex-A72
This is a synthesisable IP (Intellectual Property) core sold to other 
semiconductor companies who make implementation decisions like 
amount of cache from 8-64kb specification.

However, other companies can build own CPU from ISA alone, e.g. Qualcomm Kryo and Nvidia Denver

Then have actual MCUs e.g. STMicroelectronics, NXP
or SoCs e.g. Qualcomm Snapdragon, Nvidia Tegra 

big.LITTLE is heterogenous processing architecture with two types of processors.
big cores are designed for maximum compute performance and LITTLE for maximum power efficiency

FFT divides samples (typically from an ADC) into frequency band
A logarithmic scale typically employed to account for nonlinear values whereby a greater proportion in high frequency band

DSP instructions may include transforms like FFT, filters like IIR/FIR (Finite Impulse Response; no feedback) and statistical like moving average

With the addition of 64bit extension, ARM retroactively called aarch32.
Possible instruction sets include Thumb-1 (16-bit), Thumb-2 (16/32-bit), aarch32 (32bit instructions), aarch64 (32bit instructions), 
Neon, MTE (Memory Tagging Extension), etc. 
aarch64 does not allow Thumb instructions

FPU is a VFPv3-D16 implementation of the ARMv7 floating-point architecture
VFP (Vector Floating Point) is floating point extension on ARM architecture.
Called vector as initially introduced floating point and vector floating point.
Neon is product name for ASE (Advanced SIMD Extension), i.e. SIMD for cortex-A and cortex-R 
(more recent is SVE (Scalable Vector Extension))
Helium is product name for MVE (M-profile Vector Extension), i.e. SIMD for cortex-M

RSA (Rivest-Shamir-Adleman) is asymmetric, i.e. public and private key. Much slower than AES
AES (Advanced Encryption Standard) is symmetric, i.e. one key
SHA (Secure Hash Algorithm) is one-way and produces a digest
OpenSSL is common open-source cryptography toolkit that implements these

MPU (Memory Protection Unit) only provide memory protection not virtual memory like an MMU (Memory Management Unit)

Built atop Android OS, many phones will implement own custom OS, e.g. Huewei EMUI, Samsung One UI
The ART (Android Runtime) is the Java Virtual Machine that performs JIT bytecode compilation of APK (Android Package Kit)

EABI (Embedded) is new ARM ABI, renamed as it suits the needs of embedded applications.
An EABI will omit certain abstractions present in an ABI designed for a kernel, e.g run in priveleged mode
From the calling convention part of ABI we can garner number of arguments until stack usage and alignment requirements.

AAPCS (Arm Architecture Procedure Call Standard):
r0-r3, rest stack
s0-s7
Stack 4-byte aligned, if on function call, 8-byte aligned

AAPCS64:
x0-x7 (w0-w7 for 32-bit), rest stack
v0-v7
Stack 16-byte aligned

Most modern ARM architectures will not crash on unaligned accesses

Shader is a GPU program that is run at a particular stage in the rendering pipeline
Nvidia GPU cores named CUDA cores. AMD calls them stream processors. ARM shader cores
So, CUDA is a general purpose Nvidia GPU program that can utilise GPU's highly parallised architecture
OpenCL whilst more supportive, i.e can run on CPU or GPU, does not yield same performance benefits
Renderscript is android specific heteregenous in that it will distribute load automatically
OpenGL has a lot of fixed function legacy (now shader based) and drivers rarely follow the standard in its entirety
OpenGL ES (Embedded Systems) is a subset
Vulkan is low-level that more closely reflects how modern GPUs work

Flux is an arbitrary term used to describe the flow of things, e.g. photon flux, magnetic flux

Lumens is how much total light is produced by an emitter
Candela is intensity of light beam produced by an emitter 
Lux is how much light hits a recieving surface
Nit is how much light is reflected off a surface and so is what our eyes and cameras pick up.
A display will be in nits as its a recieving object as oppose to the backlight LEDs
A higher nit display is more easily viewable in a wider array of lighting conditions,
e.g. will combat the sun's light reflecting off the surface in an outdoor setting
Brightness is subjective, and therefore does not have a value associated to it

## Waves
Sound Waves (20Hz-20kHz)
Ultrasonic are sound waves not audible by humans
SONAR (Sound Navigation And Ranging) used in maritime as radio waves largely absorbed in seawater due to conductiveness

Radio Waves (10Hz-300GHz) 
Microwaves make up the majority of the spectrum of radio waves (300MHz-300GHz)
They are divided into bands, e.g. C-band, L-band, etc.
RADAR (Radio Detecting And Ranging) encompasses microwave spectrum
Higher frequency results in higher resolution than SONAR.
Also as EM wave, much faster transmission rate.
Allowing for long range transmission, radio waves bounce off ionosphere (where Earth's atmosphere meets with space)

Infrared Waves (300GHz-300THz) 
Can be used in object detection.
Heat is the motion of atoms. 
The faster they move, the more heat is produced
Approximately 50% of solar radiation is infrared.

Visible light 
LIDAR (Light Detection And Ranging) 
(lasers; weather dependent) 
higher accuracy and resolution than radar, lower range

Ultraviolet
UVA has longer wavelength, associated with skin ageing
UVB associated with skin burning
UVB doesn't pass through glass, however UVA does
UVC is a germicide
SPF (Sun Protection Factor) is how many times longer it takes to burn than with no sunscreen.
However, UV can still get through and sunscreen is water-resistant, not waterproof

Ionising X-Rays

Ionising Gamma-Rays
Sterilisation and radiotherapy

## Wireless Protocols
ISM (Industrial, Scientific and Medical) bands (900MHz, 2.4GHz, 5GHz) occupy unlicensed RF band.
They include Wifi, Bluetooth but exclude telecommunication frequencies

GNSS (Global Navigation Satellite Systems) contain constellations GPS (US), GLONASS (Russia), Galileo (EU) and Beido (China)
They all provide location services, however implement different frequencies, etc.

GSM (Global System for Mobile Communications) uses SIM (Subscriber Identification Module) cards to authenticate (identity) and authorise (privelege) access

4G (Generation; 1800MHz) outlines min/max upload/download rates and associated frequencies.
Many cell towers cannot fully support the bandwidth capabilities outlined by 4G. 
As a result, the term 4G LTE (Long Term Evolution) is used indicate that some of the 4G spec is implemented.
More specifically have 4G LTE cat 13 to indicate particular features implemented.

SMS (Short Message Service) are stored as clear text by provider
SS7 (Signaling System Number 7) protocol connects various phone networks across the world

Between protocols, tradeoffs between power and data rate
IEEE (Institute of Electrical and Electronic Engineers):
* 802.11 group for WLANs (WiFi 6E - high data rate), 
* 802.15 for WPANs; 802.15.1 (Bluetooth), 
* 802.15.4 low data rate (ZigBee, LoRa, Sigfox, Z-Wave)

Wifi, Bluetooth, ZigBee, Z-Wave (lowest power) are for local networks.
LoRa is like a low bandwidth GSM
LoRa (Long Range) has low power requirements and long distance. AES-128 encrypted by default.
LoRa useful if only sending some data a few times a day.
LoRa has configurable bandwitdh, so can go up to 500KHz if regulations permit
Lower frequency yields longer range as longer wavelength won't be reflected off objects. Will be called narrowband
Doesn't require IP addresses.
LoRaWAN allows for large star networks to exist in say a city, but will require at least 1 IP address for a gateway
Sigfox uses more power.

A BLE (Bluetooth Low Energy) transceiver only on if being read or written to
GATT (Generic Attribute Profile) is a database that contains keys for particular services and characteristcs (actual data) 
When communicating with a BLE device, we are querying a particular characteristic of a service

A QR (Quick Response) code is a 2D barcode with more bandwidth. Uses a laser reader.
RFID (Radio Frequency Identification) does not require line-of-sight and can read multiple objects at once. 
Uses RFID tag. 
NFC (Near Field Communication) is for low-power data transfer. 
Uses NFC tag

TV standards
Americas: NTSC (30fps, less scanlines per frame) 4.4MHz
Europe, Asia, Australia: PAL (Phase alternate line) (25fps) 2.5MHz

## Sensors
MEMS (Micro Electro Mechanical Systems) combines mechanical parts with electronics like some IC, i.e. circuitry with moving parts.
e.g. microphone (sound waves cause diaphragm to move and cause induction), accelerometer, gyroscope (originally mechanical)

On phone, many sensors implemented as non-wakeup.
This means the phone can be in a suspended state and the sensors don't wake the CPU up to report data

Accelerometer measures rate of change in velocity, i.e. vibrations associated with movement (m/s²)
So can check changes in orientation.
It will have a housing that is fixed to the surface and a mass that can move about.
Detecting the amount of movement in the mass, can determine acceleration in that plane.

Gyroscope measures rotational acceleration, unlike an accelerometer which is unable to distinguish it from linear (rad/s²)
Gyroscope resists changes to its orientation due to intertial forces of a vibrating mass.
So can detect angular momentum which can be useful for guidance correction.

A gimbal is a pivoted support that permits rotation about an axis

IMU (Inertial Measurement Unit) is an accelerometer + gyroscope + magnetometer (teslas)
The magnetometer is used to correct gyroscope drift as it can provide a point of reference

Quartz is piezoelectric, meaning mechanical stress results in electric charge and vice versa.
In an atomic clock, Caesium atoms are used to control the supply of voltage across quartz.
This is done, in order to keep it oscillating at the same frequency.

NTP (Network Time Protocol) is TCP/IP protocol for clock synchronisation. 
It works by comparing with atomic clock servers

# Dalgo



# Programmer (for this, have wallpaper list projects)
(sign 1bit)-(exponent 8bits)-(significand/mantissa 23bits)
1 *     2² *      0.1234

PIC (Position Independent Code) can be executed anywhere in memory by using relative addresses, e.g. shared libraries
The process of converting relative to absolute, i.e. query unresolved symbols at runtime adds a level of indirection 

Data deduplication means to remove duplicates

TODO: testing procedures

REST (representational state transfer) is an interface that outlines how the state of a resource is interacted with
On the web, an URL is an access point to a resource
A RESTful API will have URLs respond to CRUD requests in a standard way:
* GET example.com/users will return list of resource, i.e. all users
* POST example.com/users will create a new resource
* GET example.com/users/1 return single resource
* PUT example.com/users/1 update single resource
* DELETE example.com/users/1 delete single resource

OAuth (open authorisation) is a standard that defines a way of authorising access
OAuth offers different functionality than SSH by having the ability to 'scope' access
Typically used by RESTful services
These endpoints described in 'discovery document':
1. authorisation-server -> authorisation-code
2. authorisation-code -> access token, refresh token
3. resource-server -> resource


RFC (Request For Comments) documents contain technical specifications for Internet techologies, e.g. IP, UDP, etc.

Udp (head of line blocking) +
client server (p2p unreliable as internet path optimised for cost/closest exchange point) 
+ dedicated (peoples home Internet don't normally have high upload rates); Mix of cloud (flexible to just turn up and down, high egress bandwidth charge) and bare metal (fixed bandwidth rate set into price) 
Matchmaking,  host migration difficult as hard to measure what user has good connection,  
e.g. Whats there NAT type?



# MCU
MOSFET type of transistor that is voltage controlled

CMOS technology allows the creation of low standby-power devices, e.g. non-volatile CMOS static RAM 


EMV (europay, mastercard, visa) chip implements NFC for payments


Various synthetic benchmarks indicative of performace, e.g. 
DMIPS (Dhrystone Million Instructions per Second) for integer and
WMIPS (Whetstone) for floating point


SPDIF (Sony Phillips Digital Interface) carries digital audio over a relatively short distance
without having to convert to analog, thereby preserving audio quality.

The polarity of the magnetic field created by power and ground wires will be opposite.
So, having the same position in each wire line up will reduce outgoing noise as superposition of
their inverse magnetic fields will cancel out. Furthermore, incoming noise will affect each
wire similarly
Coaxial has the two conductors share an axis with shielding outside.
Twisted pair wire is a cheaper way of implementing coaxial
Glass fibre optic does not have this issue.

ASIC (Application Specific Integrated Circuit) MCU for specific task 

* USART: 
* SPI:
* I2C:

On startup, copy from Flash to RAM then jump to reset handler address
No real need for newlib, just use standalone mpaland/printf
Some chips have XIP (execute-in-place) which allows for running directly from flash 

QI is a wireless charging standard most supported by mobile-devices for distances up to 4cm
FreePower technology allows QI charging mats to support concurrent device charging

QSPI can be used without CPU with data queues

Chrom-ART Accelerator offers DMA for graphics, i.e. fast copy, pixel conversion, blending etc.

LED anode is positive longer lead

5ATM is 5 atmospheres. 1 atmosphere is about 10m (however calculated when motionless)
50m for 10 minutes

MIDI (Musical Instrument Digital Interface) 3 byte messages that describe note type, how hard pressed and what channel
Useful for sending out on MCU
FRAM (ferroelectric) is non-volatile gives same access properties as RAM



RENDERING:
rendering is the process of creating the 2D/3D model, i.e. the drawing onto the monitor
ray tracing solves transparency issues, it's just substantially slower than standard projection rasterisation (so future is ray tracing)

3D works by emulating a simplified model of how a single human eye views space

A single point of light from the sun hitting a single object will reflect in multiple straight line directions/paths

rasterisation is taking polygon and converting to pixels
eventually will have to convert say 3D position to 2D (as all rendering is fundamentally this)

lens refract light to central focal point

1. perspective projection (assume viewer is some distance away from screen, similar triangles)

/dev/urandom is cryptographically secure pseudo-random number
/dev/zero

creating website: https://threadreaderapp.com/thread/1606219302855745538.html

TODO: what is blockchain and web3?


