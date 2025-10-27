# Introduction

This Econet Podule allows you to add Econet to your Acorn RISC PC or A7000.

The Podule has four main hardware sections:
  
* [Flash & Address Decoding](#Flash_&_Address_Decoding)
* [Econet Module](#Econet_Module)
* Econet Clock
  - [WFF Implementation](#WFF_Implemenation)
  - [Pico Implementation](#Pico_Implemenation)
* [Termination](#Termination)

By adding the Clock / Termination to the PCB, and by populating all four DIN5 sockets, the Acorn RISC PC or A7000 can act as an 'all in one' File Server solution when used along with Acorns Level 4 File Server software. No external clock, termination, or network infrastructure required. Just plug your client Econet enabled machine to one of the four DIN5 sockets on the rear of the board.

Some general notes about the different hardware sections, and RISC OS configuration, are provided below:

## Flash & Address Decoding
The Flash & Address Decoding hardware must be installed in all cases. Flash memory ICs SST39SF010, SST39SF020 or SST39SF040 (or equivalent) can be used with this module. Flash IC contents, `eco_flash` are included in a compressed .zip file within this repo. The unzipped `eco_flash` code should be written to the flash IC using a suitable programmer.

## Econet Module
The Podule PCB is provided with a 5 and 17 pin header, so you can plug in an ADF-10 or AEH-52 (or equivalent) module into the podule, instead of having to solder on all the Econet module components.

Econet Data Tx, Data Rx and Clock LEDs are present on the PCB. However, these will only work if you plan to add the other Econet components directly to the PCB. They won't work if you plan to use an ADF-10 or AEH-52 module.

If installing the Econet components directly onto the PCB, then one of the two channels of the line driver IC7 will be used by the onboard Econet clock circuit. If not installing the Econet clock circuit, it is still necessary to install resistor R31. This will disable the clock channel on the line driver, preventing it from interfering with any external clock.

## Econet Clock - WFF Implementation
The Econet clock is based on an original design by Simon Inns. Details can be found [here](https://www.waitingforfriday.com/?p=19).

The clock circuit has been modified slightly, so instead of driving the external hardware directly from the PIC, it is driven by the second channel on the Econet line driver that is otherwise spare.

If using an ADF-10 or AEH-52 module, instead of soldering the Econet parts directly onto the PCB, the second channel is unavailable to the Econet clock hardware. In this instance, if planning to install the onboard Econet clock circuit, it will be necessary to also install IC7 and resistor R25. The purpose of R25 is to disable the unused data channel on the line driver, preventing it from interfering with the network.

When the clock hardware is installed, channel 4 of DIP switch, SW1, can be used to enable or disable the clock hardware. When open circuit, the clock hardware is disabled. When short circuit, the clock hardware is enabled.

The PIC has two clock output signals that can be used to drive the external hardware directly, bypassing the clock channel of line driver IC7 completely. Jumper bank J6 can be used to connect the two clock signals from the PIC directly to external hardware. This is not a recommended configuration. However, if using these jumpers, then the clock channel of IC7 (if fitted) must be disabled.

Jumper JP5 is used to select which of the two clock output signals from the PIC is used to drive the clock channel of line driver IC7.

## Econet Clock - Pico Implementation
Provision has been made to connect a Pico based clock via a 2x20 pin header. This was primarily designed to accept a Pico board that was originally developed for other purposes. See [here](https://stardot.org.uk/forums/viewtopic.php?t=24932).

Like the WFF implementation, this solution also relies on the second channel of line driver IC7, so it needs to be fitted, along with R31, for this clock to work.

When this Pico clock module is plugged in, the second channel of line driver IC7 is enabled automatically (via Pico module Pin 20, which is tied to GND). It can only be disabled again by unplugging the Pico clock module. This would need to be done if using an external clock.

## Onboard Termination
An Econet network needs to be terminated to operate reliably. On larger networks this is typically done at the far ends of the network. Often, on these larger networks, an SJ Research clock is used. These clocks have a special circuit that allows special 'phantom powered' terminators to be used at the far ends. These special terminators are not compatible with the WFF clock or Pico clock used on this board. Instead, if using the onboard clock, then the onboard powered termination should be enabled. Jumper bank J3 is used to enable of disable the termination. Remove the jumpers to disable the termination.

## Configuring RISC OS
Please refer to [this](https://stardot.org.uk/forums/viewtopic.php?p=409582#p409582) post which provides guidance on configuring the RISC PC / A7000.

## Author

The RISC PC / A7000 Econet podule is developed and maintained by Ken Lowe.
    
## Hardware License (Creative Commons BY-SA 4.0)

Please see the following link for details: https://creativecommons.org/licenses/by-sa/4.0/

You are free to:

Share - copy and redistribute the material in any medium or format
Adapt - remix, transform, and build upon the material
for any purpose, even commercially.

This license is acceptable for Free Cultural Works.

The licensor cannot revoke these freedoms as long as you follow the license terms.

Under the following terms:

Attribution - You must give appropriate credit, provide a link to the license, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.

ShareAlike - If you remix, transform, or build upon the material, you must distribute your contributions under the same license as the original.

No additional restrictions - You may not apply legal terms or technological measures that legally restrict others from doing anything the license permits.


## Schematic and 3D Render
Econet Podule - Schematic:
![Econet_Podule_Schematic](https://github.com/user-attachments/assets/3d9549ed-f939-44f4-a854-83617e436b97)

Econet Podule - 3D Render:
![Econet_Podule_3D_Render](https://github.com/user-attachments/assets/9ff163a5-c931-4732-b8fc-92ea635663b0)
