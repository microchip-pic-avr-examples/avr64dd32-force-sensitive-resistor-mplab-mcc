<!-- Please do not change this logo with link -->

[![MCHP](images/microchip.png)](https://www.microchip.com)

# Pressure Demo with AVR64DD32

<!-- This is where the introduction to the example goes, including mentioning the peripherals used -->
Demonstration showing the harder the force-sensitive resistor is pressed, the more lights come on. The pressure is also sent out a UART interface that can be viewed locally. The AVR64DD32 contains the ADC and UART required to run this demonstration, called core-independent peripherals (CIPs). The LEDs light up in relation to the magnitude of the value the ADC reads. The ADC also internally filters the result by taking 16 measurements and reporting the average of them all, resulting in a smoother reading.

## Related Documentation

<!-- Any information about an application note or tech brief can be linked here. Use unbreakable links!
     In addition a link to the device family landing page and relevant peripheral pages as well:
     - [AN3381 - Brushless DC Fan Speed Control Using Temperature Input and Tachometer Feedback](https://microchip.com/00003381/)
     - [PIC18F-Q10 Family Product Page](https://www.microchip.com/design-centers/8-bit/pic-mcus/device-selection/pic18f-q10-product-family) -->
- [AVR64DD32 Datasheet](https://ww1.microchip.com/downloads/aemDocuments/documents/MCU08/ProductDocuments/DataSheets/AVR64DD32-28-Prelim-DataSheet-DS40002315B.pdf)
- [AVD64DD32 Curiosity Nano Hardware User Guide](https://ww1.microchip.com/downloads/aemDocuments/documents/MCU08/ProductDocuments/UserGuides/AVR64DD32CNANO-Prel-HW-UserGuide-DS50003323.pdf)
- [Curiosity Nano Base Board Hardware User Guide](https://ww1.microchip.com/downloads/en/DeviceDoc/Curiosity-Nano-Base-for-Click-boards-User-Guide-50002839B.pdf)
- [MikroE 4x4 RGB Click](https://www.mikroe.com/4x4-rgb-click)
- [MikroE Force Click](https://www.mikroe.com/force-click)

## Software Used

<!-- All software used in this example must be listed here. Use unbreakable links!
     - MPLAB® X IDE 5.30 or newer [(microchip.com/mplab/mplab-x-ide)](http://www.microchip.com/mplab/mplab-x-ide)
     - MPLAB® XC8 2.10 or a newer compiler [(microchip.com/mplab/compilers)](http://www.microchip.com/mplab/compilers)
     - MPLAB® Code Configurator (MCC) 3.95.0 or newer [(microchip.com/mplab/mplab-code-configurator)](https://www.microchip.com/mplab/mplab-code-configurator)
     - MPLAB® Code Configurator (MCC) Device Libraries PIC10 / PIC12 / PIC16 / PIC18 MCUs [(microchip.com/mplab/mplab-code-configurator)](https://www.microchip.com/mplab/mplab-code-configurator)
     - Microchip PIC18F-Q Series Device Support (1.4.109) or newer [(packs.download.microchip.com/)](https://packs.download.microchip.com/) -->

- MPLAB® X IDE 6.0.0 or newer [(MPLAB® X IDE 6.0)](https://www.microchip.com/en-us/development-tools-tools-and-software/mplab-x-ide?utm_source=GitHub&utm_medium=TextLink&utm_campaign=MCU8_MMTCha_MPAE_Examples&utm_content=avr64dd32-force-sensitive-resistor-mplab-mcc-github)
- MPLAB® XC8 2.36.0 or newer compiler [(MPLAB® XC8 2.36)](https://www.microchip.com/en-us/development-tools-tools-and-software/mplab-xc-compilers?utm_source=GitHub&utm_medium=TextLink&utm_campaign=MCU8_MMTCha_MPAE_Examples&utm_content=avr64dd32-force-sensitive-resistor-mplab-mcc-github)
- MPLAB® Code Configurator 5.1.9 or newer [(MPLAB® Code Configurator)](https://www.microchip.com/en-us/tools-resources/configure/mplab-code-configurator)

## Hardware Used

<!-- All hardware used in this example must be listed here. Use unbreakable links!
     - PIC18F47Q10 Curiosity Nano [(DM182029)](https://www.microchip.com/Developmenttools/ProductDetails/DM182029)
     - Curiosity Nano Base for Click boards™ [(AC164162)](https://www.microchip.com/Developmenttools/ProductDetails/AC164162)
     - POT Click board™ [(MIKROE-3402)](https://www.mikroe.com/pot-click) -->
- [AVR64DD32 Curiosity Nano](https://www.microchip.com/en-us/product/AVR64DD32)
- Curiosity Nano Base for Click boards™ [(AC164162)](https://www.microchip.com/Developmenttools/ProductDetails/AC164162)
- MikroE [4x4 RGB Click](https://www.mikroe.com/4x4-rgb-click)
- MikroE [Force Click](https://www.mikroe.com/force-click)

## Setup

<!-- Explain how to connect hardware and set up software. Depending on complexity, step-by-step instructions and/or tables and/or images can be used -->

Setup the hardware as shown:

TODO: scratch_1.png

Then, configure the software using either a pre-compiled binary, or manually configure everything yourself.

EZ setup (upload the pre-compiled binary):
1. Download repo
2. In MPLAB, File -> Import -> Hex/Elf...(Prebuilt)File
  - Select .hex file from repo (avr64dd32-force-sensitive-resistor-mplab-mcc.X/dist/free/production/avr64dd32-force-sensitive-resistor-mplab-mcc.X.production.hex)
  - Device: AVR64DD32
  - Hardware Tool (make sure Curiosity Nano is plugged in): AVR64DD32 Curiosity Nano
  - Click next, then finish
3. Make sure this project is set as main: right-click project name->Set as Main Project
4. Right-click project->Make and program Device

Setup From scratch - everything is manually configured from scratch, except for 3 files: main.c, and the driver files for the 4x4 LED grid: RGBClick_4x4.c and RGBClick_4x4.h.

1. Create project
 - Setup hardware as shown in the picture above and plug it into your computer
 - File->New Project->Microchip Embedded->Standalone Project
 - Device: AVR64DD32, Tool->Curiosity Nano (the tool option can be selected later)
 - Compiler Toolchain -> select any XC8 compiler (v2.36+)
 - Selection project name/location of your choosing, click finish.


2. Setup MCC configuration (this handles all the hardware specific details)
 - Launch MCC Melody (Tools->Embedded->MCC Code configurator->Select MCC Melody->Finish)
 We now need to setup the system clock, ADC, UART, and pin configuration. After setting all these things, main.c and RGBClick_4x4.c will handle everything else.

 1. Set sysclk to 10Mhz to give head room for the ADC's calculations
  - Project Resources -> System -> CLKCTRL
  - TODO: insert settings picture

  2. Set ADC configuration
  - Project resources -> Drivers -> ADC -> ADC0
  TODO: insert scratch_3.png

  Configuration explanation:
  - Clock pre-scalar:
  - Sample Accumulation Number: 16 results are used because that is the maximum amount the AVR64DD32 supports
  TODO: insert accumulation.png
  From: AVR64DD32 datasheet page 491
  - Sample Length:
  - Free Running Mode:
  - Positive Input Selection: As shown in the picture below, the Force Click's Analog Pin is in the top left, labelled AN. Since it is in the Curiosity base board's slot 2, that corresponds to the Curiosity Nano base board's 13th pin. On the AVR64DD32, the 13th pin is PORTF3, also known as AIN19, thus Analog input 19.

  TODO: pin_selection.png

  3. Set UART configuration
  - Project Resources -> System -> UART -> UART0

  Configuration explanation:

  4. Set Pin Configuration
    - Set pins in Pin Grid View
    - ADC0 -> PORTF3
    - Pins, GPIO Output -> PORTD7
    - USART0, Tx -> PORTD4
    - USART0, Rx -> PORTD5
  5. Set Pin Name
  - Drivers -> System -> Pins
    Set a name of pin PD7 to RGB_LED
    6. Generate code
    - Project Resources -> Generate
3. Insert C files into Project
  - Right-click source files -> add existing item -> main.c, RGBClick_4x4.c
  - Right-click header file -> add existing item -> RGBClick_4x4.h
4. Make and Program Device


## Operation

<!-- Explain how to operate the example. Depending on complexity, step-by-step instructions and/or tables and/or images can be used -->


When you press on the resistor, the LEDs should light up proportionally. This will also verify your hardware is connected properly.

To view the pressure the resistor is reporting, open MPLAB data visualizer.
Under connections, select the COM port the Curiosity Nano is connected to and hit play.
Under the terminal input, select the same COM port. The pressure should now be displaying on the terminal.

## Summary

<!-- Summarize what the example has shown -->
This demo showed:
 - Advanced capabilities of built-in peripherals (CIPs) the AVR64DD32 has such as its ADC.
 - Ease of setting up CIPs through MCC, removing the headache of setting up a new MCU.
 - How to use the MikroE force click and 4x4 RGB Click with a Curiosity Nano Base Board
 - Integrated Data visualizer for viewing UART streams instead of requiring a 3rd party program
