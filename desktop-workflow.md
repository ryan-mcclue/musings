# Desktop Workflow
time -p; getrusage();

Git usage
Terminal + Editor + Window manager

Configuration files should be copied, not generated (becomes too messy)

I wanted to expose myself to a basic desktop application workflow.
This would include generic programming structures 
as well as modern hardware usage.

The application is a ray caster.

To begin, I ensured that I had a debugger 
from which I could easily step through the application's execution.
In code, I was able to programmatically set system and user breakpoints.

(expand upon the essential code libraries for most projects)

(mocking of syscalls for unit testing with file i/o)

For handling non-fatal errors, single line check. 
For fatal errors, nest all preceding code 
(I have learnt to not be afraid of indentation in this manner).
(error handling in general, i.e. reduce 'errors' by making them part of normal execution flow)

Following the basic principles of non-pessimisation, 
I make a note of the huge amount of cruft in the CRT.
The output buffering, hidden `malloc()` 'optimisations', OS line ending conversions, 
non-obvious use of mutexes etc.
Whilst these may seem like minor inconveniences, they can be insidious for performance, e.g.
`rand()` has a huge call-stack that if we replace with a simple xor shift, 
results in 3x speed up.
Although easy to criticise, it may be the situation that the CRT had to be that way 
because of C standards.
To isolate use of the CRT, wrap in functions so we can hopefully replace with system calls, 
intrinisics, etc.
To avoid the compiler having to generate a large export table of all functions, 
make them `static`
To avoid large amounts of linking, have a unity build.

When performing the common task of grouping data, a few practices to keep in mind.
Use fixed sized types to always know about struct padding 
(in fact, I like to extend this to all my code)

If wanting multiple ways of accessing grouped data, use union and anonymous structs.
Use an int to reference other structs, e.g. `plane_index` 

If the data being grouped can only exist together (e.g. points), use vectors.
The ubiquitity of vectors and normalising them pushed processors
into optimising square root functionality.
Put all structs related typedefs inside their own header file for easy access.

As floats are an approximation, when comparing to 0.0f (say for a denominator check) or negative (say for a square root) use a tolerance/epsilon less-than/greater-than check.
In fact whenever dividing should always ask oneself "can the value be zero?"
To be clear about float to int casting, use a macro like truncate/round (think about what if uneven divide)
Due to mixed integer and float arithmetic going to float, calculate integer percentages `val * 100 / total`
There is no need to overload the division operator as can do `(* 1.0f / val)`

For easy substitution, use single letter prefix names like `output_h` and `output_w`.
Convention for variable arrays, e.g. `Planes plane[1]`, `planes` and `plane_count (use ARRAY_COUNT macro here)` 
Put for loop statements on separate line to help not be afraid of indentation.
Iterate over pixel space and then convert to say, world space for calculations (normalisation and lerp)
Aspect ratio correction is simply rearranging a ratio. 
If we determine one side is larger, scale other.
Use "\r" ASCII code to print a status indicator.
Only use const for char * string literals stored in the data segment.

Endianness comes into play when reading/writing from disk (e.g. file type magic value) and working directly with `u8 *` (e.g. iterating through bytes of a u32) 

Vector and other math (perhaps put in another part)
