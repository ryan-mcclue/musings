IMPORTANT: ALWAYS PROVIDE EXAMPLE!

DC motor: raw PWM signal and ground
signal controls speed
high rpm, continous rotation (e.g. fans, cars)
servo: dc-motor + gearing set + control circuit + position sensor
signal controls position
limited to 180°
accurate rotation (e.g. robot arms)
stepper:
can be made to move precise well defined 'steps', i.e. jumps between electromagnets
position fundamental (e.g. 3D printers)

TODO: working with thumb instructions
TODO: best practices monitoring systems in the field (4G?)

note, if using newlib, will still have a _start

May compile for different architectures in embedded for different product lines 
e.g. low-end fit bit, high-end fitbit

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


battery balancing relevent to multiple cells
TP4056 Lithium Battery Charging Board?

we want performant, reliable software.

testing should have coverage information also

verification: requirements
validation: does it solve problem
authenticate: identity
authorise: privelege
token something used to authorise 

HTML not turing-complete, i.e. can't perform data manipulations

working with new ISA think about registers and addressing modes

robot ➞ renode (why not just qemu with shell scripts perhaps?)
seems that robot/renode parsing of UART cleaner?

https://www.youtube.com/watch?v=-Ecf7lb4aZ0
latches ➞ flip flops (or latches are a type of flip-flop?)
several latches combined together form a register
D-type latch in SRAM (caches)
DRAM uses transistor and capacitor

embedded systems special purpose, constrained, often real time (product may be released in regulated environment standardsd, e.g. automotive, rail, defence, medical etc.)
challenges are testabilty and software/hardware comprimises for optimisation problem solving, e.g. bit-banging or cheap mcu, external timer or in-built timer, adding hardware increases power consumption, e.g. ray tracing card or just rasterisation, big.LITTLE clusters

what would a segfault ➞ coredump look like on bare-metal?

C type qualifiers are idempotent, e.g. `const const const int` is fine
sparse + smatch static analysis tools that give named address spaces (near, far pointers)

synchronisation constructs:
* lock (only one thread access at a time)
* mutex (lock that can be shared by multiple processes)
* semaphore (can allow many threads access at a time)

is ARM nested interrupt by default?

C atomics just insert barriers?

function reentrant if can be swapped out and its execution rentered
functions that operate on global structures and employ lock-based sychronisation (could encounter deadlocks if called from signal handler) (and are thread-safe)
like malloc and printf are not reentrant 

qemu useful over native for when word-size different. also, assembly inspection

TODO: DSP, RTOS, wireless + IoT, battery/power, peripheral protocols (USB, LCD, etc.), CODECS,
optimisations particular to MCUs, assembly knowledge, sql, c++ stl, python systems testing (continous integration),
bootloaders

ARM, RISC-V, xtensa

# Language of the People

https://drh.github.io/lcc/

Garbage collection replaces us with a 'search' of our memory and decides when to free
So really only use if you can't figure out when to free something
This search is not free

No language implements a feature that determines a memory footprint, i.e. how much memory we use

Know OS specific forking (process launching), file checking/reading/writing, IPCs

Steps for adding new feature: 
create a new file, e.g. commands.c and include it in main.c (order it before things that will use it) 
then do a git commit.
This gets us off on a nice feeling
In new file, create function with barebones functionality that can then be inserted into location and called/tested

Simple word parser use `at[1] != '\0' && at[1] == 'a'`

Simple single line parser:
First separate by whitespace with `while (at[0] != '\0'); break` and `eat_spaces(), find_ch_from_left()`



3D printing ideas:
https://www.youtube.com/c/3DSage/videos

https://blog.golioth.io/taking-the-next-step-debugging-with-segger-ozone-and-systemview-on-zephyr/
martin-schroder-zephyr-acceptance-testing

lego technics for gear ideas:
https://www.youtube.com/playlist?list=PLJTMHMAVxQmvTGatIj-X2rJrN8aGJEaQD

want to make something like stargate wheel

first step in embedded debugging commandments; thou shalt check voltage (e.g. check 5V going to LCD by placing multimeter on soldered pin heads)

we can see in x86 (wikichips), instruction and data separate from L2 cache
arm SoC block diagram (datasheet), see d-bus and i-bus to RAM
introduce things like CCM (core coupled cache) and ART (adaptive real-time accelerator) that add some more harvard like instruction things
essentially, more busses instead of more cores like in x86, (i.e. a lot more than just a CPU to be concerned with)
also have more debug hardware
don't really know why cortex-m4 has MPU

AXIM, AHB and APB ARM specific
will have a bus matrix (which allows different peripherals to communicate via master and slave ports by requesting and sending data)
off this, have AHB (higher frequency and higher bandwidth).
like to think of AHB as host bus as it feeds into APBs via a bridge. APB1 normally half frequency of APB2
we can see that DMA can go directly to APB without going through bus matrix
So, relevent for speed and clock concerns going through which bus?
Also to know if we are DMA'ing something and CPUing something, they
are not going to be fighting on the same bus, i.e. spread out load
lower power peripherals on lower frequency busses?
this level of knowledge further emphasises need to know hardware to understand what is going on


# Embedded Workflow
https://go.memfault.com/debugging-embedded-devices-in-production-virtual-panel?mc_cid=32b3cae3e7&mc_eid=UNIQID
https://go.memfault.com/embedded-device-observability-metrics-panel-recording?mc_cid=32b3cae3e7&mc_eid=UNIQID

use specifically for understanding mesh networks in context of bluetooth?: https://academy.novelbits.io/register/annual-membership?_gl=1*15qz5rp*_ga*NzM2MjQ4ODUzLjE2NTY0OTIwNjc.*_ga_FTRKLL78BY*MTY1OTU3OTAxNS4xLjEuMTY1OTU3OTIyMy4w&_ga=2.149910452.341656361.1659579015-736248853.1656492067

https://interrupt.memfault.com/blog/ota-delta-updates?utm_campaign=Interrupt%20Blog&utm_medium=email&_hsmi=222505339&_hsenc=p2ANqtz-9kmqPywlKxifduWJneXhUh1h_RQ4bf-v41o2qF8iBciZYc9beFlhwM4EiOVbP3DKUl8kxc_4GOIdpzvkJi5iOGzgwSWA&utm_content=222505339&utm_source=hs_email

automated crash reporting:
https://lance.handmade.network/blog/p/8491-automated_crash_reporting_in_basically_one_400-line_function#26634
in embedded, how are metrics transmitted remotely, i.e. via bluetooth to phone than web? too much power if directly to web?

something related to systems testing with aardvark SPI/I2C adapter (more tutorials with bus pirate)
10:00 time mark: https://www.youtube.com/watch?v=N60WSQc-G_8&list=PLTG9uzDd_HQ84wVz0DwQ5_mwf1GnpY6LB&index=11
seems indepth bus pirate manual is on git?
http://dangerousprototypes.com/docs/Bus_Pirate
https://learn.sparkfun.com/tutorials/bus-pirate-v36a-hookup-guide/all
(look for device specific tutorials on bus pirate website)
http://www.starlino.com/bus_pirate_i2c_tutorial.html

seems that RMII (reduced media-independent interface) is a pin layout to connect MAC devices (flexible in implementation). Can be implemented to support say an RJ45 connector

stm32 datasheet and reference manual (documents of different depths about same mcu) nomenclature
will have 'Application Notes' that detail specific features like CCM RAM
datasheet will often be related to a family, e.g. stm32f429xx.
therefore, at the front will have a table comparing memory, number of gpios, etc. for particulars

bit twiddling:
http://graphics.stanford.edu/~seander/bithacks.html

interesting courses:
https://pikuma.com/courses

is power profiler kit specific to each board necessary, e.g. nordic, stm32?

blogs:
https://dmitry.gr/?r=05.Projects
https://tratt.net/laurie/blog/

Seems that IAR compiler produces smaller, faster code than gcc?

Fallback to: https://grep.app/search when searching for code snippets on github?

UART is protocol for sending/recieving bits. 
RS232 specifies voltage levels

DSP:
https://www.youtube.com/playlist?list=PLTNEB0-EzPluXh0d_5zRprbgRfgkrYxfO

Wifi/Ethernet:
https://www.youtube.com/watch?v=dumqa78j1sg&t=1046s

Lora/Nrf/IoT connections:
https://www.youtube.com/watch?v=mB7LsiscM78

courses:
https://www.youtube.com/watch?v=dnfuNT1dPiM&t=25s
https://www.udemy.com/course/mastering-microcontroller-with-peripheral-driver-development/?ranMID=39197&ranEAID=YuKpx7UHSEk&ranSiteID=YuKpx7UHSEk-VcxzUKL7wAR1VyB2muQaaQ&LSNPUBID=YuKpx7UHSEk&utm_source=aff-campaign&utm_medium=udemyads
https://www.udemy.com/course/microcontroller-dma-programming-fundamentals-to-advanced/?ranMID=39197&ranEAID=YuKpx7UHSEk&ranSiteID=YuKpx7UHSEk-85NcF8rkTsNIoMCfETBU3g&LSNPUBID=YuKpx7UHSEk&utm_source=aff-campaign&utm_medium=udemyads
https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/?ranMID=39197&ranEAID=YuKpx7UHSEk&ranSiteID=YuKpx7UHSEk-qjeQee0Iel.PZ6z63nXsmw&LSNPUBID=YuKpx7UHSEk&utm_source=aff-campaign&utm_medium=udemyads

memory align up:
sbrk((x + 7) & ~7); sbrk() is system call malloc uses?

silicon mainly obtained from quartz. 
electric arc when insulator air is supplied enough energy to ionise

1. **Documentation**
   Generalities such as *Debug Interface* or *Procedure Call Standard*
   Architecture specifics such as *armv7m*
   Micro-architecture specifics such as *cortex-m4*
   Micro-controller specifics such as *stm32f429zi*
   Board specifics such as pin-out diagram and schematic
2. **BSP**
   This can be done via an IDE such as STM32CubeMX to create an example project Makefile.
   Alternatively can be done via a command line application such as libopencm3.
3. **Targets**
   Disable hardware fpu instructions and enable libgloss for simulator. Ensure main() call ordering mock test working 
   Enable nano libc with no system calls for target
4. **Flashing and Debugging**
   Coordinate *JLink/STLink* probe and board pin-outs
   Ascertain board debug firmware and determine if reflashing required, e.g. *STLinkReflash*
   Preferable to use debugger software such as *Ozone* that automates flashing.
   If not, determine flash software such as *JLinkExe*, *stlink-tools*, *openocd*, *nrfjprog* etc.
   Coordinate debugger software such as *QTCreator* with qemu gdb server
5. **Hardware Tools**
   Measure voltage, current, resistance/continuity/diode with multimeter?
   Oscilloscope for ...
   Logic analyser for, SPI and I2C

6. **Protocol**
  USB port probably in-built serial port

astable means no stable states, i.e. is not predominatley low or high, e.g. square wave, sine wave etc.
oscillator generates wave (could be for carrier or clock)

RC (resistor-capacitor) oscillator generates sine wave by charging and discharging periodically (555 astable timer)
internal mcu oscillators typically RC, so subject to frequency variability

Max out the HCLK in the clock diagram as we are not running off battery.
Will have clock sources, e.g. HSI, HSE, PLL. output of these is SYSCLK.
SYSCLK is what would use to calculate cpu instruction cycles.

a clock is an oscillator with a counter that records number of cycles since being initialised

Crystal generates stable frequency
PLL is type of clock circuit that allows for high frequency, reliable clock generation (setup also affords easy clock duplication and clock manipulation)
So, PLL system could have RC or crystal input
Feeding into it is a reference input (typically a crystal oscillator) which goes into a voltage controlled oscillator to output frequency
The feedback of the output frequency into the initial phase detector can be changed
Adding dividers/pre-scalers into this circuit allows to get programmable voltage.
So, a combination of stable crystal (however generate relatively slow signal, e.g. 100MHz) and high frequency RC oscillators (a type of VCO; voltage controlled oscillator)

openocd -f /usr/share/openocd/scripts/interface/jlink.cfg -f /usr/share/openocd/scripts/target/stm32f4x.cfg
should open a tcp port on 3333 for gdb

Seems that an RTOS is a multiprocessing kernel that allows the user to control its scheduling priority?

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

the sparsity of linux can make configuration vary
e.g bluez stack -> modify policies

for long range, LoRa or sigfox
essentially tradeoffs between power and data rate
ieee 802.11 group for WLANs (wifi - high data rate), 
802.15 for WPANs; 802.15.1 (bluetooth - le variant - heavily used in audio), 
802.15.4 low data rate (zigbee implements this standard)


unlike windows msdn, linux documentation is mostly source code (not good as if not fast/easy, not used). 
so, essential to have something like ctags and compile from source (sed -e "s/-Werror//g" -i *.make)
source and unit tests are documentation in many linux sources

xor with itself sets to 0

"1's and 0's" + matej youtube channels

https://embeddedartistry.com/blog/2018/06/04/demystifying-microcontroller-gpio-settings/?mc_cid=c443ecbc14&mc_eid=UNIQID
https://embeddedartistry.com/blog/2019/04/08/a-general-overview-of-what-happens-before-main/?mc_cid=c443ecbc14&mc_eid=UNIQID

unit test notes from udemy

Aardvark adapter essential for automated testing (so, an adapter of sorts should always be used
for automated testing?)
Ask philip johnston on embedded artistry regarding widespread testing, e.g. multiple adapters?
