# mintz80_main_pcb
Main PCB for MintZ80

# Configuration
CPU clock can be drivven from several sources depending on jumper configuration
* from CPLD clock divider. CPLD has configurable 4 rate clock divider with division rate programmed by two lowest bits of port $D0. To use 256K of RAM with Rev5 and above board, clock divider read circuit had to be removed from CPLD to acommodate circuit for managing A17/RAMEN2. Two lowest bits of port $D0 are used to select CPU clock divider but state of it can not be read back if CPLD is configured to manage more than 128K of RAM.
* externally by supplying clock on SYSCLK pin. In this case short out the pads of capacitor connected to XTAL1 to prevent unwanted oscillations.
<img src="img/mintz80_r5_xtal1_cap.png" alt="Short 15p capacitor on the right">. To drive CPLD clock, oscillator must be used or jumpers configured to connect SYSCLK to CPLD clock in. In PCB revisions prior to Rev5, this means that CPLD pin 2 connected to SYSCLLK signal must be reconfigured as input to prevent signal clash. In Rev5 and up, SYSCLK trace between CPU and CPLD is reversible via jumpers.
* from crystal oscillator of 2x required frequency connected to the CPU. This means that to run CPU at 10MHz, a 20MHz oscillator needs to be installed. In this case both 15p capacitors need to be populated. In this configuration, comments about supplying clock to CPLD and reversing pin 2 as above also apply.

Clock divider configuration:
* Install CPLD oscillator value 2x (or 4x) desired CPU clock <img src="img/mintz80_r5_main_osc.png" alt="CPLD oscillator">
* Connect CPLD CLK jumper to OSC <img src="img/mintz80_r5_cpld_clk_src.png" alt="CPLD CLK jumpered to OSC">
* In Rev5 and above connect SYSOSC jumper to FROM CPLD <img src="img/mintz80_r5_sysosc_dirb.png" alt="SYSOSC to FROM CPLD">
* Connect SYSOSC to CPLD (SYSCLK to CPLD prior to Rev5) <img src="img/mintz80_r5_sysosc_src.png" alt="SYSOSC to CPLD">
* Connect SYSCLK to CLKIN <img src="img/mintz80_r5_sysclk_src.png" alt="SYSCLK to CLKIN">

# Timers
T23SRC should really be called T0SRC. This is addressed in Rev6. Boards prior to this Rev have T23SRC where T0SRC should be.

SIO A and B baud rate is determined by clock dividers CTC3 and CTC2 correspondingly. CTC channels 2 and 3 can be drivven from:
* their own crystal - mount SIO OSC and leave T0SRC disconnected <img src="img/mintz80_r5_CTC_CLK.png" alt="T23SRC to OSC">
* SYSCLK signal - do not populate SIO OSC! Connect all three pads of T0SRC together. This will connect TRG3 and TRG2 to SYSCLK but also will force TRG0 to be drivven from SYSCLK <img src="img/mintz80_r5_CTC_CLK.png" alt="T23SRC to SYSCLK">
* use a jumper wire to bypass center pad of T0SRC and connect two outer pads. Do not populate SIO OSC! This will feed TRG3 and TRG2 from SYSCLK while leaving TRG0 not connected here.

TRG0:
* Can be fed frequency from SIO OSC or from SYSCLK using T0SRC(T23SRC) jumper pads.
* If jumpered to TRG1  - from the same source as TRG1. A jumper for this was added in Rev6. Prior to this a wire jumper can be added between pins TRG0 and TRG1.
* Lastly, center pad of T0SRC can be wire-jumpered anywhere else on the board as convenient.

In Rev5 PCB, TRG1 only has a jumper to connect it to TC0 output or to TRG1 pin on main connector.This is sufficient for many uses:
* Daisy-chain TRG1 to TC0 to use as clock - use TC0 to output a pulse on TC0 on every millisecond. TC0 will also generate an interrupt at the same cadence to provide millisecond timer. Configure TC1 to generate interrupt for every 10 or 100 pulses received from TC0 and in ISR implement routine to have a clock with 10 or 100ms resolution.
* Drive TRG1 from some circuit on daughter board, e.g. CF card through pin TRG1 and have this trigger an ISR to handle event.

More flexibility is allowed by adding a jumper to drive TRG1 input from the same clock as TRG0. Option to do so was aded to Rev6 board. In Rev5 PCB this is easily done by installing a jumper between pins TRG1 and TRG0.

# Releases
* [Rev 1](REV1.md)
* [Rev 2](REV2.md)
* [Rev 3](REV3.md)
* [Rev 4](REV4.md)
* [Rev 5](REV5.md)





