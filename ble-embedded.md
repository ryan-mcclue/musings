IMPORTANT: ALWAYS PROVIDE EXAMPLE!

TODO: how to best architect timing requirements given that ISR should be small?
e.g. this executes every 1 second, this every 500ms etc.

could even be simplex (one way only)
full-duplex can still be serial, requiring at least two wires
serial (synchronous with clock data; asynchronous) + parallel 

BUTTON BOX 3D PRINT:
* print out small test part with holes to test if button/screw/etc. inserts fit correctly
* consider leaving holes in 3D scaffolding for making wiring easier
* for stylistic finish, can print a separate plate with different resin and attach with hot glue
* will have a bottom plate to account for wiring (have threaded inserts (male-to-female) to account for box moving about when pressing a button)
1. solder wires to buttons
2. solder wires to mcu
3. secure mcu

no heap allocation, just use statically allocated pools: 
https://mcuoneclipse.com/2022/11/06/how-to-make-sure-no-dynamic-memory-is-used/?mc_cid=26981ac7f4&mc_eid=UNIQID

Fast Fourier Transform (FFTs) are often used with DACs to create a spectrum analyser which allows for subsequent beat detection?

an adapter board would handle power and signal conversion

TODO: DSI vs USB vs HDMI?

tiny solid-state batteries more efficient that coin cell batteries; 
useful for wearables

TODO: go through
https://embedded.fm/blog/embedded-wednesdays

Dagger CI in code not YAML
Qucs-S for SPICE, free circuit simulator GUI

TODO: possible program usage of syncthing 

bipartite ring buffer more efficient?
TODO: seems with RTOS emphasis on 'tasks', will require understanding job queues
https://www.aosabook.org/en/freertos.html

filters relating to a pedometer: 
https://www.aosabook.org/en/500L/a-pedometer-in-the-real-world.html

TODO: richard braun embedded (useful C has functions)
https://www.twitch.tv/videos/233685076?filter=all&sort=time

I only touch low-voltage 5V side, the high 'dangerous' voltage side I leave to hardware engineers

TODO: rhymu 'rusty keyboard' series

TODO: FreeRTOS spectrum analyzer, i.e. using 'tasks': https://www.youtube.com/watch?v=f_zt7zdGJCA

https://iosoft.blog/2020/09/29/raspberry-pi-multi-channel-ws2812/
addressable rgb, each LED has serial chip with a serial port in and serial port out that it funnels through the chain
(interesting option of having a light display at sunset)
using PlatformIO (python under the hood) very slow.
perhaps though, it's useful for finding libraries or possible environment options to explore like unit testing
BOARD-SELECTION: (want a board with WiFi and a builtin display?)
IMPORTANT: chip cost may be $4, board cost $15
go with ARM (although ESP32 much more powerful than Atmel, e.g. RAM, peripherals, frequency similar cost)

LEDS:
tricolor led can be any 3 colours. RGB LED is specific colours
we power level appropriate from cable, board will enter program mode automatically
resolution of PWM setup dependent on the frequency set up for it 
(e.g. resolution of 8 gives 100% duty cycle of 255)
```
red_led_duty_cycle(pwm_max_val - red_component);
green_led_duty_cycle(pwm_max_val - green_component);
blue_led_duty_cycle(pwm_max_val - blue_component);
```
TODO: is brightness of PWM determined by duty cycle?
If so, then a PWM LED fade should just be duty cycle, or should it be colour as well?

DISPLAY:
useful to display FPS (might only need to display this say, every 250ms as oppose to every frame), 
power consumption (perhaps unscaled power, i.e. power if not performing alterations; perhaps only bright LED is power throttling)
technology, size, connection
as various permutations of these, have various controller permutations 
e.g. SH1107_I2C, SSD1306_SPI, etc. (we probably want a library to draw lines, shapes, fonts, etc.)
I2C developed by Phillips to allow multiple chips on a board to communicate with only 3 wires (id is passed on bus)
(number of devices is limited by address space; typically 128 addresses?)
price difference between a shape drawable display and character display?
```
// Moire pattern
for (u32 x = 0; x < display_width; ++x)
  draw_line(x, 0, display_width - x, display_height);

// Cocentric circles
for (u32 r_count = 0; r_count < 3; r_count += 1)
  draw_circle_outline(x, y, r_count);

// rainbow
light_led(led_i, colour_val += 10);

// Marquee (draw colour, then draw moving black spots)
for (u32 i = scroll % 5; i < num_leds; ++i) set_led(i, black);

// stars
leds[random(leds_len)] = colours[random(colours_len)];
delay_ms(200);
// might be better to draw a single LED each pass and use a LOCAL_PERSIST to
// keep track of how many times function is called as have delay

// comet
u32 trailer_pixel_fade_amount = 128;
u32 num_core_pixels = 5;
u32 delta_hue = 4;
r32 comet_speed = 0.5f;

// instead of clearing all LEDs, reduce brightness, i.e. incrementally fade to black
led_fade_to_black(leds, i, amount);

u32 hue = HUE_RED;
hue += delta_hue;

i32 direction = -1;
u32 pos = 0;
pos += (direction * comet_speed); 
// IMPORTANT: to get a 'smooth' effect, require floating point speed multiplier
// IMPORTANT: so, animation stages are u32, r32, then easing (kinematics/lerp)/cycling(sin) etc.
// IMPORTANT: gradual colour blending to clear, instead of instant colour clear
// IMPORTANT: then move to perhaps altering colour
// IMPORTANT: when calculating values, the natural derivation may prove too large or too small,
// e.g. cur_time - prev_time. so, add something like a 'speed_knob' that will / or *

// IMPORTANT: floating point coordinates only one part of smoothness.
// also require fractional drawing
// this means that drawing 0.25 of a pixel will be 0.25 of its original colour
colour colour_fraction(colour_in, fraction)
{
  fraction = min(1.0f, fraction);
  return colour.fadeToBlack(255 * (1.0f - fraction));
}
// IMPORTANT: so, in addition to 'sub-pixel' drawing, also have automatic blending
void draw_pixels(ipos, pixel_len, colour)
{
  first_pixel_avail = 1.0f - (fpos - (int)fpos); // get fractional part
  first_pixel_avail = min(first_pixel_avail, pixel_len);
  remaining = min(count, strip_len - fpos);
  ipos = fpos;

  if (remaining > 0.0f)
  {
    // ipos = get_pixel_order();
    set_led(ipos++, colour_fraction(colour, first_pixel_avail));
    remaining -= first_pixel_avail;
  }

  while (remaining > 1.0f)
  {
    set_led(ipos++, colour);
    remaining--;
  }

  if (remaining > 0.0f)
  {
    set_led(ipos++, colour_fraction(colour, remaining));
  }
}
// IMPORTANT: now increment by say 0.1 instead of 1

// draw comet block
// the delay() value will probably be specific to the CPU frequency

// Check how fast we can draw this out over I2C (want something like get_ms())
// use weighted average to prevent value flickering, i.e. jumping (it will take some number of frames to stabilise as starts at 0)
weighted_average = prev_value * 0.9 + new_value * 0.1; (probably use LOCAL_PERSIST)
```

```
free-fall: v = sqrt(-2 * gravity * distance_fallen);

r32 gravity = -9.81f;
r32 start_height = 1;
r32 impact_velocity = free_fall(start_height);
r32 speed_knob = 4.0f;

struct BouncingBallEffect
{
  u32 output_led_strip_len;
  u32 fade_rate;
  b32 reversed (draw from len inwards; i = size - 1 - i); // common parameter for effects
  b32 mirrored (size = size / 2); // common parameter for effects
  r32 time_since_last_bounce;
  r32 ball_speed;
  r32 dampening; // value closer to 1 preserves more energy (0.9f - i / pow(num_balls, 2))
  u32 colour;
};

time_since_last_bounce = time() - clock_at_last_bounce;
// constant acceleration function (here gravity is decellerating us)
// this could be considered an easing function
ball_height = 0.5 * gravity * pow(time_since_last_bounce, 2.0f) + ball_speed * time_since_last_bounce;
if (ball_height < 0) // bounce
{
  ball_height = 0;
  ball_speed = dampening * ball_speed;
  if (ball_speed < 0.01f) ball_speed = initial_ball_speed;
}

position = (ball_height * (strip_len - 1) / start_height);
set_led(position, colour); 
// IMPORTANT: as no need for explicit collisiong detection, 
// if we do additive colours, led[i] += colour; we get mixing
if (mirrored) set_led(strip_len - 1 - position, colour);

print(" " * sin(freq * i) + "ryan") // map sin() to our desired range
// could also have sawtooth(), cubic() etc. 
```
#define cli() nvic_globalirq_disable()
#define sei() nvic_globalirq_enable()

```
// fractional drawing required for smooth 'slow-motion' effects

blend_amount_self = 2, blend_amount_neighbour1 = 3 (take 2 parts of itself, 3 parts of neighbour)
blend_total = 5;

// for parameters essentially / for setting relative to length,
// + for offset, and * for a scaling factor

// cool
for (u32 i = 0; i < size; ++i)
  heat[i] = max(0L, heat[i] - random(0, ((cooling * 10) / size) + 2));

// move heat up
for (u32 i = 0; i < size; ++i)
  heat[i] = (heat[i] * blend_self + heat[(i + 1) % size] * blend_neighbour) / blend_total;

// ignite a spark
for (u32 i = 0; i < num_sparks; ++i)
{
  if (random(255) < sparking_threshold_probability)
  {
    u32 y = size - 1 - random(spark_height);
    heat[y] = heat[y] + random(160, 255);
  }
}

// convert heat to colour
for (u32 i = 0; i < size; ++i)
  colour = map_colour_to_red(heat[i]);
  draw_pixels(i, 1, colour);
```

```
FAN_STRIP_SIZE = 16;
NUM_FANS = 3;
ZERO_PIXEL_OFFSET = 4; (how much have to rotate by for 0th pixel to be on bottom)

enum PIXEL_ORDER
{
  PIXEL_ORDER_SEQUENTIAL = 1 << 0,
  PIXEL_ORDER_REVERSE = 1 << 1,
  PIXEL_ORDER_BOTTOM_UP = 1 << 2,
  PIXEL_ORDER_TOP_DOWN = 1 << 3,
  PIXEL_ORDER_LEFT_RIGHT = 1 << 4,
  PIXEL_ORDER_RIGHT_LEFT = 1 << 5,
};

u32 get_pixel_order(i32 pos, PIXEL_ORDER order)
{
  // this allows negative indexing, i.e. -1 is last
  while (pos < 0) pos += FAN_SIZE;

  u32 offset = (pos + ZERO_PIXEL_OFFSET) % FAN_STRIP_SIZE;
  u32 reverse_offset = (pos + FAN_STRIP_SIZE - ZERO_PIXEL_OFFSET) % FAN_STRIP_SIZE;
  // start in particular fan, e.g. 0, 16, 32, 48, 64, etc.
  u32 fan_base = pos - (pos % FAN_STRIP_SIZE);

  switch (order)
  {
    case PIXEL_ORDER_SEQUENTIAL:
      return fan_base + offset;
    case PIXEL_ORDER_REVERSE:
      return fan_base + FAN_STRIP_SIZE - 1 - reverse_offset;
    case PIXEL_ORDER_BOTTOM_UP:
      return fan_base + FAN_STRIP_SIZE - 1 - (VERTICAL_LOOKUP[pos % FAN_STRIP_SIZE] + ZERO_PIXEL_OFFSET) % FAN_STRIP_SIZE;
  }
}

// IMPORTANT: connecting multiple strips is daisy chaining, so still same output
// allow for offset from fan_index, and also just passing number treating all fans together
// IMPORTANT: optional arguments, use struct
for (u32 i = 0; i < NUM_FANS; ++i)
   draw_fan_pixels(sin(x), 1, red, PIXEL_ORDER_LEFT_TO_RIGHT, i);
for (u32 i = 0; i < NUM_PIXELS; ++i)
   draw_fan_pixels(sin(x), 1, red, PIXEL_ORDER_LEFT_TO_RIGHT, 0);
```

```
wire strippers +/- affects tension which will affect any wire braiding
put heat shrink on before wire soldering
will often twist multiple common wires (e.g. ground) together and solder to single ground source
(have gloves on for this)
wiring can be too fine for holding a particular amperage?
TODO: look into sleeving wires
TODO: something like loctite threadlocker is a liquid that prevents threads loosening due to vibration 

Adhesive tape (with conductive pads on end) on bottom of LED strip stick wires to. Then solder end tips to connect bottom to top
(have some electrical tape for this)
Also, buy wires merged together and only pull apart the end points
With multimeter, verify that no short circuit by checking continuity amongst wires 

Critical section means non concurrent access to this code. 
Acheived with a mutex obtain and lock pair?
```

IMPORTANT: for a preemptive multitasking kernel like linux, a call to pthread_yeild() (allow other threads to run on CPU)
is not necessary. however, for embedded, maybe

LEDS:
WS2812B is standard. Neopixel brand. Each chip is RGB LED with MCU.
probably has low drop-out regulator onboard so can pass 5V to 3.3V
can buy as grids or strips
have a library to generate the square wave forms, e.g. FastLED

by powering board with pins, can ensure enough current is passing to it
can simultaneously connect USB cable and will defer to power pins over it? 

With power, still have to be careful if say, setting max brightness and all LEDS to white
seems it's common to have GBR as format?
TODO: only in video 4 are power calculations done?
(have a function to limit max watts of power drawn?)

// #define AT_MOST_N_MILLIS(NAME,N) static AtMostNMillis NAME(N); if( NAME )
// operator bool() { return ready(); }

8BIT MATH (another reason to inspect assembly):
//      qadd8( i, j) == MIN( (i + j), 0xFF ) (saturated add)
//      qsub8( i, j) == MAX( (i - j), 0 )
//      mul8( i, j)  == (i * j) & 0xff
//      add8( i, j)  == (i + j) & 0xff
//      sub8( i, j)  == (i - j) & 0xff
//      sin8( x)  == (sin( (x/128.0) * pi) * 128) + 128
//      cos8( x)  == (cos( (x/128.0) * pi) * 128) + 128
#if QADD8_C == 1
    unsigned int t = i + j;
    if( t > 255) t = 255;
    return t;
#elif QADD8_ARM_DSP_ASM == 1
    asm volatile( "uqadd8 %0, %0, %1" : "+r" (i) : "r" (j));
    return i;
#endif


TODO: gdb scripts
https://github.com/espressif/freertos-gdb

TODO: is an IoT cloud data store like ThingSpeak used commercially?

TODO: investigate Renode for embedded testing

TODO: networking with containers: https://iximiuz.com/en/series/computer-networking-fundamentals/ 

TODO: look at embeddedartistry course developments for 'building testable embedded systems' 

TODO: working with time-series data

LIN protocol for vehicles (seems to be a host of protocols specific to automotives)

trimpot a.k.a potentiometer

MEMS (micro-electromechanical systems) motion sensor

TODO: using in-built security features of chip, e.g. AES-256

buck converter steps down DC-DC voltage, while stepping up current
(various step-down mechanisms in relation to AC/DC and voltage/current)

although bluetooth LE say 50m distance, a repeater can be used (and really for any RF)

TODO: profiling not your application
https://linus.schreibt.jetzt/posts/qemu-9p-performance.html

TODO: https://dev.to/taugustyn/call-stack-logger-function-instrumentation-as-a-way-to-trace-programs-flow-of-execution-419a

MIPI (mobile industry processor interface) DSI

TODO: outdoor project: https://hackaday.io/project/186064-green-detect
look into further hackaday.io/projects

TODO: embed LEDs onto 3D printed board

contrasting telephoto and wide angle lens

TODO: differences between ribbon cable and normal

TODO: TPM (trusted platform module) for embedded 
https://www.amazon.com/Trusted-Platform-Module-Basics-Technology/dp/0750679603

TODO: freeRTOS
https://www.digikey.com/en/maker/projects/getting-started-with-stm32-introduction-to-freertos/ad275395687e4d85935351e16ec575b1

TODO: bootlin courses

TODO: relay connected to a maglock

TODO: GPS tracking, LORAwan (long range, low power), satelitte connection, etc.

TODO: 3D printing; outdoor case enclosure

Digital laser dust sensor (particulates in the air). PM1 being worst as most fine
TODO: DC vs stepper motor?
TODO: what is the use case for a motor driver such as L298 Dual H-Bridge Motor Driver
and Tic T500 USB Multi-Interface Stepper Motor Controller (circuitry without MCU?)
driver lines are diode protected from back EMF?

Growth of 'enviro sensor kits', i.e. test air quality etc. to create smart home or garden
Growth of 'IoT' sensor kits/smart home
Growth of 'AI sensors'
(Perhaps more relevent to me is power/energy and automation and sensing)
Growth of 'IoT' sensor kits
Growth of sensor compounding, e.g. video now with LiDAR to detect depth, gesture detection sensor

H-bridge is IC that switches voltage polarity, e.g. run DC motors forwards or backwards
Rectifier converts AC to DC (transformer is high voltage AC to low voltage AC)

TODO: is something like SerialStudio necessary for visualisation? 
Could not live stream gnuplot?

TODO: domestica course on creative coding.
css, html graphics also for 2D animations?

TODO: Hackaday for project inspiration

Marketing refers to some MCU work as physical computing

TODO: investigate both AVR github and gists: https://github.com/BobBurns 

CFFI to create a python interface for C for something say like a console session?

TODO: Perhaps for a machine just targeting embedded development, use WSL to easily use GUI debugger apps?

TODO: Essentials of putting metadata section in firmware binary, such as version string! (particularly so test runners can utilise this information)

Might see GNSS + INS (inertial navigation system; i.e using IMU as well)

Perhaps if gaffer low-level correctness, searching for web API design message types?

TODO: amplifiers, e.g. class-D etc. MEMs accelerometers for vibration detection in cars

TODO: RTC is external to oscillator?

Analog LED is single colour
Digital LED allows controlling each colour separately (so, a.k.a e.g addressable APA102 LEDs)
This is done through an LED chip (think WS2812B LED chip for NeoPixel)

A channel in a sensor is quantity measured. So an acceleration sensor could have 3 channels, 1 for each axes

DSP: http://www.dspguide.com/pdfbook.htm
terms like THD (total harmonic distribution), PFC (power factor correction), HV (high voltage)

circuit design in software (perhaps parallel with Robert Feranc?): https://www.jitx.com/  

https://embeddedartistry.com/blog/2019/04/08/a-general-overview-of-what-happens-before-main/?mc_cid=3835da293a&mc_eid=UNIQID

Go through talks on memfault.com blog 

Simulator testing gives much faster turn-around times, can add sanitisers without memory concerns, pass peripheral data in from file, draw to window instead of display etc. 

For memory allocator, set to 0 on free in debug builds?
Investigate gcc tunables, e.g. in debug build: export MALLOC_PERTURB_=$(($RANDOM % 255 + 1))

network game (with some testing tools): https://github.com/TheSandvichMaker/netgame 

TODO: how to calculate: "Its power supply system can supply up to 4200 mAh and run for more than 5 hours"
SiC (silicon carbide) power module
Whole area in power management (also leads onto safety regulations, e.g. SIL) 

use time-series database where everything is stored ordered 
(as oppose to relational based on set thoery whose order is determined in clauses. also get full SQL support here and can store more data types)

investigate automated 'agriculture', e.g smart argicultural kit, automatic pot plant watering, etc.

databases useful for tracking time series IoT sensor data 

how to overclock and underclock?
how are these different to adjusting clock scalers?

awesome escape puzzles:
https://www.youtube.com/c/PlayfulTechnology

investigate cpu fault handling: https://github.com/tobermory/faultHandling-cortex-m

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

AVR used in lower-end (8bit) as less complex, cheaper than ARM

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
1/4 scalar performance for 1/2 power consumption good tradeoff

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


TODO: zephyr series (perhaps better to do this first as an RTOS as more modern resources than FreeRTOS?)
https://training.golioth.io/
https://blog.golioth.io/zephyr-threads-work-queues-message-queues-and-how-we-use-them/?mc_cid=f6bdf458d1&mc_eid=UNIQID
https://blog.golioth.io/taking-the-next-step-debugging-with-segger-ozone-and-systemview-on-zephyr/

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

something related to HIL (hardware-in-loop testing)
https://blog.golioth.io/golioth-hil-testing-part1/?mc_cid=da33e3796b&mc_eid=UNIQID

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

software-blogs:
https://www.gingerbill.org/article/
https://www.rfleury.com/

https://linuxafterdark.net/ podcast

embedded-blogs:
https://blog.feabhas.com/
https://blog.st.com/
https://dmitry.gr/?r=05.Projects
https://tratt.net/laurie/blog/
http://stevehanov.ca/blog/
https://thephd.dev/
https://www.embeddedrelated.com/blogs.php
https://lemon.rip/
https://jpieper.com/
https://www.embeddedrelated.com/
https://patternsinthemachine.net/category/general/
https://embeddeduse.com/
https://martinfowler.com/articles/patterns-of-distributed-systems/?mc_cid=3835da293a&mc_eid=UNIQID

memfault

https://embeddedartistry.com/fieldatlas/embedded-software-development-maturity-model/?mc_cid=da33e3796b&mc_eid=UNIQID

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

(HSPI is high speed parallel interface)

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

Pull-up/down resistors are to used for unconnected input pins to avoid floating state
So, a pull-down will have the pin (when in an unconnected state) to ground, i.e. 0V when switch is not on

IMPORTANT: Although enabling internal resistors, 
must look at board schematic as external resistors might overrule

Vdd (drain, power supply to chip)
Vcc (collector, subsection of chip, i.e. supply voltage of circuit)
Vss (sink, ground)
Vee (emitter, ground)

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
