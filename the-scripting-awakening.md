# The Scripting Awakening (05/04/2022)
I decided to try and implement ctime in Bash for pedagogical purposes.
My first task was to write and read a binary file. Googling how to do this in Bash returned the consensus, "use another language". Often when I read this, I'm not deterred.
I have encountered similar naysayers before when it comes to directly using Xlib.
However, when it came to wanting structures to read and write to (essential for binary work), I found Bash was empty.
With this understanding, I realised more generally that the usefulness of scripting languages are limited. 
Specifically, they should be limited to basic tasks that involve searching or external tool usage.
I suppose my biggest use case of Bash is for enhancing my terminal (.bashrc) and vim (`:.! `) interactions.
C is a simple language and if you understand it's low-level capabilities, you can do many things.
However, despite this recognition, I did come upon certain procedures to follow when working with Bash scripts.
```bash
# Checking arguments as callee
[ ! $# -ge 3 ] && return 1
[ "$1" = "-arg" ]
# Checking arguments as caller
func || exit 1
# Function returning value
_FUNC_NAME=val
# Informative usage information
printf "App v1.0 by Ryan McClue\n" >&2
printf "Usage:\n" >&2
printf "app -arg <value>\n" >&2
```
