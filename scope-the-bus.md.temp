240V/50Hz mains.
oscilloscope default noise is mains (200MHz, 1Gsamples/sec as oppose to multimeter which is maybe 10samples/sec so really only applicable for perhaps a logic gate or 0.1hz square wave)
can debug PWM, I2C
Ensure BNC connector is plugged in correctly (affect probe compensation; similar to banana plugs in multimeter not being plugged in correctly)
Continous triggering enables us to view from start point, i.e. static image not free flowing. Will show before and after trigger point, i.e. start in centre of screen
No real issue with oscilloscope blowing up if testing battery powered/isolated DC power supply.
With USB powered, ground must be on ground! Otherwise will short USB and that port will probably break

Normal-mode triggering the best of both worlds

Using RS232 (recommended standard) decoder functionality (there is also SPI/I2C decoding).
https://www.youtube.com/watch?v=SarsWOCMvjg&t=76s
Also investigate PWM  

IMPORTANT: ensure offset dials are correct first, i.e. at 0 so 0 is centre
with menu, end-arrows can still be pressed even if not visible
change to 24mega points for memory when just wanting wave length (not zooming in?)

math -> decoder on; event table on (make sure zoomed out enough to view multiple packets (this will increase memory automatically?); increase baud also)

single-shot triggering contact bounce first then ringing as stablises (makes it a balancing act between selecting triggering level for button press and release)
Multimeter measure power consumption of MCU? (stm32 nucleo boards have convenient IDD jumper)
verify signal ringing (e.g. clock signal), i.e. inspect ramp-up/down (measure time to completely bottom out)
