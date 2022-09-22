<!-- SPDX-License-Identifier: zlib-acknowledgement -->
hotloading
metaprogramming

X11 copy and paste: https://handmade.network/forums/articles/t/8544-implementing_copy_paste_in_x11
font rasteriser: https://handmade.network/forums/wip/t/7610-reading_ttf_files_and_rasterizing_them_using_a_handmade_approach 

Often legacy supported code adds lots of corner cases, e.g. opengl
i.e  don't have consistent performance characteristics

immediate mode and retained mode are about lifetimes. 
for immediate, the caller does not need to know about the lifetime of an object.

discovering for linux docs (xlib, alsa) are code and for 
development may have to add to groups (uinput) 

plugins just be .so files that are loaded with dlsym()

Use of discriminated unions and type fields for type sharing

Visualising essential for debugging insidious problems...
Print rows of text aligned
Conversion from single-state to recording data over time will require memory which we will cast to appropriate state structure, 
e.g. DebugState 
state structures will have `b32 is_initialised` field which will initialise a memory arena
Then have CounterState and CounterSnapshot (move per frame data into this)
Each frame, copy over src to dst (with a rolling buffer)
In the rendering of the records, to display snapshots, generate statistics like min, max, avg.
These will be converged into a Statistic struct for each value
Have helper function for begin, update and end statistic with value
Now we have max, loop over again to generate graph height scale and say red colour scale
Alternatively we could have absolute height scale, e.g `total / 0.033f`
(So in drawing, will typically have 'raw' structures that just have data, and will then
loop over these again to generate relationships to actually draw from etc.)

(TODO: UI have layout and font information...)
Drawing have left_edge, top_edge
Drawing charts, define chart_height, bar_width, bar_spacing etc. in pixels
Can draw single pixel height reference line
Cycle through colours with `arr[index % ARRAY_COUNT(arr)]`

Drawing just take state, and input.
After base drawing, look at input and alter if appropriate

Draw routines with origin vector and vector axis (basis vectors)
Axis that aren't perpendicular causes shearing

artist creates in SRGB space (in photoshop) 
so if we do any math on it (like a lerp), 
will have to convert it to linear space and then back to srgb for the monitor (if we were to just blit directly, it would be fine)
to emulate complicated curves, could table drive it
we compromise on r² and √2
without gamma correction, resultant image will look very dim (due to nature of monitor gamma curve) 
so with rgb values, preface with linear or srgb
