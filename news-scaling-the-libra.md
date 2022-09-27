# NEWS - Scaling the Libra
Xcode cloud subscription. oh no
wasting so much time on naming of new processes, e.g. 'Developer Experience Infrastructure'

Another billionaire investing in a 'utopian cities' seems more dystopic than anything.
Promulgation of venture capitalists, angel investors, etc. 

PIC have weird instruction set, so generally have to use with assembly over C compiler (no good free compiler)
So, use AVR for low-end like just LED driving?
e.g. TLC5971 

time-of-flight sensor like an infrared radar

over-current and over-voltage detectors for when using charging devices, e.g USB-C charger?
so, perhaps investigate/understand voltage regulators?
also have UVLO (undervoltage-lockout)

new USB-PD (power delivery) interface more power from USB-C

I can see machine learning 'appropriate' in say cleaning up random noise,
e.g. brain signals

interesting set of questions to inspect a software engineering workplace:
https://neverworkintheory.org/2022/08/30/software-engineering-research-questions.html?utm_source=tldrnewsletter

things like database accelerator library indicative of normal software not being fast

MOSFETs are a type of transistor.
different transistors for say quick-switching, low signal, high frequency, amplifier etc.

USB-4, PCI5, DDR5 emerging standards.

Could buy a GPU for running interesting machine learning applications like Stable Diffusion (prompt engineers, oh dear...)
https://www.krea.ai/
GPU structure similar to CPUs, e.g have cache, GDDR6 memory (more simple parrellisation)

float-toy nice web visualistion tool

interestingly CRT can scale better than fixed-set resolution LCD

new TV monitor combination QD-OLED
curved monitors less fatiguing as physically matches our eye's shape
flexible/bendable monitors no one asked for...

cool looking completely submerged server desktops. 
pure water is a very good insulator (our tap water will have chlorine for example)
obtained via ozone treatment

the Nintendo DSi implemented augmented reality
virtual reality is completely immersive

how does wireless charging work without a pad, i.e. no induction charging?
interesting power sharing from phone to watch

trie -> gzip. still have to decode and resulting memory same as before compression
succinct data structures are designed to not have to be decoded,
i.e. everything is stored as bits.
∴ uses a lot less memory (however, only for larger datasets)

compound literals issues with debugger and maintanence

C11 static assertions, how to use?
performance improvements?
	static const char sound_signature[] = {
#embed <sdk/jump.wav>
	};
	static_assert((sizeof(sound_signature) / sizeof(*sound_signature)) >= 4,
		"There should be at least 4 elements in this array.");
anonymous structs
int fnc(int arr[static 2])

C23
can prevent VLA by defining constexpr
true, false keywords
nullptr (it's a pointer not number)
enum e : unsigned short {
    x
};
digit separators
function attributes 

smarter devices not necessarily better, e.g. printer's with end-of-life software

lol, ferrocene is a developing rust toolchain

PLC more expensive microcontroller that is more versatile, e.g. handle voltage overload (often used in assembly lines)

removing shared pointers probably gives later code in the pipeline a small cache boost
as all the data is now co-located
excessive logging cause for bad performance
excessive heap usage bad for performance if on an OS
note there is static, stack and heap memory areas

lol, killedbygoogle.com. perhaps not wise to rely on new services

is self-administration the only benefit of an inhaled COVID vaccine?

doesn't seem like quantum computers will be able to solve any practical tasks 
(unless program exploits quantum parrelism to a large extent)

cloud computing with containers growing, e.g. AWS/Azure ➞ Kubernetes ➞ docker

it's sad, but you really could do a stand-up of modern software projects, e.g.
"introducing Goliath, an automatic external dependency manager. 
under the hood we use a Nextrus package manager.
can be scripted with Freasy language extension of Frosty core language"

seems a lot of phones adding satellite connectivity even though it's much slower

although space travel seems like the future, how to cope with serious health affects like space radiation

opening up web pages from tech news sites just awful.
inudated with floating content-blocking video ads, 
permanent marquee ads embedded beween paragraphs and
browser title bar flashing with bot message notification,
bot message popup, cookies accept bar, ...

asciinema useful terminal recording tool with website to host

potential for sim-locking being a thing of the past with a push to eSIM cards

New AI that can edit videos with textual prompts
The dramatic shift in technology that mobile and cloud devices have brought is being realised by natural language processing
As text is seen as a universal input in a lot of Unix programs, interesting possibilities.

eyes convert photons to electrochemical signal
transduction converts one energy form to another
CCD (charged couple device; less noise, more power, lower speed) and CMOS (consumer grade) common camera sensors
digital cameras convert photons to array of pixels, represented as voltage levels
quad pixel camera sensor combines four adjacent pixels in this array

round design of new Nvidia GPU may be evidence for the 20-year cyclical nature of fashion

amazing new 6GHz stock speed Intel CPU

although all crypto is ponzi scheme, it seems the goals of Ethereum to perform secure financial transactions is headed in a better
direction than BitCoin.
furthermore following The Merge, it does not rely on power hungry mining 

avx512 cant actually get performance stated by Intel 'marketing' as it causes heat up and cpu thermal throttles

good to see some work being done on the interopability of digital wallets

in general, Occum's razor approach to muscular issues

smart power homes whereby the source of the power can be discerned, i.e. if it's clean or not. 
extends to phone chargers with knowledge of this

interesting Github Copilot Labs able to do rough translations between languages

good to see (in some ways) hardware vendors pushing for AI standards to allow for greater optimisation

ATM machines can be targeted for card-skimmers

possibility of sprite animation in terminal using chafa tool to create ascii block art

Intel new naming scheme confusing using brand name as category, i.e. 'Intel Processor' instead of 'Pentium'

YouTube no respect for customers, running clandenstine experiment running up to 5 ads at the start instead of spacing them out
Furthermore, Mozilla researchers found that buttons like 'Stop recommending', 'Dislike' have next to no effect

Growing space economy with NASA ISS becoming privatised

Gaming phones with extended fans/cooling and amenable gripping features 

I suppose it would be kind of cool to travel in a plane going Mach 1

There can be such extremes in software, e.g. from programming hardware in FPGA to writing natural-language descriptions for an AI to convert to code

Cool 3D printing pens, although slow, can be used to say repair a chipped brick

Cool USB SAMD boards (possible malware creation)

Amazing creations from AI from still images, videos, digital assistants: https://threadreaderapp.com/thread/1572210382944538624.html 
Companies like DeepMind, OpenAI chatbot
AI used in UI testing
GPU DLSS (deep learning super sampling) is AI upscaling

Finally, alleviating ambigious USB 4.0 v2.0 naming scheme with devices having clear
USB 40Gbps, 240W printed on them

Compelled to investigate a service like paperspace in order to run OpenAI and other AI projects

Promising Framework laptop build to easily repair in mind. However, subpar experience

Teachers are fired for moonlighting

Record breaking DDos attacks, 17.2million requests per second, 3.4terabits per second, 809million packets per second

Google have size to challenge Dolby with new HD audio standard.
However, whilst seeming altruistic, is just so they don't have to pay Dolby licensing fees in their hardware 

Interesting Domain Brokerage services to allow you to get already used domains

Amount limiting will not work to prevent MFA fatigue attacks

Record-breaking figures acheived with overclocking can be decieving as may employ high-end coolers or even liquid nitrogen 

Still Windows updates break things, e.g. NVidia drivers

The inundation of Javascript web frameworks has provided a learning point for adopting new technologies.
In general, stick to familiar technologies and only adopted bleeding edge later

sextempber cornucopia of condoms
don't have to be nostradamus to work out parents parking
in effect, most people's journey to learning is bespoke
my engineering mind (which has been molded by experience) results in largely ad hoc responses 
many positive social dealings at university are unfortunately pyrrhic
