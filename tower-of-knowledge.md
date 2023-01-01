# IMPORTANT: FOR ALL TOPICS INCLUDED, HAVE SOME MENTION ON WALLPAPER

creating website: https://threadreaderapp.com/thread/1606219302855745538.html

TODO: what is blockchain and web3?

# MCU
QI is a wireless charging standard most supported by mobile-devices for distances up to 4cm
FreePower technology allows QI charging mats to support concurrent device charging

# Phone
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


For intel CPUs, i3-i7 of same generation will have same micro-architecture.
Just more cache, hyperthreading, cores, die size etc. 


$(grub-)

Intel clock frequency is often changed by OS?

Is cortex-m4 microarchitecture? How does this relate to intel naming? AMD also?

also have uOP cache considered L0

$(getconf LEVEL1_DCACHE_LINESIZE)

$(sudo lshw -html > info.html) for RAM, harddrive and drivers of things
$(ldd --version) 
$(lsb_release -a), $(cat /etc/debian_version)

SATA ssd is the lowest grade ssd (however still 4 times bandwidth)

storage devices: form factor (M.2 keying, PCIe), interface (SATAIII, NVMe, PCIe), technology
a single form factor may support multiple interfaces, so ensure motherboard has
appropriate chipset

DRAM refreshed periodically. SDRAM (synchronises clock speed with memory speed).
SDRAM. LPDDR4 (low-power; double pumping on rising and falling edge of clock)

SRAM more expensive, faster, not refreshed, larger die size.

DIIM (dual in-line memory module) is form factor (wider bus)
SODIMM (small outline)



From $(cpuinfo) see that although 64bit cpu this is just what the instruction set supports.
As we have no need for 16 exabytes (tera, peta, exa), the physical address size may be
39bits, and virtual size 48bits to save on unused transistors

$(cpu-x) for cache information per core



NUMA node relationship between CPU socket (location on motherboard) and memory banks.
So, say 2 sockets will probably have 2 NUMA nodes.
Therefore, not all physical memory directly accessible from 1 cpu socket;
will have to go through other socket to get it

Hyperthreads don't increase number of instructions per second, rather number of instructions
that can be queued (so hyperthread like a queue)

Caches, are instruction and data caches combined?
What distinct features in a core, e.g. shared caches? 
What additional features on one capable of hyperthreading?

CPU could implement VT-x, but motherboard and bios must support this as well?
What additional features are present when CPU supports this?

Cache probably 8-way as compromise between lookup and copy speed.
Flow of cache, is it check if from 8-way copy then L2 8-way or finish L1 entirely?
If found, in L2 does it copy to L1?

Microarchitecture will affect instruction latency and throughtput by implementation of execution and control units

# Wearable
5ATM is 5 atmospheres. 1 atmosphere is about 10m (however calculated when motionless)
50m for 10 minutes

# Desktop
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

Called /dev/sd as originally for SCSI (small computer system interface; standards for transferring data between computers and devices) 
The preceding letter indicates the order in which it's found, e.g. /dev/sda first found 
The preceding number indicates the partition number, e.g. and /dev/sda1, /dev/sda2

UEFI use of GPT (GUID partition table) incorporates CRC to create more recoverable boot environment
over BIOS MBR (located in first sector of disk)
Furthermore, UEFI has more addressable memory in 64-bit mode as oppose to only 16-bit mode
Also, UEFI supports networking
The ESP (EFI system partition) will have EGI entries that point to a UUID of where to boot
one of these will be grub binary like shimx64.efi
NOTE: The bootloader is the EFI OS loader and is part of the OS that will load the kernel

ACPI interface to pass information from firmware to OS.
This firmware will have hardware information baked into it set by manufacturers 

Each CPU socket have memory banks that are local to it, i.e. can be accessed from it directly.
NUMA (non-uniform memory access) means that accessing memory from a non-local bank will not
be the same speed. A NUMA-aware OS will try to mitigate these accesses

Common transistor size is 7nm. Low as 2nm
Silicon atom is 0.2nm
From gleaning a Moore's law transistor graph, see that average CPU a few billion transistors
and high end SOCs around 50 billion transistors

TDP (thermal design power) maximum amount of heat (measure in watts) at maximum load
that is designed to be dissipated (bus sizes of chips smaller, so TDP getting lower)
However, value is rather vague as could be measured on over-clock and doesn't take into account ambient conditions

Cache eats up precious die-area. Having a large cache increases lookup time

Linux is a monolithic kernel, i.e. drivers, file system etc. are all in kernel space.
So, more efficient, not as robust to component failure
Windows in hybrid kernel, moving away from microkernel due to inefficiencies

SystemV ABI:
IMPORTANT: cannot easily access upper portion of register, only lower 
rdi, rsi, rdx, rcx, r8, r9 (6 integer arguments)
xmm0, xmm1, xmm2, xmm3, xmm4, xmm5, xmm6 (7 floating arguments)
remaining arguments pushed right-to-left on stack
rax return and syscall number
stack 16-byte aligned before function call (SSE2 is baseline for x86-64, so make efficient for __m128)?
(word size 8 bytes?)



CISC gives reduced cache pressure for high-intensive, sustained loops
Instructions with higher cycle count
Typically a register-memory architecture, e.g. can add one value in register, one in memory (as oppose to load-store)

SRAM is fast, requires 6 transistors for each bit.
So, for 64MB cache, 3.2billion transistors. Sizeable percentage of die-area
DRAM is 1 transistor per bit

Average CPU die-size is 100mm², GPU much larger as benefits from parallelisation around 500mm²

direct-mapped cache has each memory address mapping to a single cache line
lookup is instantenous, however high replacement inefficiences
fully-associative cache has each memory address mapping to any cache line
entire cache has to be searched, however low replacement inefficiencies
set-associative cache divides cache into blocks of fully-associative.
the number of cache lines in a block is what say an 8-way cache means
this compromises between the efficiency and lookup speed

Caches will stay relatively small due to the nature of how computers are used.
At any point, there is only a small amount of local data the CPU will process next.
So, beyond the sweet spot of 64MB, having a larger cache will marginally better hit ratio.

## Formats 
Inodes store file metadata.
The metadata stored by an inode is determined by filesystem in use, except for filename which is never stored 
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

Xfce as default Ubuntu GNOME had bug with multiple keyboards. 
Furthermore, Xfce codebase was readable when inspecting X11 code.
In addition, Xfce automatically provided GUI shortcut creation

Linux DRM (direct rendering manager) -> X11 (display server) -> xfce (desktop environment)  
Linux ALSA (advanced linux sound architecture) -> pulseaudio (sound server) 

A premptive scheduler will swap processes based on specific criteria.
Round-robin means each process will run for a designated time slice
CFS is a premptive round-robin scheduler. 
Time slices are dynamic, computed like `((1/N) * (niceness))` 
Processes are managed using a RB-Tree. 
Therefore, cost of launching a process or a context switch is logarithmic
(kernel will have internal tick rate that updates waiting threads.
lowering this will increase granularity however will increase CPU time and hence battery time as more time spent in kernel code)
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

.deb and .rpm are binary packages. 
Annoyances arise due to specifying the specific library dependency for each distro version
Flatpaks and Snaps are containerised applications that include the specific libraries and runtimes
AppImages combine the 'shared' libraries and runtimes of Flatpaks and Snaps into a giant file. 
This file can be copied and run on any distro
If packages is being actively maintained, preferable to use .deb as faster and simpler

Terminal type writer something you should ask your grandfather about

KVM (kernel-based virutal machine) kernel based virtual machine allows linux kernel to act as hypervisor
∴ type 1 hypervisor (virtualbox type 2, i.e. run atop an OS)
Often see with KVM/QEMU (virt-manager)

hardware scheduler allows for hyperthreading (AMD simultaneous multi-threading) 
share execution units


RAID is method of combining multiple disks together so appear like one disk called an array.
Various types, e.g. RAID0 (striping) some parts of file in multiple disks, 
RAID1 (mirroring) each disk is dusplicate so could give speed increase etc.


PCI usually for attaching peripherals to motherboards, 
e.g. network/audio/usb/graphics controller cards


# Laptop
## Screens
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

## Power
LiPo  is lithium-ion poly. uses polymer electrolyte instead of liquid.
lipo more expensive, shorter lifespan, less energy, more robust 
li-poly battery is rechargeable (so rechargeable is structured to allow a current to be passed to it to reverse the process)
battery will have two electrodes, say lithium cobalt oxide (cathode) and graphite (anode, negative)
graphite is almost always used as anode
when charging, lithium ions move to graphite
when discharging, lithium ions move from graphite to lithium
so, we can see that the atomic structure of the electrodes are changed, 
hence why charge cycles affect battery life
LiPo  is lithium-ion poly. uses polymer electrolyte instead of liquid.
lipo more expensive, shorter lifespan, less energy, more robust 

## Memory
eMMC is the interface as well
eMMC precursor to SD.
TF (transflash) more commonly known as micro-SD card
cheaper than SSD, yet slower
still uses NAND flash technology

QSPI can be used without CPU with data queues



## CPU
cpu contains clock. each tick marks a step in the 
fetch(signal sent along address bus as specified by PC; instruction or data sent along data bus)-
so, whenever interacting with memory, will first have to specify address bus and get/store result via data bus
therefore, for something like LDR rax, [val] will require two fetch-executes.
this is von-neumann bottleneck
most modern computers are modified-harvard, in that the L1 caches have separate instruction and data regions.
therefore, they can be read simultaneously 

decode-execute cycle
an accumulator register is term used to indicate typical register that holds results of large arithmetic

von-neumann
instruction, data in RAM
instructions+data share data bus between CPU and RAM, so they have to
be fetched separately

harvard 
instruction, data separate

in reality, all CPUs are von-neumann presented to the user
and harvard at the low-level.
it's only harvard from the pipeline/cache stage
modified harvard could also mean instruction and data bus, but unified memory 
so, the variation amongst harvard architectures is in how one transitions to the other 

Procedure Call Standard for the Arm Architecture (AAPCS).
Part of ABI for ARM (also part of ABI is debugging, dynamic linking semantics etc.)
From this we can garner say, not to put more than 4 word sized arguments to
function as this will require stack, consuming more time and space
Furthermore, alignment restrictions mean that passing a double word has to be even-odd register pair,
so ordering of parameters important
So, call standard important for ordering and number of parameters?

When people say vector operations, they mean SIMD

endianness only relevent when reading and writing to memory addresses and 
interpreting the bytes
overall, endianness usefulness determined by majority of operations performed,
e.g. converting string to int easier in big-endian
e.g. little-endian easier to read values of variable length (address does not change)


memory model: visibility and consistency of changes to data stored in memory
necessary for understanding multiple thread execution 

alignment ensures that value doesn't straddle cache line boundaries


more instructions required for unaligned memory accesses?
(most modern x64 arm will not crash on an unaligned access?)
from armv7, unaligned accesses allowed

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
so, language memory model will abstract away hardware memory model?
https://research.swtch.com/plmm

## ARM
cortex-a53 is a synthesisable IP core sold to other semiconductor companies (stm, nxp, etc.)
as licensed IP core, the amount of cache will vary e.g. 8-64Kb?

FPU is a VFPv3-D16 implementation of the ARMv7 floating-point architecture

details hidden due to IP nature, e.g. LPDDR3 memory controller from CPU is all we can know?

## Ports
(audio visual) HDMI-A, C (mini), D (micro)
USB-A,USB-B,USB-B(mini) 
USB-C is USB3.0

3.5mm audio jack (3 pole number of shafts (internal wires); 4 pole for added microphone)

ethernet CAT backwards compatible. connector called RJ45 
telephone cable is called RJ11

IEC power cords (kettleplug, cloverleaf)

DC barrel jack


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
