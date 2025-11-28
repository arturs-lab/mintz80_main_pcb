# mintz80_main_pcb
Main PCB for MintZ80

# Configuration
* CPU clock can be drivven from several sources depending on jumper configuration
- externally by supplying clock on SYSCLK pin. In this case short out the pads of capacitor connected to XTAL1 to prevent unwanted oscillations.
<img src="img/mintz80_r5_xtal1_cap.png" alt="Short 15p capacitor on the right">
To drive CPLD clock, oscillator must be used or jumpers configured to connect SYSCLK to CPLD clock in. 
- from crystal oscillator of 2x required frequency connected to the CPU. This means that to run CPU at 10MHz, a 20MHz oscillator needs to be installed. In this case both 15p capacitors need to be populated.
- from CPLD oscillator or clock divider. Clock divider does not fit in CPLD when A17 is generated. So either CPU is connected directly to CPLD oscillator or board is limited to 128KB of RAM.
-- clock divider configuration:
--- Install CPLD oscillator value 2x (or 4x) CPU clock <img src="img/mintz80_r5_main_osc.png" alt="CPLD oscillator">
--- Connect CPLD CLK jumper to OSC <img src="img/mintz80_r5_cpld_clk_src.png" alt="CPLD CLK jumpered to OSC">
--- Connect SYSOSC jumper to FROM CPLD <img src="img/mintz80_r5_sysosc_dirb.png" alt="SYSOSC to FROM CPLD">
--- Connect SYSOSC to CPLD <img src="img/mintz80_r5_sysosc_src.png" alt="SYSOSC to CPLD">
--- Connect SYSCLK to CLKOUT <img src="img/mintz80_r5_sysclk_src.png" alt="SYSCLK to CLKOUT">

* SIO A and B baud rate is determined by clock dividers CTC3 and CTC2 correspondingly. CTC channels 2 and 3 can be drivven from:
- their own crystal - mount SIO OSC and connect T23SRC to OSC <img src="img/mintz80_r5_CTC_CLK.png" alt="T23SRC to OSC">
- SYSCLK signal - connect T23SRC to SYSCLK <img src="img/mintz80_r5_CTC_CLK.png" alt="T23SRC to SYSCLK">

# Releases
* [Rev 1](REV1.md)
* [Rev 2](REV2.md)
* [Rev 3](REV3.md)
* [Rev 4](REV4.md)
* [Rev 5](REV5.md)

