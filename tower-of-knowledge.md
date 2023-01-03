# IMPORTANT: FOR ALL TOPICS INCLUDED, HAVE SOME MENTION ON WALLPAPER

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
Doesn't allow instruction reordering, so doesn't maximise hardware speed)
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
DRAM refreshed periodically. SDRAM (synchronises clock speed with memory speed).
SDRAM. LPDDR4 (low-power; double pumping on rising and falling edge of clock)

SRAM more expensive, faster, not refreshed, larger die size.

DIIM (dual in-line memory module) is form factor (wider bus)
SODIMM (small outline)

Column Address Strobe (CAS), or CL (CAS latency) is time between RAM controller
read and when data is available

so, ram frequency gives max. throughput
however latency of ram also important

RAM potentially hundreds of cycles (CAS latency and cache retrieval process)
(important to note that RAM CAS is only say 20% of total latency as will 
check traverse cache and than copy it to cache)

(Serial Advanced Technology Attachment)
storage devices: form factor (M.2 keying, PCIe), interface (SATAIII, NVMe, PCIe), technology
a single form factor may support multiple interfaces, so ensure motherboard has
appropriate chipset

important that these are sustained speeds, 
so for small file sizes expect a lot less as seconds smaller
note that storage space advertised with S.I units, whilst OS works with binary

SATA SSD is the lowest grade ssd (however still 4 times bandwidth)



NUMA node relationship between CPU socket (location on motherboard) and memory banks.
So, say 2 sockets will probably have 2 NUMA nodes.
Therefore, not all physical memory directly accessible from 1 cpu socket;
will have to go through other socket to get it

Each CPU socket have memory banks that are local to it, i.e. can be accessed from it directly.
NUMA (non-uniform memory access) means that accessing memory from a non-local bank will not
be the same speed. A NUMA-aware OS will try to mitigate these accesses

RAID is method of combining multiple disks together so appear like one disk called an array.
Various types, e.g. RAID0 (striping) some parts of file in multiple disks, 
RAID1 (mirroring) each disk is dusplicate so could give speed increase etc.

PCI usually for attaching peripherals to motherboards, 
e.g. network/audio/usb/graphics controller cards

SRAM is fast, requires 6 transistors for each bit.
So, for 64MB cache, 3.2billion transistors. Sizeable percentage of die-area
DRAM is 1 transistor per bit

Petrol cars still use lead-acid as they have lower internal resistance and 
so can give higher peak current then equivalent lipo (just not for as long)

LiPo  is lithium-ion poly. uses polymer electrolyte instead of liquid.
lipo more expensive, shorter lifespan, less energy, more robust 
li-poly battery is rechargeable (so rechargeable is structured to allow a current to be passed to it to reverse the process)
battery will have two electrodes, say lithium cobalt oxide (cathode) and graphite (anode, negative)
graphite is almost always used as anode
when charging, lithium ions move to graphite
when discharging, lithium ions move from graphite to lithium
so, we can see that the atomic structure of the electrodes are changed, 
hence why charge cycles affect battery life

Lithium-ion has higher energy density, cheaper, not available in small sizes 
and more dangerous due to liquid electrolyte

Battery 51Watt/hr, which is A/hr * V is not a fixed value, e.g. 1A/hr could run 0.1A for 10 hours 

NAND and NOR flash are two types of non-volatile memory
NOR has faster read, however more expensive per bit and slower write/erase
NOR used in BIOS chips (firmware will be motherboard manufacturer, e.g. Lenovo)
A NAND mass storage device will require a controller chip, i.e. a microcontroller
How the controller accesses the NAND flash (i.e. how its Flash Translation Layer operates), 
will determine what type of storage it is:
* SD (secure digital)
* eMMC (embedded multimedia card): Typically SD soldered on motherboard
* USB (universal serial bus) 
* SSD (solid state drive): Parallel NAND access, more intelligent wear leveling and block sparring

QSPI can be used without CPU with data queues

(audio visual) HDMI-A, C (mini), D (micro)
USB-A,USB-B,USB-B(mini) 
USB-C is USB3.0

3.5mm audio jack (3 pole number of shafts (internal wires); 4 pole for added microphone)

ethernet CAT backwards compatible. connector called RJ45 
telephone cable is called RJ11

IEC power cords (kettleplug, cloverleaf)

DC barrel jack

Touch screen types need some external input to complete circuit
Resistive works by pressure pushing down plastic<-electric coating->glass
Unresponsive, durable, cheap (e.g. atm machine)
Capacitive how a grid of nodes that store some charge, i.e. like a capacitor
When our finger touches (we are good conductor due to impure ion water in us), 
charge flows through us and back to the phone, changing the electric current
Things electrically similar to our fingers are sausages, banana peels

1080i/p
interlaced means display even and odd rows for each frame (due to bandwith, not used anymore)
progressive will display each row sequentially for a given frame

4k means horizontal resolution of approximately 4000 pixels. standard
different for say television and projection industry

plasma is superheated matter, i.e. ionised gas

LED is the backlight, as oppose to fluorescent (LCD still used within this).
so really LED LCD. IPS (in-plane switching), TFT (thin film transistor) 
are examples of LCD panel technology
OLED produces own light, i.e. current passed through an OLED diode to produce light. LTPO (low-temperature polycrystalline oxide)

HDR (high dynamic range) ability to show contrasting colours. 
Also have XDR (extreme dynamic range)

nit is a measure of luminance, i.e. intensity of light (brightness is how we percieve luminance ∴ subjective).
higher nit display are more easily viewable in a wider array of lighting conditions (e.g. combat the sun's light reflecting off the surface)

e-ink less power than LCD as only uses power when arrangment of colours changes



# Dalgo

# Networks
RFC documents contain technical specifications for Internet techologies, e.g. IP, UDP, etc.

# Rendering
shader is a GPU program that is run at a particular stage in the rendering pipeline
Nvidia GPU cores named CUDA cores. AMD calls them stream processors
So, CUDA is a general purpose Nvidia GPU program that can utilise GPU's highly parallised architecture

font-rendering with stb: 
https://todool.handmade.network/blog/p/8561-rendering_glyphs_from_a_storage_buffer#26937

# Programmer
(sign 1bit)-(exponent 8bits)-(significand/mantissa 23bits)
1 *     2² *      0.1234

Data deduplication means to remove duplicates

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

## Wireless
SMS are stored as clear text by provider
SS7 (signaling system number 7) protocol connects various phone networks across the world
This protocol is old and has been attacked many times

leaked emails from large companies, probably from internal mail server? 

cloudflare (what are they?) ban kiwi farms forums, so they move to Russian servers.
how does this help?

RFID (radio frequency identification) no line of sight required 
(can read through objects unlike barcode) 
Also read multiple objects at once. RFID tag. Greater read range (shop security)
A QR code is just a 2d barcode with more bandwidth. Laser readers quicker.
NFC for low-power data transfer. NFC tag. (payment systems)

TV standards
Americas: NTSC (30fps, less scanlines per frame) 4.4MHz
Europe, Asia, Australia: PAL (Phase alternate line) (25fps) 2.5MHz

sound waves 20Hz - 20kHz (sonar; sound navigation and ranging)
ultrasonic (radio waves largely absorbed in seawater due conductiveness)

more easily focused as longer wavelength to not bend around corners
radio waves 10Hz - 300GHz (true radio would be FM radio)
higher frequency means more information can be conveyed
radio waves bounce off ionosphere (long range)

microwaves make up majority of the spectrum of radio waves
microwaves 300MHz - 300GHz (radar)
divided into bands, e.g. C-band, L-band, etc.

infrared waves 300GHz - 300THz (object detection)
heat emission

visible light (lasers; weather dependent) 
lidar

UV

ionising x-rays

ionising gamma-rays

ISM (industrial, scientific and medical) bands exclude telecommunication frequencies

4G (generation) type of cellular technology. define min/max upload/download rates
as many cell towers cannot fully support the bandwidth capabilities outlined by
4G, use the term 4G LTE (long term evolution) to indicate implement some of 4G features.
now have 4G LTE cat 13 to indicate featurs implemented
around 1800MHz

does version 5.0 expand upon enhanced data rate, i.e. bluetooth classic bandwidth? 
seems higher that higher than 6GHz requires license?
ISM range? (why bluetooth considered part of ISM?)

GNSS (Global Navigation Satellite Systems) constellations (GPS:US, GLONASS:Russia, Galileo:EU, Beido:China)
all location providing services. implement different frequencies, etc.

lower frequency, longer range

telecommunications standards:
* GSM (global system for mobile communications) - uses SIM (subscriber identification module) cards to authenticate and authorise access
I suppose eSIM would make SIM-locking easier to overcome 
Phone number linked/contained within SIM, however can be reassigned
* CDMA (code division multiple access) - uses ESN (electronic serial number) 

Udp (head of line blocking) +
client server (p2p unreliable as internet path optimised for cost/closest exchange point) 
+ dedicated (peoples home Internet don't normally have high upload rates); Mix of cloud (flexible to just turn up and down, high egress bandwidth charge) and bare metal (fixed bandwidth rate set into price) 
Matchmaking,  host migration difficult as hard to measure what user has good connection,  
e.g. Whats there NAT type?




## ARM
cortex-a53 is a synthesisable IP core sold to other semiconductor companies (stm, nxp, etc.)
as licensed IP core, the amount of cache will vary e.g. 8-64Kb?

FPU is a VFPv3-D16 implementation of the ARMv7 floating-point architecture

details hidden due to IP nature, e.g. LPDDR3 memory controller from CPU is all we can know?



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

# MCU
On startup, copy from Flash to RAM then jump to reset handler address
No real need for newlib, just use standalone mpaland/printf
Some chips have XIP (execute-in-place) which allows for running directly from flash 

QI is a wireless charging standard most supported by mobile-devices for distances up to 4cm
FreePower technology allows QI charging mats to support concurrent device charging

# Phone
Procedure Call Standard for the Arm Architecture (AAPCS).
Part of ABI for ARM (also part of ABI is debugging, dynamic linking semantics etc.)
From this we can garner say, not to put more than 4 word sized arguments to
function as this will require stack, consuming more time and space
Furthermore, alignment restrictions mean that passing a double word has to be even-odd register pair,
so ordering of parameters important
So, call standard important for ordering and number of parameters?

more instructions required for unaligned memory accesses?
(most modern x64 arm will not crash on an unaligned access?)
from armv7, unaligned accesses allowed

# Wearable
5ATM is 5 atmospheres. 1 atmosphere is about 10m (however calculated when motionless)
50m for 10 minutes
