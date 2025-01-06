# PNC-Mini-QTDragon

Based on [the awesoe work by Expatria](https://github.com/Expatria-Technologies/Flexi-Pi) go support them!

This is the linuxCNC config for my [PrintNC](https://wiki.printnc.info/en/home) mini.
Whilst the machine is based on the PrintNC, in particular the mini, it has been modified.
It uses:
- [Expatria Flexi-HAL controller](https://github.com/Expatria-Technologies/Flexi-HAL))
- Raspberry Pi 4 connected directly to the Flexi-HAL
- Waveshare USB to modbus (My early Flexi-HAL reviision has issues with modbus)
- Invertek OptiDrive E3 VFD

## Tool setting

This config does not use the standard QTDragon tool length settinng.
I don't have an ATC setup, so I use the scripts defined in `subroutines/dc-tool-change.ngc`
and `subroutines/dc-tool-job-begin.ngc`.
The workflow relies on having both a normally open touch probe, and a normally closed toolsetter.

Workflow:
- At start issue `M600`
- Issue `M6 T99` and load the touchprobe.
- The touchprobe will have its length measured functioning as the reference tool.
- Use the probe to set your work coordinate offset. If using the probe screen make sure Auto tool zero is active.
- Take the touchprobe out of the spindle and load and start your program (assuming it starts with an M6 T##)
- The toolprobe routine will use the difference in length between eachloaded tool and the first tool loaded
  after the M600 (the touch probe) to work out Z-offset.

**Warnings**
Adding the `M600; M6 T99` as a macro to QTDragon for some reason doesn't work and will break everything.
Just issue it as an MDI command.

I haven't tested this super extensively so use at your own risk.

Make sure you modify all settings like lengths etc for your machine. Settings for the toolchange are
in the `subroutines/dc-tool-change.ngc` file.