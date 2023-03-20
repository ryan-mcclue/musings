<!-- SPDX-License-Identifier: zlib-acknowledgement -->

https://marketplace.fedevel.education/itemDetail.html?itemtype=course&dbid=1569757838995&instrid=us-east-2_KpwYC7yK5:45f6c01d-ccc8-43e0-8f33-c5a70caf707f

when creating lid lips, factor in printer tolerance, say 0.4mm

Spreadsheet (have dimensions to make parametric):
if want to access parameteric sketch constraint, <<Sketch>>.Constraints.name
(IMPORTANT: even though variable, make sure to reference it off the sketch)
GIVE OPTIONAL NAME SO CAN REFERENCE FROM .Constraints!
(formula typecasting may be necessary, e.g. (value*1mm))
  * printer_tolerance
  * container_l/w/h
  * board_offset_x/y
  * case_thickness
  * base_thickness
  * hole_diametre_percent_ratio (e.g. <<Internal>>.Constraints.height * hole_diametre_percent_ratio)
  * global_chamfer

1.75mm PLA (polylactic acid) into 0.4mm nozzle

0.15mm layer height speed/quality
15% infill gyroid integrity/speed/usage

supports required for model that does not print over itself

export g-code (like printer ISA?) to sd card 

supports necessary for complex geometries like overhangs, greater than 45°, etc.
put supports everywhere by default? (perhaps add blockers to this?)
maybe just use everywhere to get a feel for it, then manually add enforcers?

can also use printrun usb-b interface to print without SD card?


--------------------------------------------------------------------------
most solder has flux core (typically rosin) to prevent oxidation (wets the metal?)
if we were to leave a blob of solder on the iron whilst hot, it would oxidise and become visibly flaky

Sn/Pb (60/40) lower boiling point and shinier finish (cone shaped) then non-leaded.
fumes are flux as boiling point of lead (≈1700°C) much higher 
lead is if course bad if ingested
lead-free developed due to landfill leeching of disposed electronics

like most metal products, the iron has core (Cu, Fe) and plating metals (Cr, Sn)

fume extractor at top


wire guage is diametre
use solid wire as easier to work with than stranded (which handles flexing better, so better for final projects)

when heated, oxidation of solder iron tip is greatly accelerated thereby losing conductivity, 
wetting, etc.

rusting is a form of corrosion (damage slowy by chemical reaction)
as rust is technically iron oxide, only iron can rust (giving electrons)
however, other metals can corrode
e.g. less reactive copper corrodes more slowly
e.g. more reactive aluminium corrodes, however the oxide is tough and not flaky

silicon mat, brass wool (routinely removing stuck solder debris and rust), 
chisel-head (conical head other 10%),
gloves, glasses

Although boiling point of leaded solder is 180, 400 as wanting to transfer heat to junction as well
450 for non-leaded

soldering station has feedback/adjustable/regulated temperature

## Tinning:
First use tin by touching tip with solder and thrusting into wool. Repeat twice

Future use tin whenever the iron starts looking anything less than clean.
Also tin the iron before finishing and then turn off

Tinning is coating the tip with the solder to reduce the rate of oxidation in the presence of heat.

IMPORTANT:
A 'tip tinner' is something you want to avoid until there is no other option.
They are a corrosive agent designed to corrode the oxides and expose the underlying metal

## Cleaning:
Use regularly to clean away contaminants (to remove dirt/grease will require cloth or steel brush) 
You do not use brass wool as an abrasive in any way
(do after every component?)
There should still be some solder on it.

## Action:
heat materials with iron and apply solder to junction

Hold iron against blob shortly, than slightly pull iron up when removing?

## THT
1mm gauges
components typically higher thermal capacity, ∴ higher temperature

## SMD
flux pen
0.6mm gauge

grounding strap with 1Mohm resistor to ensure same potential as board for sensitive electronics
(not really necessary for dev-boards)
