# Basic Desktop Workflow - Part 1 ()
I wanted to expose myself to a basic desktop application workflow.
This would include generic programming structures as well as modern hardware usage.
The application is a ray caster.

To begin, I ensured that I had a debugger from which I could easily step through the application's execution.
In code, I was able to programmatically set system and user breakpoints.
(expand upon the essential code libraries for most projects)

For handling non-fatal errors, single line check. 
For fatal errors, nest all preceding code (I have learnt to not be afraid of indentation in this manner)

Following the basic principles of non-pessimisation, I make a note of the huge amount of cruft in the CRT.
The output buffering (and undefined behaviour), hidden malloc 'optimisations', OS line ending conversions, etc.
Often better to cut out the middle-man and just use system calls directly.
To avoid the compiler having to generate a large export table of all functions, make them `static`
To avoid large amounts of linking, have a unity build.
For small scale memory allocations, can use the stack, e.g. `Plane planes[2];`

When performing the common task of grouping data, a few practices to keep in mind.
Use fixed sized types to always know about struct padding (in fact, I like to extend this to all my code)
If wanting multiple ways of accessing grouped data, use union and anonymous structs.
Use an int to reference other structs, e.g. `plane_index` 
If the data being grouped can only exist together (e.g. points), use vectors.
Put all structs related typedefs inside their own header file for easy access.

As floats are an approximation, when comparing to 0.0f (say for a denominator check) or negative (say for a square root) use a tolerance/epsilon less-than/greater-than check.
To be clear about float to int casting, use a macro like truncate/round.
Be aware that mixed integer and float arithmetic will go to float (implications for rounding)

For easy substitution, use single letter prefix names like `output_h` and `output_w`.
Naming convention for variable arrays, e.g. `planes` and `plane_count`
Put for loop statements on separate line to help not be afraid of indentation.
The ability to map from pixel space to another space is essential (normalisation and lerp)
Aspect ratio correction is simply rearranging a ratio. If we determine one side is larger, scale other.
Use "\r" ASCII code to print a status indicator.

Endianness comes into play when reading/writing from disk (e.g. file type magic value) and working directly with `u8 *` (e.g. iterating through bytes of a u32) 

Vector and other math (perhaps put in another part)
