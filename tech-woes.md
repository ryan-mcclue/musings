The diverse software landscape of Linux distros means that we don't know what version of each

STM32CubeMX (this justs generates code, IDE is full fledged) to download, must get link with email.
STM32CubeIDE is woefully slow. Maximising to full screen just blurs out half
Once installed, a series of pop-up menus just keep appearing spontaneously as it has to download more to satisfy a simple create project
To download qtcreator well known bug that it selects the wrong mirror, 3MBps to 30kbs. 
Have to decipher command line arguments and mirror parsing, e.g. ./qt-unified-linux-x64-4.3.0-1-online.run --mirror http://ftp.jaist.ac.jp/pub/qtproject  

My god, installing cubemx is awful. require email. password setting for account fails.

QTcreator does not honour system .gdbinit file, have to manually set breakpoints and dissassembly flavour
QTCreator doesn't show console output. Tick 'run in terminal' crashes on startup

Before even using the program, installing is bad. Install pandoc. It requires a pdf engine, e.g. xelatex that must be separately installed
Which name is not found with traditional `sudo apt install xelatex`, it's in `texlive-xetex` and is 100MB...
Searching for the appropriate package name to install a missing dependency is what I use ubuntu stack exchange mostly for.....

Firefox just forces restart

gpus have introduced a whole host of undocumented NDA annoyances

sound output varies significantly across different hardware. so, to detect possible sound bugs we may need
to invest in a good piece of sound hardware

in linux, x11 is mixer for drm, pulse a mixer for alsa (more flexibility with pulse, 
e.g. can disable it if only needing one application at time, or can redirect it to another sound card)
audio is a mess in linux

due to disconnected nature of x11 and scheduling, must vsync. sleeping isn't an option due default to round-robin
scheduling policy (switching to real-time scheduling is not suitable for general-os).
we could improve by setting niceness (however must be sudo)
as not real time, we can't control a lot of things, e.g. could be USB latency, adobe photoshop in background, etc.
on say raspberry pi could program GPIO pins to have a accurate poll for a joystick.

sound on linux is absurd (tribal knowledge). even the people on the mailing list don't know how it works.
constantly saying, don't do this, use a library...
often times, source code is the documentation, so good luck spending months on how to operate low level.
return when threading is involved.

x11 is more complicated than it needs to be. like many OS commands, it should do a lot of the dirty work for you, as no-one knows all the minutiae.
it's bad to think that OSs are getting harder to work with. however, it is unfair to pick on X11, as their are other apis (even modern)
that are just as bad, even worse.
x11 is particular annoying in that if it crashes, even launching a virtual terminal may not work as keys aren't registered

if only eclispe cdt debugger was good. 
it terminates for no reason when running (can sometimes be fixed by switching to run mode and then back to debug). sometimes have to restart to fix. 
run-to-line doesn't work when inside sub-routine. memory browser crashes on entering address.
doesn't give information on stack overflows.

firefox will unexpectanctly say we updated in the background and you must restart or tab crashed etc.

Online help forums for QTCreator useless. 
In reality, only core devs would be able to answer your question thoroughly
So, this is where I see the benefit of mailing lists

Often Ubuntu audio mixer just stops working and generates fuzz and have to restart 

Firefox facebook video chat. For some reason just mysteriously stops working. says to restart, did that work, nope! both rev every fucking second is probably why.

Unfortunate just resign yourself to choosing the software with fewest or manageable 'quirks' (bugs to offensive...)

How on earth can something so frequently used like TeX have such poor/cryptic parsing error information
Furthermore, pandoc doesn't treat text as text. If different file extension, will perform different parsing e.g. inserts 'hidden' YAML header if markdown file

FreeCAD have to click in particular order for symmetry, e.g. point-line-point

QTcreator so complicated to run cross-debugger, have to write down steps

Tim Berners-Lee envisioned a universal place for information exchange. 
Unfortunately the freedom of creativity has resulted in much of the Web being intrusive 
and hindering

Order of arguments matter in docker, for some reason --rm must be before -it

default firefox credential store overwrote bitwarden password. ugh

Have so many Ubuntu variants when in reality just different packages installed (possibly different kernel parameters), e.g. mate, xubuntu, lbuntu, studio

Ad division of companies seem to have greatest control over design

Go on a news website and try to watch a video. after watching an obtrusive ad, video starts, skip to 30% of way, the same ad repeats...
