# Oberheim Matrix 1000

This project documents my efforts to reverse engineer the Oberheim Matrix 1000
CPU interface and develop a modern equivalent.  

## Hardware

- cpu       (MC6800 @ 2Mhz)
- code rom  (27256 32Kb)
- patch rom (27512 64Kb)
- DAC       (DAC312 12bit)
- Timers    (82C54 3x16bit)
- UART      (MC68B50)
- VOICE     (CEM3396 Curtis DCO)


## Files

- images
  - 27256.bin (code ROM)
  - 27512.bin (patch ROM)
  -     *.txt (user readable hex dump)


- dasm
  - 27256.lst (code ROM disasembly listing)
  - 27256.sym (DASMx symbol file)
  - ida.lst   (quick dasm with IDA eval)


## Tools

- DASMx [http://myweb.tiscali.co.uk/pclare/DASMx/]


## 6809 Memory Map

This memory map was gleened from the address decode logic shown in the
schematics in service manual.  Its almost certainly incorrect.


```
0x0000
    I/O*
    U819: {
    0x0000 T1*
    0x0400 T2*
    0x0800 T3*
    0x0c00 T4*
    0x1000 DAC
        0x1000
            $ U712 - 74LS374 - DAC HI 7
            b00000001 - DAC 5
            b00000010 - DAC 6
            b00000100 - DAC 7
            b00001000 - DAC 8
            b00010000 - DAC 9
            b00100000 - DAC 10
            b01000000 - DAC 11
            b10000000 - FASTX

        0x1010
            $ U713 - 74HC138 - DAC LO 5
            b00000100 - U715 - 74HC138 - S&H Enable
            b00001000 - DAC 0
            b00010000 - DAC 1
            b00100000 - DAC 2
            b01000000 - DAC 3
            b10000000 - DAC 4

    0x1400 UORV*
        $ U809 - 68850 - UART
    0x1800 SW*
    0x1c00
        U820: {
        0x1c00 L1*
        0x1c20 L2*
        0x1c40 L3*
        0x1c60 MISC*
            $ U818 - 74LS174 - 6bit latch {
                b000001 - VA13
                b000010 - VA14
                b000100 - VA15
                b001000
                b010000
                b100000
            }
        0x1c80 LED1*
        0x1ca0 LED2*
        0x1cc0 LED3*
        0x1ce0 LED4*
        }
    }

0x2000
    $ U803 - 27512 (Patches) {

        Bank Select - VA13, VA14, VA15
    }
The code rom is
0x4000
    $ U802 - 27512 (Expansion) {

        Bank Select - VA13, VA14, VA15
    }

0x6000
    $ U8012B - 43256 {
        RAM
        Bank Select - VA13, VA14
    }

0x8000
    $ U809 - 27256 {
        0x8000 Rom Base (27256)
        0x8003 Reset Handler
        0x84b4 IRQ Handler
        0x85e3 FIRQ Handler (Serial IRQ)
        0xFFF0 Vector Table
    }
```
