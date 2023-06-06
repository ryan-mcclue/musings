TODO: using ChatGPT effectively for embedded, i.e. AI as a coding assistant, i.e. AI as a coding assistant
https://news.ycombinator.com/item?id=36037559

if have code signing keys, should have way to revoke keys in case they are stolen

document control system for CAD/HW files, i.e. store in separate repo to software to avoid say 8GB repos? (so a non-monorepo)

learning RTOS:
The core of using a RTOS is the application design, that should be event driven to avoid polling and work efficiently and execute obeying the real time requirements.

first some concepts are universal: mutex, binary semaphor, counting semaphore, a queu, and priority
a good learning example is to create your own (each one of those) using a conditioned variable
learn these separately. have a good understanding.
understand for example priority inversion.
only then learn a specific rtos which is an implimentation
another thing to learn is how a task switch occurs on a specific cpu.
how for example does the rtos start the first task, why does it create this weird data structure on the stack? how is this similar to an interrupt/context(pre-emption) switch? agian that would be for a specific implementation of an rtos on a specific cpu.
and yes you will need to learn assembly language it is a key component of how a scheduler and context switch occur

Not every RTOS is POSIX compliant, but IMO there is value in learning the POSIX implementations and APIs for ptheads, mutexes, semaphores, message queues, barriers, etc.

for safety critical applications, typically have a team or someone specialising in specifying software safety requirements:
Working in Automotive safety, I can tell you that you need a qualified team of engineers in each domain, or the effort needed in a cross-functional safety team increases tremendously. 

compression for compression speed, e.g. lz4 or resultant size

regarding embedded CI:
CD is not comparable to what CD would mean for a Web app. In our case it's a series of scripts in a pipeline which automate the delivery process (from building to packaging, notifying the right people, archiving all artifacts, etc). But then it's not "deployed" per se as it wouldn't make sense in our product.
Basically all steps that used to be done locally and manually when delivering firmware have been automated/scripted and put in a configurable pipeline.

Totally depends on your needs, it's here to help you not to add constraints. Maybe in some IoT projects it's even a proper deployment in the sense that at the end of the pipeline some firmware update is pushed to all endpoints. In your case or mine it may not be relevant.
What matters is to automate what can be automated and would bring value if it is, shorten the cycle and gain in confidence in what you're doing. The what depends on the use case.

Yeah implementing buzzwords doesn't make your product or sev experience better. You implement buzzwords IF they make your product/dev experience better. Or if the boss says so...

Honestly I believe the biggest hurdle is effective and sane hardware in the loop ci/cd.
We've accomplished this by having on prem ci/cd agents hooked up to hardware over uart and jtag to run tests etc

TODO: stm32 radar

TODO: use terminology like this CRC
STM32F1 implements the CRC32 from IEEE 802.3 (Ethernet). Lots of other systems use that CRC32-IEEE, but there are a few other different CRC32 standards in common use. MPEG2 also uses the same CRC32 version as Ethernet. CRC32C is probably the second most common CRC32 function used.
CRC32 is a pseudorandom function family, there are many functions (mappings from {0,1}n→{0,1}32) in the family. The parameters are the 32-bit input polynomial, the byte ordering, the bit ordering, whether there's padding, and whether there's an initial input value (and what it is if so).

CRC32 isn't one standard algorithm. It's 232 different possible algorithms, depending on which polynomial you pick. There are a few dozen or so commonly used polynomials. ST's AN4187 gives good documentation on how to compute them using the CRC peripheral. The STM32F1 series is specialized to compute only the 0x4C11DB7 polynomial, with the initial value 0xFFFFFFFF. That's a common one (used in 802.3 Ethernet), but isn't the only one.

IIRC even the old STM32F1xx the CRC32 peripheral can process has a throughput of 8 bit per cycle (also 16 bit every two cycles or 32 bit every 4 cycles). That's hard to beat with anything more than add/shift/xor.

78k cycles to compute a CRC for 256 words is some 300 cycles per word. That must be for the 1-bit at a time algorithm.
I'ts possible to do CRC32 using a byte based algorithm using a 1k lookup table at a fraction of these cycles.

ST manual will give polynomial used for CRC, so can check on PC:
```
class Crc32:
    crc_table = {}

    def __init__(self, _poly):
        # Generate CRC table for polynomial
        for i in range(256):
            c = i << 24
            for j in range(8):
                c = (c << 1) ^ _poly if (c & 0x80000000) else c << 1
            self.crc_table[i] = c & 0xFFFFFFFF

    # Calculate CRC from input buffer
    def calculate(self, buf):
        crc = 0xFFFFFFFF

        i = 0
        while i < len(buf):
            b = [buf[i+3], buf[i+2], buf[i+1], buf[i+0]]
            i += 4
            for byte in b:
                crc = ((crc << 8) & 0xFFFFFFFF) ^ self.crc_table[(crc >> 24) ^ byte]
        return crc
    
    # Create bytes array from integer input
    def crc_int_to_bytes(self, i):
        return [(i >> 24) & 0xFF, (i >> 16) & 0xFF, (i >> 8) & 0xFF, i & 0xFF]
        
# Use it
# Prepare CRC block
crc = Crc32(0x04C11DB7)
result = crc.calculate(your_input....)
```



TODO: know how to write application with an RTOS

TODO: STM32 audio synthesiser https://trebledj.github.io/posts/digital-audio-synthesis-for-dummies-part-1/ 

interesting pnemuatics and vacuum pumps: https://www.programmableair.com/ 

https://www.vipshek.com/blog/gpt-learning?utm_source=tldrnewsletter
TODO: read these after shower!

then i suggest something like the following:

    describe a lightly complex system; for example:

        100Hz ODR sensor (lets say acceleromter)

        50MHz cortex M MCU

        firmware algorithm to process sensor data (give range of performance; perhaps 50 to 50k cycles to process 1kB of sensor data, depending on the values in that data set)

        wifi module connected via UART

        user control via wifi (interface is out of scope, maybe a smartphone or browser)

        user controls can affect sensor ODR, filter algorithms and therefore performance, and turn on/off data streaming to cloud versus just local storage of summary data

    ask candidate to sketch the block diagram

        give loads of assistance here. the goal is to agree on how these are connected and make sure the candidate was part of the process rather than handed an inadequate spec)

    ask candidate to draw up protocol diagrams showing how the comms between various components would play out. ask questions to motivate the candidate through at least the following:

        boot: something has to configure the sensor, connect to wifi, etc

        sync: something has to pull existing user settings from the cloud/etc. use that to fine tune the sensor config and other options. perhaps time sync matters too?

        operational mode: what happens once it is up and running normally? this is the stuff that most will focus on right away

        reconfig: user sends some config packets while system is running; how to handle that? do you restart threads? do you collect a set of changes and commit all at once or commit each small change as they are made?

        resync: system losses wifi signal then reacquires it; what do you do? can this be detected? do you need a heartbeat or similar?

        halt: perhaps the system is battery powered and needs to halt on its own as the battery gets low - what should it do? is it safe to broadcast a wifi msg saying "i'm halted"? how to reduce power consumption but prevent a reboot into operational mode again?

        end of life: any thoughts on the permanent end of application life? e.g. maybe this sensor is part of an airbag control system on a car; once it has crashed it should be considered "dead" until proven otherwise... how might that be accomplished?

    bad design

        ask candidate to model their firmware for this system (e.g. pseudo-code describing the threads/tasks)

        ask them to make as many mistakes as they can: do it poorly as a counter example of good design

        ask candidate to describe the mistakes:

            who might make a mistake like this (experienced engineers?)

            how difficult would it be to debug a mistake like this, and why?

            how harmful to the system might this mistake be (e.g. crash versus poor performance)?

            how might the candidate guide another engineer to better understand this mistake and avoid it in the future (good collaboration skills required here)

    better design

        ask candidate to model their firmware without intentional mistakes

        ask how they might test this firmware to expose potential accidental mistakes

        ask how their tests might prove suitability for purpose even without proving it is bug free

        ask how they might ensure the design is maintainable over time and that future engineers are least likely to inject new mistakes (e.g. readable code, inline documentation, unit tests, etc)

i hold that interviews are conversation starters. you should listen more than you speak and give the candidate lots of opportunity to make mistakes and identify them with the least interference you can muster. the best work environments, in my opinion, will eventually give them the opportunity to solve problems however they see fit... they need to be comfortable making mistakes, identifying them, owning them in front of their group, fixing them, and documenting that process to help the group as a whole mature such that they will all make fewer mistakes in the future.

Ask the candidate about their experience with RTOS then expand from there. Start with what they know then base the design on that to stretch them a little. E.g. What would you change to add another sensor? How did you prove it works? What would you change for the next revision? How would you handle scaling to 100x quantity?

firmware security: https://bugprove.com/knowledge-hub/firmware-vulnerabilities-you-dont-want-in-your-product/ 



JEDEC specification to implement commands to get NOR flash sector sizes?

https://academy.nordicsemi.com/courses/bluetooth-low-energy-fundamentals/

video card/SOC/IC controllable by MCU:
No. I could imagine Full HD @ 8bit static display might be just on the verge of what a Cortex M with fast SDRAM or octo-SPI flash is capable of, but not 4x more bandwidth. Even then, the cost of HDMI coverter would ruin the project.
Memory bandwidth and size make little sense to pair with a weak CPU.

Something else I don't see mentioned: Some applications will require a high level of functional safety that IAR meets (IEC 61508)

JTAG like SWD is a debugging interface and is not capable of flashing EEPROM or FLASH chips directly (these require I2C or SPI)

BLE battery powered device should only advertise. Fixed power device should scan

Environment limitations, e.g. would IR RF communication work in direct sunlight?

IoT is a subset of embedded that will bring you more towards OSes like FreeRTOS or even embedded Linux.

Reach out to your manager/new company and ask them what skills for job. We can’t tell you what you’ll be doing, and embedded is vast. You could be doing anything from application level work to kernel/OS side
Yeah no you’re good. This is a good thing to ask. I even ask it in my interviews as a way of trying to show interest and initiative. “If I were to join the team, based on my background, what could I specifically study or improve on to prepare for the role”

If your technical lead/manager is good - as a new hire you're probably going to be spun up on the project and pointed at an existing code base. Your job here will likely be to learn, follow, and improve upon their pre-existing patterns, not break anything, and test and verify existing legacy code. These initial tasks will probably be very specific and there won't be much you can study for ahead of time in that regard.
You will be pretty busy learning the team's workflow, version control pattern, agile/waterfall SDLC, and the various other squishy less technical processes surrounding software development that are just as important as the software development itself.
Your attitude for being prepared is great and I wish more seasoned engineers were willing to review the fundamentals or take them me to read up on new technology and tooling. However I think any initial refreshing you do won't be as useful as you expect. As a software professional you will constantly be learning and relearning everything about your stack while on the job. The larger your tech stack and codebase and the more you move around within it, the harder this brain-maintenance is. This is normal. Your first couple of months, just aim for competency in general but pick a couple of things to really excel at.

I wish i learn how to communicate ideas and how to say "I don't know how to do it" in team environment before entering my first job
When I started, i have the mindset where if I'm incapable to do X job with minimal assistance then I'm a bad employee.
Turns out what it doesn't matter. I can ask for help, and all my team lead care about is whether I can deliver quality code faster.


A good rule of thumb is to pick hardware with at least twice the resources (RAM, flash, CPU cycles etc) you estimate will be required. Running out of resources makes maintenance and adding features very difficult
my rule of thumb was when engineers estimated how long it would take to do something was to double it. it almost never failed me. there should probably be a universal 2x rule :)


AI:
There are I guess 3 main areas:
    General embedded systems that are capable of running simplified models for e.g. Edge-based inference (e.g. Tensorflow->TFLite)
    Embedded systems with dedicated edge/AI accelerators that use their own frameworks (OpenVINO, TensorRT/CUDA, etc.)
    Microcontrollers or other resource-constrained processors running bare metal models (tfmicro, various PL IPs for HLS on FPGAs, etc.)
Each approach has its own pros/cons and plenty of things to work on.
You also have intermediate frameworks that can run the same model on different devices (ONNX, OpenVINO, etc.) in case you have to deal with changes of availability of resources and driving system reconfiguration at run-time.

TODO: allocate time for reading articles (perhaps after running?)
https://animeshz.github.io/site/blogs/demystifying-uefi.html
https://jack-vanlightly.com/blog/2023/5/9/is-sequential-io-dead-in-the-era-of-the-nvme-drive?utm_source=tldrnewsletter

Python for black-box testing

Reading assembly:
Don't worry about being able to write it well, if you need to do that you're doing something fairly specialized like custom RISCV instructions or whatever. Knowing how to read it though? That'll save you a lot of headaches, especially with uC drivers and such.

"If you have a 8bit register, how would you flip the 0th, 3rd, and 5th bits in C"

tools/debug how to read a data sheet (wave form) and compare with an oscilloscope plot/trace.(ie: is the i2c correct? are you using the correct spi mode?

Embedded is all about cramming the most functionality into the smallest viable package with lowest power consumption. Hence, looking at which special-function ICs are trending and which peripherals/processors are trending should give you an idea what's coming to the world of embedded.

RISC-V: https://www.eetimes.com/examining-the-top-five-fallacies-about-risc-v/ 

(https://maaslalani.com/slides/)
Giving presentation on project:
And start your presentation with why you chose this project while emphasizing more on the value it produced
    Requirements (what the product needs to do)
    Pseudocode and System design
    Dependencies listed, assumptions called
    Changes made (not including the code changes as they were made when I worked in another company)
    Reasoning why the changes were made
    Test results
Wanted to see:
Presentation skills - specially explaining tech stuff to non tech or different domain experts, time management
Dev Team wanted to see, code style and use of version control
Overall playground for seeing how I think, design and approach problems

q1) can you draw a block diagram of that system as a whole
q2) ok you did the little part over there in the corner (not the whole thing)
can you show/draw a block diagram of your part in detail. all inputs and outputs
q3) can you intelligently describe the parts involved? who they interact?

FOTA is OTA for ECU(contains an MCU) in automative 


I asked it to write a C program to compute a CRC. The result looked pretty good... but it was fundamentally wrong. It should have asked me what kind of CRC I wanted; instead it produced code for the (pedestrian!) most common sort.
That's exactly what I'd expect from an inexperienced developer. A pro would snort, realize he asked the wrong question, and reframe it with more detail. The result will be (presumably) useful code; the AI acting as a great adjunct, but only when carefully instructed. It's rather like a search engine: ask the wrong question and you get poor results. As I wrote in the last issue, framing the problem is a big part of solving the problem.

Today, you don’t even have to know how to read, perform on an instrument, or write music, if you can whistle a tune, there is software for your cell phone which will write the score and let you refine it and add multiple parts.  You can even publish your music from your cell phone although repeat listeners are not guaranteed.
I believe software is headed down this same path

differential drive, i.e. two separately driven wheels 
zero turn radius, i.e. rotate in place
focal length indicates how long

https://www.youtube.com/playlist?list=PLDqMkB5cbBA5oDg8VXM110GKc-CmvUqEZ

https://philbooth.me/blog/nine-ways-to-shoot-yourself-in-the-foot-with-postgresql?utm_source=tldrnewsletter

https://www.bengreenberg.dev/posts/2023-04-09-github-profile-dynamic-content/?utm_source=tldrnewsletter

HD video streaming over Wifi
Small stuff can be done with an ESP32. But it will choke on HD.
I personally would use an Intel N5105 + Linux + Python + ffmpeg und putting it to Suspend to RAM automatically. There are variants that are able to directly eat 12V.

RESUME: move skills to bottom and have some statement about what you want to do and how you work. Emphasize how you work across the stack

If you have any friends with jobs at companies that hire embedded engineers, ask them for referrals.
TODO: what does a job referral mean?

https://www.reddit.com/r/embedded/comments/12u20oh/37_free_sessions_the_2023_embedded_online/

This list can never be complete, because the technology and the industry are forever changing. 
The most important skill you can develop is the ability to learn new skills. That builds versatility to keep you relevant and marketable in the jobs marketplace for a long, sustainable career. 
List of skills so long because:
Embedded systems comprise a wide range, environments, resources, safety, reliability etc.

IMPORTANT: know MISRA C subset (i.e. so can say embedded C) 

You'll need to learn about both hardware capabilities and software protocol stacks:
    Serial
    USB
    Ethernet
    WiFi
    Bluetooth
    Zigbee
    Cellular

checksum, TLS, CRC, OSI, TLS, AES, PID, feedback loop, flash filesystem

Specific commercial and open-source RTOS's and their internals and support tools.
Like communications technologies, pick a couple to learn about in detail. Understand their configuration and build systems, driver model, thread/task model, concurrency mechanisms, differentiation between kernel and userspace and other privilege and security models, system libraries, BSP's, etc.

TODO: hardware Protocol analyzer (i.e. communications data/packet capture)

They might ask you how they work, where they would be appropriate to use, what tradeoffs there might be among various options, how you would implement them.

FOR PROJECT: Being able to document your work and present it to others is itself an important skill. 

pyserial for HIL

Plenty of people can code and do specialized things, passion is a real winner.
I want to sense strong commitment to learn new things
What will actually let you stand out from others at your level is character and the feeling that you are a good fit and a team player
Keeping engaged and asking questions when I don’t the terminology
"I know things, but stay modest etc."

1.Willingness to adapt and learn their technology.
2.Be Inquisitive and ask tons of questions , challenge existing ideas as well as present your own.
3.Be pleasant to work with and open to criticism from your peers.
4.Show competence in Electronics basics and at-least one programming language.

My employer does 2-week sprints. I apply this process to each ticket in the sprint, to planning and executing the sprint, and to the overall project.
1.Define the problem.
2.Plan a solution.
3.Implement the plan.
4.Test the solution.
5.Document the results, recurse and/or iterate as needed.

tell me about your school projects, what did you do what had problems and how did you fix the problems?
draw me a block diagram of the entire system, then draw a diagram of the part you did and explain how it worked

ROS (robot operating system) experience good also

PLC in industry are hardened over typically SoC (at the cost of lower processing power), e.g. high MTBF, I/Os high protection (optoisolator), 200kg screwdriver dropped on it (environment tolerant)

DESIGN API QUESTIONS (e.g. bit-banging, ISR, multiple threads, DMA, etc.)
While it's not necessarily a bad idea to make buffers for storing longer pieces of RX and TX data the best practice for a UART API would be to expect a caller-supplied buffer for that purpose. Most low-level APIs will only be moving a couple (or just one) byte around at a time.
Sort of a side note: any time you're outlining an implementation with a resource shared between threads you should acknowledge the potential for concurrency issues and throw in some mention of a mutual exclusion primitive (typically mutex will be the go-to unless you're dealing with a pool of resources where multiple can be in-use at a time, in which case you want a semaphore).

The other option is bit-banging which is far less efficient, but may be necessary in certain niche cases. For bit-banging you'll likely end up implementing some form of finite state machine with a hardware timer (hopefully) to keep things aligned with time.

The question is vague, which makes me think it was designed around you understanding the UART protocol, how a micro might have a peripheral that supports UART and how a buffer would then interact with the peripheral.

For an interview, there is no wrong answer per say, but explaining your thought process and understanding it’s flaws is key.
You may have missed the race handling as the ring buffer pointers get updated .. assuming there is an interrupt driven section.

Python CI and CD and:
An example:
we use a scpi based power supply this lets us power cycle the board and cycle through different input voltages while monitoring the power and current
That is just a socket or usb serial interface it depends on the supply
Meanwhile the python code is talking scpi to an tel or hp scope collecting wave forms setting up triggers and taking measurements
And logging this data into a database
And it is commanding the the oven to heat to some temp
It might also tell a vacuum chamber to pump down while cooling stuff to -50 deg c
And the python code might be talking to the device exercising different io interfaces

how to implement a thread scheduler

implementing division with bitwise operations

I’ve seen questions like design a traffic control systems, design a calculator that performs basic math. Candidates are expected to choose sensors, actuators, justify the choice of processor suitable for the system, draw timing diagrams, component diagrams, etc and even make a choice between bare metal or RTOS system. What are other such systems you can think of?

asking you about processor architectures

simple software problem solving in pseudo code

importance of "volatile" keyword

C/asm constructs

ability to read schematics

understanding of and experience with dmms and scopes

Anything related to synchronization between processes, and with interrupts.

Timing, performance, latency, throughput.

Some sense of time scales and data rates.

Some sense of memory size required for various forms of data.

AS INTERN:
Don’t get stuck, ask questions to create forward movement.
Yes, please please please ask questions, whether it would be in person or even within a PR - something like "what is the purpose of this line of code?" isn't to challenge the person but genuinely understand what is going on.

Start reading up a bit on the office culture, and start picking out a couple people you want to talk to. Not specific people, but roles, like product engineer, project manager, quality, etc. Try to plan what you want to learn and how to engage people. I know it's kind of a weird suggestion, but all of my interns generally have a better experience the faster they build relationships with more members of the team

Do: Listen more than you talk. Write notes.

Take notes, ask questions and don't expect to solve all problems you're presented with by yourself. It's important to realize you're there to learn from the experts

In my experience, they barely let intern touch the code but do more maintenance jobs. I say ask questions and ask them to get task on embedded software u can do. They might say no at first but if ur consistent, they will eventually give you a shot. Try to get as many experience you can during ur internship.


An embedded project, with some micro-processor with a design document.
IMPORTANT: What you did to make it work and IT WORKS.

Power supply with SCPI command support

Nice all-in-one portable https://digilent.com/shop/analog-discovery-2-100ms-s-usb-oscilloscope-logic-analyzer-and-variable-power-supply/  

Government will soon impose security and privacy regulations on devices

Error handling policies essential (and static analysis amongst others) to 'prevent fires'

$(fzf) like terminal fSearch

Modbus can be implemented over UART?

fast shutter (when light exposed) speeds essential for non-blur fast motion cameras 

in test mode, will want to have code that intentionally breaks things to enable higher coverage
also for testing analog input, use function generator?

cortex-a adds FIQs?

project where data will be occasionally written to flash under control of our own code. Of course, this exposes us to flash memory corruption during unforeseen power loss events
instead: during Flash programming run code in SRAM and this code will check in a loop the PVD detector state?
Also make sure your micro has the BOR programmed and can handle sudden supply drop off below operating point.
MCUs have multiple voltage operating levels.
At some the flash might not work while the CPU can still kick.

battery charging IC

transformer delivers same power at different voltage
rectifier is AC to DC
When monitoring power, will get 50Hz mains noise and 100Hz noise (ripple/residual AC voltage from rectifier)

The Higher priority task will normally wait for the mutex/semaphore to become available. 
This is usually called "priority inversion". Where a lower priority task has seemingly higher priority than a higher priority task.

To understand priority inversion, imagine a low priority task has acquired a mutex for your I2C bus, and then a high-priority task attempts to acquire the same mutex, but is blocked from doing so by the low-priority task. But now imagine that a middle-priority task starts running, preventing the low-priority task from running and completing its I2C bus access and freeing the mutex. The problem here is that we want the high priority task to run as soon as possible, but it's being prevented from doing so by the middle-priority task, even though that task doesn't use the I2C bus at all.
Different RTOSes provide different approaches for resolving this. For instance, with priority inheritance, the low-priority thread that holds the mutex automatically and temporarily inherits the priority of the high-priority thread that is waiting on it, allowing it to complete use of the I2C bus and freeing up of the mutex, at which point it returns to its normal priority.

deadlock, no one can resolve the issue, e.g:
two threads which attempt to run transfer(a, b) and transfer(b, a) each acquiring locks

When it becomes more than simple IO with a single developer involved, an RTOS provides the structure to write communicating state machines. This allows breaking up the overall firmware into discrete pieces (here's the mqtt state machine.., here's the tcp state machine, etc, here's our sensor dma comms state machine, etc) 
However:
* Stacks are typically statically defined, and easy to overflow and estimate wrong
* Priorities can be inverted if not careful
* Near impossible to test over the complete set of potential states your program can be in
* Deadlocks and memory corruption become more common and harder to find
Discrete communicating state machines are very powerful - however depending on your project, an rtos might take itself out of the running if you can't devise a thorough testing regime.

Noisy signals:
Need to know what kind of noise or interference affects your data.
So, plot a FFT of your signal. 

* Does the noise band overlapp in the frequency domain with the signal band? If no, then a bandpass filter is enough. If yes, then you need to perform noise cancellation. Now it depends if you have a state space model of the measured system, if yes, Kalman filter is the way. If not then something like SVD or certain types of Wiener filters can be used. 
* low-pass: out += (noisy_signal - out) * gain
  where gain is < 1.0, often around 0.1 depending on the sample rate. if your platform doesn't have floats, you'll have to open your bag of integer math tricks
  (One downside is with integer math and a steady state input,you won't ever reach a state where in = out)
* try the "leaky integrator" first over moving average? 
Linear filters (lowpass, bandpass, bandstop - analog or digital)
Model-based signal reconstruction (e.g. Kalman filter)
1. Band pass filter to select the desired frequency range
2. Maybe a mean filter with just 3, 4 in-memory samples to smooth the values but at the same time don't distort large variations in the signal.
3. If you want to perform a roll, pith, yaw estimation, use complementary filter since accelerometer is good for "long-term" compensation while gyroscpoe is good for "short-term" compensation.
(If you have cables sending the signal to your board, without a doubt, that signal should be sent over a differential pair)



I usually use mutex when a resource should be locked like UART or modification of concurrent data. And for kind of communication I use semaphores. Mutex have an interface like lock and unlock. Semaphores have an interface like wait and notify.

Spinlocks are faster and useful to protect a tiny portion of critical section. Mutexes introduce context-switch, so you have more overhead, but are indicated to protect a larger amount of critical section with more than 2 threads accessing it.
Mutex cooperates with the scheduler to put a thread in a sleep state, spinlocks waste the time in a loop, trying constantly to get the lock for the resource

Usually you will see spinlocks in contexts that can't sleep (e.g. interrupts) that is the most common case.


have to communicate the value you gained from each project to the recruiter/engineer that will read your portfolio.

Interpolation is any technique that estimates the value of a sample at a non-measured position based on other measures samples

Clock configuration named differently, e.g. stm32 RCC, msp430 UCS

I am familiar with various peripherals like UART, SPI, I2C , Timer module ,RTC, GPIO and UCS etc.
Familiar with various communication protocols like RS485 MODBUS, TCP/IP and have development stacks for the same.
Have good experience with FREERTOS and I am familiar with various managers like Heap manager, Task Manager, Semaphore, Queues, Event groups etc. 

There is no guarantee that the bit fields will be in the order you expect - they probably will be, but if you switch compiler or platform you might get an unpleasant surprise. Some compilers have options to specify the bitfield order etc, but I don't like relying on that.
The conventional way to do it is define the field values and use bitwise operations (and and or)


addressable LEDs: A shift register has 8 outputs (sometimes lettered ABCDEFGH), a "clock" input and a "data" input. When the clock input changes, it reads the data input line and puts that on the first output, A. The previous value of output A is moved to output B, the previous value of output B is moved to output C, and so on down the line.

If your embedded application runs on battery, it is sometimes more battery-efficient to have a short periodic burst of high-speed communication (such as wi-fi or 5G - that are active for less time and then shut off between transmissions) than a slower, less power-hungry transmission method (LoRa) that must be active for a MUCH longer period of time.
Depending upon your application, Protocols that utilize UDP, RTSP can be FAR more efficient (and faster) than protocols like REST which rely TCP-based transport protocols (which requires a 3-way handshake). As one other commenter mentioned, you will want to decide how much error correction you wish to add and/or if/how you want to ensure lossless data transfer (retries, etc).
Sometimes, basic data compression of a file can be effective.

Various RTOS IPC: Message Queue vs IPC Service vs IPM

Interesting critiques for industrial MCUs:
* Every IoT project I've been remotely involved in has required readback protection to prevent IP theft of the firmware.
* For industrial applications you need at least a range of -40..85 (industrial grade components), and for things that may end in really hot places you'd better go for automotive grade stuff (-40.. 125 Celsius degrees). 

You should not forget the other players than STM32. NXP still has the highest performing microcontrollers at 600+ MHz (although ST is catching up). Texas Instruments has a speciality for analog peripherals and are often used in motor control systems where I work. And then the big players for automotive industry like Renesas and Infineon. The whole industry is pretty much based on ARM Cortex-M now.

Low number of pins, 8bit MCUs like ATTiny202 useful for something simple like a battery gauge indicator, e.g.detecting when the battery gets plugged into a charger or load device, waking devices from sleep, lighting up the indicator, some animations, and going back to sleep when it get's disconnected 

Would be a computer engineer, however don't really care for analog

Seems that for 100K units, ST not reliable. NXP, Microchip, TI better
At least, 32bit CPU issues. Plenty of 8bit CPUs

CR2032 coin cell battery and associated adapter to obtain Vin and Gnd pins?

Interesting 'embedded supercomputers' jetson Xavier NX. Will run embedded linux. Might write a driver for a camera sensor

Mail queues are present in CMSIS-RTOS?

Do you need the ability to program via swd in the field? If not, you can do something similar to some dev kits and put, say, a tag connect header on a snap off. Then when you are done programming and debugging, just snap it off.
Otherwise, just put pads to SWD data, clock, and output (if you need it) on the board and attach wires or use pogos when programming and debugging (similar to some of the Adafruit Feather boards).
I believe either just leaving the pads to use e.g. with pogo-pins / tag-connectors or even the SWD 2x5 pin headers would work
(Pogo pin programming is for when say SWDIO pin is a pad on board not a physically wire insert?)

minimalist debugging, e.g. no UART pins on PCB
I once worked on a system in which the only way to dump info after a crash was via a blinking LED. The SW would bit bash a UART over the LED.
We would attach a circuit with a phototransistor and RS232 driver to send the data to a PC's serial port. There was a magnet in it so that it would stay attached to the panel.

TODO: embedded security:
The Secure Thingz Secure Boot Manager (SBM) provides a robust root of trust for a device, securing the overall boot process, protecting the device against the injection of malicious software and enabling and protecting a secure update mechanism. The SBM will utilise the security and cryptographic capabilities of each particular device. It should be injected into a microcontroller (MCU) at birth, alongside the provisioning of secure keys and certificates.
Early in the development phase, the OEM programs the SBM into the MCU using its preferred method (SWD, JTAG, other, etc) and provisions the MCU’s certificates, keys and security lock bits. At this point, the SBM is immutable and any subsequent "application programming" for the processor must be delivered via the secure process utilizing the boot manager that ensures the application code is signed and encrypted correctly.
Every time the processor transitions through a device reset process, the SBM calculates the hash signature of the application and compares it against the values stored in the signature table (in protected memory) to ensure nothing in the application memory has been tampered with, before running the application.
The SBM also enables a fully authenticated secure software update process. As part of the software update the customer application will download the software update to a separate memory location and will make a software update API call to the SBM. The SBM will reset the MCU, authenticate the update and program the flash with the software. This also enforces anti-rollback when required by the application
A port of the SBM is available for each supported device. The SBM source code is available as part of the Security From Inception Suite.

MSP430 is a Texas Instruments architecture

USBx is an embedded USB stack
FILEx is an embedded FAT stack
lwIP is an embedded TCP/IP stack

omdia research?

TODO: watch embedded+memfault webinars https://go.memfault.com/ota-updates-fleet-management-at-scale
TODO: watch videos from https://embeddedonlineconference.com/ 

Lauterbach Trace32

stm32 chrom-art and neochrom GPU

certain things in C might introduce CVEs but be fast?

https://eternalstudent.xyz/post/links

post embedded project to hackaday.io?

TODO: https://github.com/jubalh/awesome-package-maintainer?utm_source=tldrnewsletter
become debian package maintainer for raylib

COBS for framing. Don't want length tag, as you might start reading a packet mid way
slip encoding to send packets?
serialisation can be just a memcpy struct or protobuf (nanopb) 

TODO: Filters, e.g. low-pass, high-pass
Seems use to reduce data processing, as only have to operate on a subset of data, e.g. <1500Hz? 
You will always need a lowpass filter before you quantize your signals. This filter is called an anti-aliasing filter and will prevent you from sampling high frequencies as low frequencies(shannon-nyquist sample theorem). If you don't have this filter, and your signal contains frequencies above half the sample rate, you will get false signals.

https://www.youtube.com/playlist?list=PLTjcBkvRBqGGbSckyAGLTy05sbPPl6dJA

most open source embedded projects are flight controllers and RTOSs
-------
Yup, always RTOS. Because if you get feature creep, it's a lot easier to add stuff into an RTOS than bare metal.
And if you're bare metal and decide at some point you do need RTOS, that means you start over. I've never seen the reverse where someone wants an RTOS removed.

The caveat would be really low power, really low cost stuff. I make a lot of embedded implementations that are going to do something very minimal, like in 10,000 or more units that cost a few dollars, and it does something like "read an i2c sensor and send it to a level converter to an RS-485 bus" or "send code A when button A is pushed, send code B when button B is pushed"

But, as you say, priorities were handy. Mostly I used lower priority threads for those long blocking operations.
I have seen many applications where people got into a dreadful pickle with complex thread interactions. Also, every thread needs a control block and a stack. That can become expensive, so a mechanism to keep the thread count low is useful.

My application had hard realtime sensor reads at a moderate frequency, with a background 100ms blocking calculation run at 1Hz to process data accumulated in the previous second. Raw data and crunched results were written to FatFS in a third lower priority thread. All comms between threads was through events posted to their respective queues.

My concern is that most people seem to see a gulf between a single task system (typically done with a super loop) and a preemptive system (they often go full thread-tastic). I've seen people advising that every sensor should have its own thread! That's wasteful and complicated.

I liked and stuck to the approach that was taken in my first safety-critical projects: bare metal with a simple scheduler.

Unless you want to use libraries that require using an RTOS, I don’t really see the point of using one.

Closer to bare metal, more control and more stability production-wise as you use less complexity. Further from bare metal you can handle a lot more complexity but harder to achieve stability as the stack can reach EOL and kill you.

Save serial number in flash
Typically implemented in the script that runs the production programmers. You statically allocate a memory address for the HW ID, that is common for all the devices. That way you only compile one version of you fw. It's not uncommon to only flash a boot loader + provisioning app to your device in prod, for the end-user to run a DFU to get the latest FW revision when they start using the devices.
However, some devices:
Some devices already have a unique read-only HW ID register (in MCU or flash etc.), set by the vendor. You can read it in production and use it for QA, provisioning, etc. It will also speed up FA request to the vendor if you have the HW ID on hand.

Some devices have OTP (one-time-programmable) areas in their flash. Unique device identifiers go there.
This is more secure than flash

If you don't have a recovery image then you'll brick the device if you lose power while writing the new firmware in program space.



Tell them HOW you would find it if you don’t know. That’s the important part.

https://apollolabsblog.hashnode.dev/how-to-estimate-your-embedded-iot-device-power-consumption
You need to get a power monitor. Then you can enable different components and see what they pull relative to base draw. 
e.g. establish a baseline for idle system, then enable some component and check the difference from idle.
e.g. log power usage during a wireless transmission
In this case, you won’t actually use a battery. You wire up the system to the power monitor which acts like the battery.

software reset It will (should) behave exactly as if it has just been powered on, except that RAM retains values that were set before software reset. 
if your goal is to avoid reinitializing variables After software reset, then you'll need eeprom to save a reset-counter variable.
no need for an eeprom, simply define a memory region in normal ram as noinit (maybe called differently depending on linker/compiler). the startup code then won't touch it, except you explicitly tell it to do so.
I use this to keep track of error codes, that need to persist a soft reset but gets wiped after a hard reset.
I have done this with IAR (.icf linker files) and also GCC (.ld linker files).

Yes you need what’s called a « watchdog strategy », defining what your watchdog is monitoring and why it’s worth resetting if it goes wrong.
You can either run the watchdog with the highest priority, and then the register will be serviced about a thousand times a second.
Or at the lowest priority, and if ever your system becomes lagged, then it reboots, even if everything works. It all depends what is important for you.


If you can't think of things to use 76 I/O pins for, you haven't designed any real products.
The one I've got a reference up for right now is a controller for an LED hula hoop. 
It has about 30 I/Os - 4 for SPI flash, 8 for the WiFi module's SPI plus control lines and IRQ, 2 for IR comms, 3 for the motion sensor I2C and IRQ, sleep button, 2 battery charger status lines, 3 for LED data and clock, power control signals for the LEDs and IR receiver, USB VBUS detection, battery voltage monitoring, and a serial debug output.
And that's for a device with no parallel interfaces (e.g. LCD). Most of those I/Os don't involve a lot of CPU time. 
Things like the charger status signals can be interrupts, or polled a few times a second. 
Some of them are only needed for proper reset sequencing of peripherals. 
Anything involving high-speed transfers uses DMA - the WiFi module, SPI flash, and LED output can all be running simultaneously at full speed without taking up CPU cycles.

Two days later, you're trying to decide which peripherals can be multiplexed because you have no more pins available.
It's amazing how fast 50 pins needed turns into 120 pins needed. Even simple ICs sometimes give you so many options that they could burn 6 or 7 IOs.


https://embeddedinventor.com/books/ (interview questions)

    if don't know, say what you would assume based on experience, how to find out more

    implementing a ‘time of day’ from a partial data sheet and a single 16 bit timer, 
    down counting so they need to think harder about boundaries and races.

    C like volatile, how to make an FSM, how to manipulate bits etc

    https://rmbconsulting.us/publications/a-c-test-the-0x10-best-questions-for-would-be-embedded-programmers/

    TODO: elecia white 'Interview Questions' at end of chapters

    What is static?

    What is volatile?
What types of issues can arise if you forget the volatile keyword?

    How does an interrupt work?

    What programming practices should typically be avoided in embedded systems, and why?

    Basic circuits questions: Ohms law. Voltage Dividers. Series and Parallel Resistors.

    Compare and contrast I2C, UART, and SPI

    How does an ADC work? How about a DAC?

    Compare and contrast Mutex and Semaphores
A mutex is a binary semaphore, i.e. a semaphore initiated with counter at 1. Mutex for mutually exclusive access, semaphores to manage wait queues. These are the main differences and similarities in a nutshell.

    Linked List algorithm questions

    String manipulation algorithm questions

    Bit manipulation algorithm questions

    Tell me about a hardware issue you debugged.

    Why would you use an RTOS?

    How does an OS manage memory?

What is spinlock? When and how do you use it?

- What is a cache? Why do we need a CPU cache? Cache policies, associativity, ways, cache coherence.

- High speed buses/interfaces and signal integrity.

- MMU & IOMMU. TLB, ASID, page tables, then come questions on memory spaces for a process.

- SoC boot up process. Linux kernel boot up, initrd, initramfs, boot loaders.

- File systems and flash. Wear levelling.

- What would happen if we have unstable DDR or CPU core power supply? How would you debug it

- What is the most common issue you dealt with on embedded system? What is a race condition, priority inversion, starvation, memory corruption, etc

- How would you debug <some issue> (like memory corruption).


    // TODO(Ryan): How can I answer/know: 
    // "Hey you need to guarantee a response to this external command in 40ms, that seems pretty tight."
    // "Sweet, I've got 10x what I need" (thinking 5ms is an eternity)
    // (perhaps just knowing cycle count of CPU and average instructions in ISR?) 
    //
    // "And it frequently comes from someone asking how long it takes this system to boot. Oh around 600 uS"
    //  Most of which is waiting for the power supply to settle down and send the power-good signal...
    //  Or the PLL to lock, or the 32 khz crystal to stabilize
    //
    // This extends to the idea of literally powering a light bulb for a few micro seconds a time each second and sleeping in between  
    //
    // Seems that printf is considered resource heavy: hence lightweight-logging

// fast control loops:
// I ran a timer at 20kHz and did 25us of work in the ISR every time. That was on a 168MHz F4. It was absolutely fine. The system had plenty of steam left for comms, UI and so o
// Depending on the application, you could even spend 50-75% of CPU time in the control loop, with the RTOS only managing the other 25%!
You just need set your control loop interrupt priority so that it is lower priority (higher number on Cortex-M) than configMAX_SYSCALL_INTERRUPT_PRIORITY
There is no need to run the control loop using the RTOS master tick. Just use another timer, then have the ISR signal your control loop thread. You can use this up to 100 - 200 kHz with STM32H7. Above that you need to put the control code in the ISR itself.

    Which endianness is: A) x86 families. B) ARM families. C) internet protocols. D) other processors? One of these is kind of a trick question.

    Explain how interrupts work. What are some things that you should never do in an interrupt function?

    Explain when you should use "volatile" in C.

    Explain UART, SPI, I2C buses. Describe some of the signals in each. At a high-level describe each. Have you ever used any? Where? How? What type of test equipment would you want to use to debug these types of buses? Have you ever used test equipment to do it? Which?

    Explain how DMA works. What are some of the issues that you need to worry about when using DMA?

    Where does the interrupt table reside in the memory map for various processor families?

    In which direction does the stack grow in various processor families?

    Implement a Count Leading Zero (CLZ) bit algorithm, but don't use the assembler instruction. What optimizations to make it faster? What are some uses of CLZ?

    What is RISC-V? What is it's claimed pros or cons?

    List some ARM cores. For embedded use, which cores were most commonly used in the past? now?

    Explain processor pipelines, and the pro/cons of shorter or longer pipelines.

    Explain fixed-point math. How do you convert a number into a fixed-point, and back again? Have you ever written any C functions or algorithms that used fixed-point math? Why did you?

    What is a pull-up or pull-down resistor? When might you need to use them?

    What is "zero copy" or "zero buffer" concept?

    How do you determine if a memory address is aligned on a 4 byte boundary in C?

    What hardware debugging protocols are used to communicate with ARM microcontrollers?

    What processor architecture was the original Arduino based on?

    What are the basic concepts of what happens before main() is called in C?

    What are the basic concepts of how printf() works? List and describe some of the special format characters? Show some simple C coding examples.

    Describe each of the following? SRAM, Pseudo-SRAM, DRAM, ROM, PROM, EPROM, EEPROM, MRAM, FRAM, ...

    Show how to declare a pointer to constant data in C. Show how to declare a function pointer in C.

    How do you multiply without using multiply or divide instructions for a multiplier constant of 10, 31, 132?

    When do you use memmove() instead of memcpy() in C? Describe why.

    Why is strlen() sometimes not considered "safe" in C? How to make it safer? What is the newer safer function name?

    When is the best time to malloc() large blocks of memory in embedded processors? Describe alternate approach if malloc() isn't available or desired to not use it, and describe some things you will need to do to ensure it safely works.

    Describe symbols on a schematic? What is a printed circuit board?

    Do you know how to use a logic probe? multimeter? oscilloscope? logic analyzer? function generator? spectrum analyzer? other test equipment? Describe when you might want to use each of these. Have you hooked up and used any of these?

    What processors or microcontrollers are considered 4-bit? 8-bit? 16-bit? 24-bit? 32-bit? Which have you used in each size group? Which is your favorite or hate?

    What is ohm's law?

    What is Nyquist frequency (rate)? When is this important?

    What is "wait state"?

    What are some common logic voltages?

    What are some common logic famlies?

    What is a CPLD? an FPGA? Describe why they might be used in an embedded system?

    List some types of connectors found on test equipment.

    What is AC? What is DC? Describe the voltage in the wall outlet? Describe the voltage in USB 1.x and 2.x cables?

    What is RS232? RS432? RS485? MIDI? What do these have in common?

    What is ESD? Describe the purpose of "pink" ESD bags? black or silvery ESD bag? How do you properly use a ground strap? When should you use a ground strap? How critical is it to use ESD protections? How do you safely move ESD-sensitive boards between different parts of a building?

    What is "Lockout-Tagout"?

    What is ISO9001? What is a simple summary of it's concepts?

    What is A/D? D/A? OpAmp? Comparator Other Components Here? Describe each. What/when might each be used?

    What host O/S have you used? List experience from most to least used.

    What embedded RTOS have you used? Have you ever written your own from scratch?

    Have you ever implemented from scratch any functions from the C Standard Library (that ships with most compilers)? Created your own because functions in C library didn't support something you needed?

    Have you ever used any encryption algorithms? Did you write your own from scratch or use a library (which one)? Describe which type of algorithms you used and in what situations you used them?

    What is a CRC algorithm? Why would you use it? What are some CRC algorithms? What issues do you need to worry about when using CRC algorithms that might cause problems? Have you ever written a CRC algorithm from scratch?

    Do you know how to solder? Have you ever soldered surface mount devices?

    How do you permanently archive source code? project? what should be archived? what should be documented? have you ever written any procedures of how to archive or build a project? How about describing how to install software tools and configuring them from scratch on a brand new computer that was pulled out of a box?

    What issues are a concern for algorithms that read/write data to DRAM instead of SRAM?

    What is the "escape sequence" for "Hayes Command Set"? Where was this used in the past? Where is it used today?

    What is the "escape character" for "Epson ESC/P"? Where is this used?

    After powerup, have you ever initialized a character display using C code? From scratch or library calls?

    Have you ever written a RAM test from scratch? What are some issues you need to test?

    Have you ever written code to initialize (configure) low-power self-refreshing DRAM memory after power up (independent of BIOS or other code that did it for the system)? It's likely that most people have never done this.

    Write code in C to "round up" any number to the next "power of 2", unless the number is already a power of 2. For example, 5 rounds up to 8, 42 rounds up to 64, 128 rounds to 128. When is this algorithm useful?

    What are two of the hardware protocols used to communicate with SD cards? Which will most likely work with more microcontrollers?

    What issues concerns software when you WRITE a value to EEPROM memory? FLASH memory?

    What is NOR-Flash and NAND-Flash memory? Are there any unique software concerns for either?

    Conceptually, what do you need to do after reconfiguring a digital PLL? What if the digital PLL sources the clock for your microcontroller (and other concerns)?

    What topics or categories of jokes shouldn't you discuss, tell, forward at work?

    Have you ever used any power tools for woodworking or metalworking?

    What is a common expression said when cutting anything to a specific length? (old expression for woodworking)

    Have you ever 3D printed anything? Have you ever created a 3D model for anything? List one or more 3D file extensions.

    Do you know how to wire an AC wall outlet or ceiling light? Have you ever done either?

    Have you ever installed a new hard drive / RAM / CPU in a desktop computer?

    Have you ever installed Windows or Linux from scratch on a computer that has a brand-new hard drive?

    Have you ever "burned" a CD-R or DVD-R disc? Have you ever created an ISO image of a CD or DVD or USB drive or hard drive?

    Have you ever read the contents of a serial-EEPROM chip from a dead system (though EEPROM chip is ok)?

    Have you ever written data to a serial-EEPROM chip before it is soldered down to a PCB?

    How do you erase an "old school" EPROM chip? (has a glass window on top of the chip)

    Describe any infrared protocols, either for data or remote controlling a TV.

    What is the most common protocol is used to communicate with a "smart card"? Have you ever written any software to communicate with a "smart card" in an embedded product?

    What is I2S? Where is it used? Why might you want to use I2S in an embedded system? Have you ever used it?

    What is CAN, LIN, FlexRay? Where are they used? Have you ever used any?

    What is ARINC 429? Where is it commonly used? Have you ever used it?

    What in-circuit debuggers or programmers have you used? Which one do you like or hate?

    Do you know any assembler code? For which processor? What assembler code is your favorite or hate? Have you ever written an assembler from scratch?

    What is "duff's device"? Have you ever used it?

    What is dual-port RAM? Why would it be useful in some embedded systems? What concerns do you need to worry about when using it? Have you ever used it? How?

    Have you ever soldered any electronic kits? Have you ever designed your own PCB(s)? Describe. What is a Gerber file?

    If you create a circular buffer, what size of buffer might optimized code be slightly faster to execute? why?

    Describe how to multiply two 256-bit numbers using any 32-bit processor without FPU or special instructions. Two or more methods?

What is a DSP?  
What are virtual and physical addresses? What is a MMU?

Describe different types of Cache. When do you need to flush the cache, when to invalidate cache?

What is SIMD?

What is a Mailbox register?

What is a Cacheline?

What is a Mutex?

What is Scatter-Gather DMA? What is Ping-Pong DMA?

What is WCET and where does it matter?

What is Lockstep execution?

-------
https://github.com/Serial-Studio/Serial-Studio

https://core-electronics.com.au/guides/identify-electrical-connectors/

battery form factors, e.g. 18650?

image sensor module is the optical to digital conversion part of a camera

TODO: running some functions out of SRAM instead of flash

oscilloscope XY mode to display graphics

Backup power from a capacitor that has just enough power to transfer state to EEPROM in case of power outage?

Linear acutator is motor that moves up and down

Bare metal: https://github.com/cpq/bare-metal-programming-guide 
C-oddities: https://tmewett.com/c-tips/

LiPo require special charging circuitry.
Furthermore, when using typically have either a regulator or boost converter  

IMPORTANT: ALWAYS PROVIDE EXAMPLE!

TODO: raylib game programming (perhaps also for byte jams as well)

TODO: IoT embedded project

TODO: incorporate rss reader feeder.co into morning routine

TODO: memfault debugging with IoT
https://embeddedartistry.com/wp-content/uploads/2022/07/IoT-Device-Observability-Requirements-Checklist.pdf?mc_cid=582b1ec73e&mc_eid=UNIQID

working with MPU: https://www.state-machine.com/null-pointer-protection-with-arm-cortex-m-mpu?mc_cid=582b1ec73e&mc_eid=UNIQID

RTOS debouncing: https://blog.golioth.io/how-to-debounce-button-inputs-in-a-real-time-operating-system/?mc_cid=582b1ec73e&mc_eid=UNIQID

TODO: wolfsound youtube for some audio DSP

TODO: Fun graphical displays
https://tcc.lovebyte.party/
YouTube LoveByte and FieldFX byte jams

TODO: how to best architect timing requirements given that ISR should be small?
e.g. this executes every 1 second, this every 500ms etc.

Thermal imaging coupled with camera more 'deterministic/maintainable' than AI vision

There are of course international standards (IEC 60601, or IEC 62304) that you have 
to follow in order to get your device CE/FDA approval.
These standards usually require to provide a lot of documentation and to pass a 
series of tests in order to verify that your device is working as intended 
and it's not dangerous.

If you put "used Bluetooth" then I would expect you to talk about which transceivers you used, what the api was like (deficiencies it had, how low level, how was it good), and of course a high level overview of where the Bluetooth spec ends and application begins.

Simply bieng able to say "I don't know, but I do know that in the spec/experience it says xyz, so maybe it would do abc, but I would have to look for jkl to be sure." is huge.

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

I only touch low-voltage 5V side, 
the high 'dangerous' voltage side I leave to hardware engineers

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
I2C developed by Phillips to allow multiple chips on a board to communicate with only 3 wires 
(id is passed on bus)
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
boustrophodon:
0 > 1 > 2 > 3 > 4
                |
8 < 7 < 6 < 5 <--
IMPORTANT: LED matrix arranged as boustrophodon columns
if (x % 2 == 0)
   return (x * height) + y;
else
   return (x * height) + (height - 1 - y);

FFT divides samples into frequency buckets
logarithmic scale employed in spectrum analyser to account for high frequency range 
being more greatly separated than low frequency
```

wire strippers +/- affects tension which will affect any wire braiding
put heat shrink on before wire soldering
will often twist multiple common wires (e.g. ground) together and solder to single ground source
(have gloves on for this)
wiring can be too fine for holding a particular amperage?
TODO: look into sleeving wires
TODO: something like loctite threadlocker is a liquid that prevents threads loosening due to vibration 

Adhesive tape (with conductive pads on end) on bottom of LED strip stick wires to. 
Then solder end tips to connect bottom to top
(have some electrical tape for this)
Also, buy wires merged together and only pull apart the end points
With multimeter, verify that no short circuit by checking continuity amongst wires 

Critical section means non concurrent access to this code. 
Acheived with a mutex obtain and lock pair?
```

Modern OS will have code memory write protected for security reasons. 
Bare metal can do this however

IMPORTANT: for a preemptive multitasking kernel like linux, 
a call to pthread_yeild() (allow other threads to run on CPU)
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

MIPI (mobile industry processor interface) developed DSI

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
Growth of sensor compounding, e.g. video now with LiDAR to detect depth, 
gesture detection sensor

H-bridge is IC that switches voltage polarity, e.g. run DC motors forwards or backwards
Rectifier converts AC to DC (transformer is high voltage AC to low voltage AC)

TODO: is something like SerialStudio necessary for visualisation? 
Could not live stream gnuplot?

TODO: domestica course on creative coding.
css, html graphics also for 2D animations?

Marketing refers to some MCU work that react to environment as physical computing

TODO: investigate both AVR github and gists: https://github.com/BobBurns 

CFFI to create a python interface for C for something say like a console session?

TODO: Perhaps for a machine just targeting embedded development, 
use WSL to easily use GUI debugger apps?

TODO: Essentials of putting metadata section in firmware binary, such as version string! 
(particularly so test runners can utilise this information)

Might see GNSS + INS (inertial navigation system; i.e using IMU as well)

TODO: amplifiers, e.g. class-D etc. MEMs accelerometers for vibration detection in cars

TODO: RTC is external to oscillator?

Analog LED is single colour
Digital LED allows controlling each colour separately (so, a.k.a e.g addressable APA102 LEDs)
This is done through an LED chip (think WS2812B LED chip for NeoPixel)

A channel in a sensor is quantity measured. 
So an acceleration sensor could have 3 channels, 1 for each axes

DSP: http://www.dspguide.com/pdfbook.htm
terms like THD (total harmonic distribution), 
PFC (power factor correction), HV (high voltage)

circuit design in software (perhaps parallel with Robert Feranc?): https://www.jitx.com/  

Go through talks on memfault.com blog 

Simulator testing gives much faster turn-around times, 
can add sanitisers without memory concerns, pass peripheral data in from file, draw to window instead of display etc. 

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
Most MCU don't even have multiple cores so adding software overhead 
(or are there additional hardware to emulate cores?)
Even without RTOS, will most likely have some timer and separation of tasks.
Main overall benefit of RTOS is consistent driver interfaces across multiple mcus 
(useful for say complex bluetooth/ethernet stacks etc.)
Most mcus are overpowered for what they do, so using an RTOS is probably a good idea.

tamper response usually done with a button on the board that gets activated when case opens
this will trigger an interrupt handled by RTC (real time clock)

cpu has modes like Stop mode that is a power saving mode.


battery balancing relevent to multiple cells
TP4056 Lithium Battery Charging Board?

verification: requirements
validation: does it solve problem
authenticate: identity
authorise: privelege
token something used to authorise 

HTML not turing-complete, i.e. can't perform data manipulations

robot ➞ renode (why not just qemu with shell scripts perhaps?)
seems that robot/renode parsing of UART cleaner?

embedded systems special purpose, constrained, 
often real time (product may be released in regulated environment standardsd, e.g. automotive, rail, defence, medical etc.)
challenges are testabilty and software/hardware comprimises for optimisation problem solving, e.g. bit-banging or cheap mcu, external timer or in-built timer, adding hardware increases power consumption, e.g. ray tracing card or just rasterisation, big.LITTLE clusters
1/4 scalar performance for 1/2 power consumption good tradeoff

what would a segfault ➞ coredump look like on bare-metal?

C type qualifiers are idempotent, e.g. `const const const int` is fine
sparse + smatch static analysis tools that give named address spaces (near, far pointers)

is ARM nested interrupt by default?

C atomics just insert barriers?

function reentrant if can be swapped out and its execution rentered
functions that operate on global structures and employ lock-based sychronisation (could encounter deadlocks if called from signal handler) (and are thread-safe)
like malloc and printf are not reentrant 

qemu useful over native for when word-size different. also, assembly inspection

TODO: DSP, RTOS, wireless + IoT, battery/power, peripheral protocols (USB, LCD, etc.), CODECS,
optimisations particular to MCUs, assembly knowledge, sql, c++ stl, 
python systems testing (continous integration),
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

michael ee for RTOS: https://www.youtube.com/playlist?list=PLLYZoEqwvzM35p2Kc7bk7bkwxLtTVwpvy 

A real time scheduling algorithm is deterministic (not necessarily fast), i.e. it absolutely must
(soft time is it should)
(real time processing means virtually immediately)
So, a higher priority task will preempt lower priority tasks
FreeRTOS will have a default idle task created by the kernel that is always running
(this idle task gives indication of a low-power mode for free?)

middleware extends OS functionality, drivers give OS functionality

freeRTOS makes money through some commercial licenses (with support), 
middleware (tcp/ip stacks, cli etc.)

TODO: setting freeRTOS interrupt priorities is sometimes done wrong?
tasks are usually infinite loops

freeRTOS is more barebones (only 3 files) and effectively just a scheduler (so has timers, priorities) 
and communication primitives between threads
In most embedded programs, sensors are monitored periodically. Time and functionality are closely related 
For small programs super loop is fine.
However, when creating large programs, this time dependency greatly increases complexity.
So, a priority based real-time scheduler can be used to reduce this time complexity. 
(priority over time-slice more efficient in most cases, as if not operating, can go to sleep)
In addition, schedular allows for logical separation of components (concurrent team development) 
Also allows easy utilisation of changing hardware, e.g. multiple processor cores
COTS (commercial-off-the-shelf) as opposed to bespoke


SEPARATE API CALLS TO AVOID LOW PRIORITY CORRUPTING BY BEING INTERRUPTED BY HIGH PRIORITY
Define a context structure that will hold api call information

typedef struct file_api_params_s
{
    uint8_t api_opcode;
    uint8_t control_opcode;
    uint8_t file_handle;
    semaphore sem;
    void * param1;
    size_t param2;
} file_api_params_t;

Depending on the RTOS you can actually define your messages and message pool ahead of time - or you'll be providing a pointer to the parameter structure in your message
file_api_params_t file_api_pool[n]; //where n = number of tasks that can access the file system + 1;

Each api function does the following:
   - allocates a free param structure from the pool
   - populates the parameters from the api call
   - initializes the semaphore
   - allocates and initializes a message to send to the file system task
   - adds the message to the queue
   - wait for the semaphore

The filesystem task does the following:
   - waits for messages from the message queue
   - checks the message validity and that the sender still is valid (for some systems that have bad behavior if the semaphore api does not check for validity)
   - performs the requested operation
   - increments the semaphore
   - goes back to waiting for a message
So, filesystem task is assigned high priority

If taskA dependent on taskB, taskB should be of higher priority than taskA 



zephyr more of an RTOS a step towards linux with lots of drivers e.g. LVGL, LittleFS, etc. 
(with this comes a lot more configuration hassle)


TODO: zephyr series (perhaps better to do this first as an RTOS as more modern resources than FreeRTOS?)
https://training.golioth.io/
https://blog.golioth.io/zephyr-threads-work-queues-message-queues-and-how-we-use-them/?mc_cid=f6bdf458d1&mc_eid=UNIQID
https://blog.golioth.io/taking-the-next-step-debugging-with-segger-ozone-and-systemview-on-zephyr/

lego technics for gear ideas:
https://www.youtube.com/playlist?list=PLJTMHMAVxQmvTGatIj-X2rJrN8aGJEaQD

want to make something like stargate wheel

first step in embedded debugging commandments;
thou shalt check voltage 
(e.g. check 5V going to LCD by placing multimeter on soldered pin heads)

we can see in x86 (wikichips), instruction and data separate from L2 cache
arm SoC block diagram (datasheet), see d-bus and i-bus to RAM
introduce things like CCM (core coupled cache) 
and ART (adaptive real-time accelerator) that add some more harvard like instruction things
essentially, more busses instead of more cores like in x86, 
(i.e. a lot more than just a CPU to be concerned with)
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

seems that RMII (reduced media-independent interface) 
is a pin layout to connect MAC devices (flexible in implementation). 
Can be implemented to support say an RJ45 connector

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
http://www.ganssle.com/watchdogs.htm
https://vivonomicon.com/
https://patternsinthemachine.net/
https://blog.feabhas.com/
https://blog.st.com/
https://dojofive.com/blog/
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
