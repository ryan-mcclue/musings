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
