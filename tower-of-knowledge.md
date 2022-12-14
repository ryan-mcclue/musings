# IMPORTANT: FOR ALL TOPICS INCLUDED, HAVE SOME MENTION ON WALLPAPER

# MCU

# Phone

# Wearable
5ATM is 5 atmospheres. 1 atmosphere is about 10m (however calculated when motionless)
50m for 10 minutes

# Desktop
RAID is method of combining multiple disks together so appear like one disk called an array.
Various types, e.g. RAID0 (striping) some parts of file in multiple disks, 
RAID1 (mirroring) each disk is duplicate so could give speed increase etc.

How do containerised/sandboxed applications like Flatpak work on linux?
kernel offers various methods of process isolation, e.g. chroot, cgroups etc.
(chroot cannot access files outside its designated tree)
A container will utilise one of these options provided by the kernel to acheive:
 * cannot send signals to processes outside container
 * has own networking namespace
 * resource usage limits

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

nit is a measure of luminance, i.e. intensity of light (brightness is how we percieve luminance ∴ subjective).
higher nit display are more easily viewable in a wider array of lighting conditions (e.g. combat the sun's light reflecting off the surface)

e-ink less power than LCD as only uses power when arrangment of colours changes

# Dalgo

# Networks
RFC documents contain technical specifications for Internet techologies, e.g. IP, UDP, etc.

# Rendering
shader is a GPU program that is run at a particular stage in the rendering pipeline
CUDA is a general purpose GPU program that can utilise the GPU's highly parallised architecture

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

## Bootstrap 
UEFI (interface between firmwire and system; essentially interface to boot into things) ACPI (data format to convey firmwire information)
the ESP will have EGI entries that point to a UUID of where to boot
one of these will be grub binary like shimx64.efi

## Formats 
fat for esp (because FAT simple, open and supported virtually everywhere)
vfat is driver (typically for fat32)

ext4 for system (supports larger file sizes)
(ntfs microsoft proprietary)
most filesystems will use a self-balancing tree to index files

## Unix
KVM kernel based virtual machine allows linux kernel to act as hypervisor
∴ type 1 hypervisor (virtualbox type 2, i.e. run atop an OS)
Often see with KVM/QEMU (virt-manager)

Contempt culture (language wars)-change-systemd
Original Unix borne out of want for simplicity over competing systems
Terminal type writer something you should ask your grandfather about
Service is like a collection of daemons.
Need for services came from growth of Web and need for handling databases, 
lots of connections etc. 
Something like launchd (an unit system) allows services to be started not at boot time.  
Also listen for software hardware changes.  
So,  we extend from simple bootstrapping to handling events.  
So,  now more than service manager,  a system manager
As kernel more dynamic,  e.g change IRQ for device at runtime,  
can think of a system layer as an intermediary between kernel and users pace.  
Have things like network manager. 
This avoids having 15 disparate packages each with own file format config
Systemd a collection of binaries,  so not exactly violating Unix philosphy
Some disparity between systemd idea and its inplementation
Major component of systemd is service manager,  I.e init system to bootstrap user space

system level software
systemd ()
cron (job scheduler)

in a sense, LTS involves significant backports

hardlink to inode (therefore impervious to file name change, deletion, etc.)
softlink to file name

a directory is a mapping of filenames to inodes (for ext4 atleast?)

journaling is regularly writing operations that are to be performed in RAM to disk area of memory known as journal.
then apply these changes to disk when necessary
this overhead makes them slower, but more robust on crashes as can read journal to ascertain
whether certain operations finished performing

the metadata stored by inode determined by filesystem in use  
e.g. fat32 won't store permissions, last modification time
fat32 also no journaling or soft-links
inode references file metadata (pointer to data, permissions, access time).
this allows concurrent modification of the file

UUID/GUID (universally/globally) 16bytes

premptive scheduler will swap processes based on specific criteria.
most are round-robin, i.e. has process is run for a predefined time slice 
CFS does not use heuristics (ad hoc guide), ∴ more straightforward
uses rb-tree. so, as task inserted onto this is logarithmic, ∴ this is cost of context switch
each node key is `(time process as run on CPU) * (process niceness)`

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

CISC gives reduced cache pressure for high-intensive, sustained loops

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

3.5mm audio jack (3 pole; 4 pole for added microphone)

ethernet CAT backwards compatible. connector called RJ45 
telephone cable is called RJ11

IEC power cords (kettleplug, cloverleaf)

DC barrel jack

## Legalities
anti-trust laws don't prevent monopolies, they prevent attempting to
monopolise illegal, e.g. Microsoft attempting to monopolise browser market

permissive (MIT, BSD, Apache, zlib) gives users more freedom to say relicense, 
include closed source software, etc.
generally just attribution licenses (in that this is only enforcement)

weak copyleft (LPGL) (glibc)
applies to files of library not your entire codebase, i.e. must still release your version of the library used
so, dynamic linking makes this easier for keeping your source closed
if static, must make a few extra steps to ensure the LGPL parts are available, e.g. publish object files

copyleft (GPL) enforces devlopers wants the for code on users, so the software stays under the same license. 
encounter more licensing restrictions.
GPLv3 tiviosiataion, issue for Mac as their code is signed so doesn't allow for user modification
(so deals with hardware/system locks)
code signing issues?
so, GPL infects your software, enforces even if just a library 
usage to release whole project as GPLv3 (so source code released)

although android technically open-source, most of the software run on it isn't

creative commons is a set of licenses that make explicit requirements for users, e.g. CC BY-NC, CC BY-SA, etc.
unlike MIT, Apache includes a user non-litigation, so users can't initiate litigation if the creators decide to patent something

RENDERING:
rendering is the process of creating the 2D/3D model, i.e. the drawing onto the monitor
ray tracing solves transparency issues, it's just substantially slower than standard projection rasterisation (so future is ray tracing)

3D works by emulating a simplified model of how a single human eye views space

A single point of light from the sun hitting a single object will reflect in multiple straight line directions/paths

rasterisation is taking polygon and converting to pixels
eventually will have to convert say 3D position to 2D (as all rendering is fundamentally this)

lens refract light to central focal point

1. perspective projection (assume viewer is some distance away from screen, similar triangles)

