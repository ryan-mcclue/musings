# Resilience

programming in AVR assembly actually made me think fondly of modern technology (a rare feeling indeed)

TODO: MAKES THESE SUBSECTIONS OF PROJECTS

TACKLING DIFFICULT PROBLEM -- HOTLOADING
FILE MODIFICATION TIME BEING READ TOO EARLY.
MISINTERPRETING TIMESPEC NANOSEC

SREG NOT BEING SAVED IN AVR INTERRUPT

linux raw ALSA can fall into the trap of being so niche and difficult it is neck beard inducing

use of set -x in bash scripts will exit if subprocess returns an error, even if 2>/dev/null. Cryptic!

xlib scaled, vsync, refresh rate

ell library
read test files for documentation

linux input analysing SDL2 source

polling multiple keyboards lag due to bug in gnome. inspect gnome git
switched to desktop environment to lxfe 4. also makes creating shortcuts a breeze

Starting from scratch, try to avoid analysis paralysis

Pragmas and binary searching codebase to check where compiler optimises routine wrong

signed comparison/branching/add/sub specifics and code flow jumps in assembly

copy-pasta from UDP socket attempting to access like a TCP. doesn't fail, just hangs on read()

break inside of for loop thinking inside of switch
having a assignment instead of comparison inside of if
unsigned - value causing overflow thereby giving larger than actual
mul instruction copying to r0, r1

compiler giving wrong storage class for function, even though the issue was an unmatched closing parentheses.
example of earlier syntax bug, giving other false-errors

ALWAYS ENABLE ADDRESS SANITISER! (look at niagara user github page)
u8_cursor += byte_counter SHOULD-BE u8_cursor = file_mem + byte_counter;

Having -O2 made program crash as was using memory of stack that in debug mode was never modified
