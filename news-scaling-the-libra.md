# NEWS - Scaling the Libra
Xcode cloud subscription. oh no
wasting so much time on naming of new processes, e.g. 'Developer Experience Infrastructure', 

Another billionaire investing in a 'utopian cities' seems more dystopic than anything.

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
GPU structure similar to CPUs, e.g have cache, GDDR6 memory (more simple parrellisation)

float-toy nice web visualistion tool

interestingly CRT can scale better than fixed-set resolution LCD

new TV monitor combination QD-OLED
curved monitors less fatiguing as physically matches our eye's shape
flexible/bendable monitors no one asked for...

cool looking completely submerged server desktops. 
pure water is a very good insulator (our tap water will have chlorine for example)

the Nintendo DSi implemented augmented reality
virtual reality is completely immersive

interesting wireless charging...

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

sextempber cornucopia of condoms
don't have to be nostradamus to work out parents parking
in effect, most people's journey to learning is bespoke
my engineering mind (which has been molded by experience) results in largely ad hoc responses 
many positive social dealings at university are unfortunately pyrrhic
