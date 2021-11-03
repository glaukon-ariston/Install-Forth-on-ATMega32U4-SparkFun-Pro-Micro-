# Install Forth on ATMega32U4 SparkFun's Pro Micro
How to install Forth on AVR's ATMega32U4 board (SparkFun's [Pro Micro](https://learn.sparkfun.com/tutorials/pro-micro--fio-v3-hookup-guide/all#hardware-overview-pro-micro) Arduino board)

__warning! work in progress! not verified yet! it is NOT working! (yet)__

## On Windows
- Download [FlashForth](https://flashforth.com/index.html) [here](http://www.sourceforge.net/projects/flashforth)
- Unzip the ZIP archive `ff5.0.zip` into the project directory, e.g. `c:\avr\`
- Download and install Microchip Studio (ex Atmel Studio)
- Locate `avrasm2.exe` in the Microchip Studio installation (e.g. `C:\app\electro\Atmel\Studio\7.0\toolchain\avr8\avrassembler\`) and copy it into the project directory (e.g.  `c:\avr\flashforth\`)
- Locate `m32U4def.inc` (e.g. `C:\app\electro\Atmel\Studio\7.0\packs\atmel\ATmega_DFP\1.6.364\avrasm\inc`) and copy it to the newly created `include` directory under the project directory (e.g. `c:\avr\flashforth\include`)
- Read the `atmega.sh` from the project directory
- Comment out the definitions of U_VID and U_PID in the source, see `avr\src\usbcdc.asm`
- Run `avrasm2.exe -Dffm32u4 -Dop3 -DU_VID=0x1B4F -DU_PID=0x9205 -I include/ avr/src/ff-atmega.asm -o avr/hex/32u4-16MHz-USB_VID1B4F_PID9205.hex -fI`
- Upload the produced `32u4-16MHz-USB_VID1B4F_PID9205.hex` file using the USBasp or similar programmer
- `avrdude -u -c usbasp-clone -p m32u4 -e -U flash:w:"c:\avr\flashforth\avr\hex\32u4-16MHz-USB_VID1B4F_PID9205.hex":a -U lfuse:w:0xFF:m -U hfuse:w:0xD9:m -U efuse:w:0xE9:m`

`avrasm2.exe` log
```log
> avrasm2.exe -fI -Dffm32u4 -Dop3 -DU_VID=0x1B4F -DU_PID=0x9205 -I include/ avr/src/ff-atmega.asm -o avr/hex/32u4-16MHz-USB_VID1B4F_PID9205.hex
AVRASM: AVR macro assembler 2.2.8 (build 80 Jan 14 2020 18:27:50)
Copyright (C) 1995-2020 ATMEL Corporation

avr/src/ff-atmega.asm(34): Including file 'avr/src/config.inc'
avr/src/config.inc(19): Including file 'include/\m32U4def.inc'
avr/src/ff-atmega.asm(35): Including file 'avr/src/macros.inc'
avr/src/macros.inc(39): warning: Register r26 already defined by the .DEF directive
avr/src/ff-atmega.asm(35): info: 'avr/src/macros.inc' included from here
avr/src/macros.inc(40): warning: Register r27 already defined by the .DEF directive
avr/src/ff-atmega.asm(35): info: 'avr/src/macros.inc' included from here
avr/src/macros.inc(41): warning: Register r30 already defined by the .DEF directive
avr/src/ff-atmega.asm(35): info: 'avr/src/macros.inc' included from here
avr/src/macros.inc(42): warning: Register r31 already defined by the .DEF directive
avr/src/ff-atmega.asm(35): info: 'avr/src/macros.inc' included from here
avr/src/ff-atmega.asm(104): warning: Use of undefined or forward referenced symbol 'TXU_' in .equ/.set
avr/src/ff-atmega.asm(105): warning: Use of undefined or forward referenced symbol 'RXU_' in .equ/.set
avr/src/ff-atmega.asm(106): warning: Use of undefined or forward referenced symbol 'RXUQ_' in .equ/.set
avr/src/ff-atmega.asm(343): Including file 'avr/src/usbcdc.asm'
avr/src/ff-atmega.asm(34): Including file 'avr/src/config.inc'
avr/src/config.inc(19): Including file 'include/\m32U4def.inc'
avr/src/ff-atmega.asm(35): Including file 'avr/src/macros.inc'
avr/src/ff-atmega.asm(343): Including file 'avr/src/usbcdc.asm'

"ATmega32U4" memory use summary [bytes]:
Segment   Begin    End      Code   Data   Used    Size   Use%
---------------------------------------------------------------
[.cseg] 0x005b80 0x007f68   6340   2608   8948   32768  27.3%
[.dseg] 0x000100 0x000333      0    563    563    2560  22.0%
[.eseg] 0x000000 0x000000      0      0      0    1024   0.0%

Assembly complete, 0 errors. 7 warnings
```

`avrdude` log
```log
>>>: avrdude 
32u4-16MHz-USB_VID1B4F_PID9205.hex: 8,948 / 32,768 Bytes (27.31%)
>>>: avrdude -u -c usbasp-clone -p m8 
Detected 1e9587 = ATmega32U4
~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~
>>>: avrdude -u -c usbasp-clone -p m32u4 -e -U flash:w:"c:\avr\flashforth\avr\hex\32u4-16MHz-USB_VID1B4F_PID9205.hex":a -U lfuse:w:0xFF:m -U hfuse:w:0xD9:m -U efuse:w:0xE9:m 

avrdude.exe: warning: cannot set sck period. please check for usbasp firmware update.
avrdude.exe: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.00s

avrdude.exe: Device signature = 0x1e9587 (probably m32u4)
avrdude.exe: erasing chip
avrdude.exe: warning: cannot set sck period. please check for usbasp firmware update.
avrdude.exe: reading input file "c:\avr\flashforth\avr\hex\32u4-16MHz-USB_VID1B4F_PID9205.hex"
avrdude.exe: input file c:\avr\flashforth\avr\hex\32u4-16MHz-USB_VID1B4F_PID9205.hex auto detected as Intel Hex
avrdude.exe: writing flash (32616 bytes):

Writing | ################################################## | 100% -0.00s

avrdude.exe: 32616 bytes of flash written
avrdude.exe: verifying flash memory against c:\avr\flashforth\avr\hex\32u4-16MHz-USB_VID1B4F_PID9205.hex:
avrdude.exe: load data flash data from input file c:\avr\flashforth\avr\hex\32u4-16MHz-USB_VID1B4F_PID9205.hex:
avrdude.exe: input file c:\avr\flashforth\avr\hex\32u4-16MHz-USB_VID1B4F_PID9205.hex auto detected as Intel Hex
avrdude.exe: input file c:\avr\flashforth\avr\hex\32u4-16MHz-USB_VID1B4F_PID9205.hex contains 32616 bytes
avrdude.exe: reading on-chip flash data:

Reading | ################################################## | 100% 0.00s

avrdude.exe: verifying ...
avrdude.exe: 32616 bytes of flash verified
avrdude.exe: reading input file "0xFF"
avrdude.exe: writing lfuse (1 bytes):

Writing | ################################################## | 100% -0.00s

avrdude.exe: 1 bytes of lfuse written
avrdude.exe: verifying lfuse memory against 0xFF:
avrdude.exe: load data lfuse data from input file 0xFF:
avrdude.exe: input file 0xFF contains 1 bytes
avrdude.exe: reading on-chip lfuse data:

Reading | ################################################## | 100% -0.00s

avrdude.exe: verifying ...
avrdude.exe: 1 bytes of lfuse verified
avrdude.exe: reading input file "0xD9"
avrdude.exe: writing hfuse (1 bytes):

Writing | ################################################## | 100% -0.00s

avrdude.exe: 1 bytes of hfuse written
avrdude.exe: verifying hfuse memory against 0xD9:
avrdude.exe: load data hfuse data from input file 0xD9:
avrdude.exe: input file 0xD9 contains 1 bytes
avrdude.exe: reading on-chip hfuse data:

Reading | ################################################## | 100% 0.02s

avrdude.exe: verifying ...
avrdude.exe: 1 bytes of hfuse verified
avrdude.exe: reading input file "0xE9"
avrdude.exe: writing efuse (1 bytes):

Writing |  ***failed;  
################################################## | 100% 0.09s

avrdude.exe: 1 bytes of efuse written
avrdude.exe: verifying efuse memory against 0xE9:
avrdude.exe: load data efuse data from input file 0xE9:
avrdude.exe: input file 0xE9 contains 1 bytes
avrdude.exe: reading on-chip efuse data:

Reading | ################################################## | 100% -0.00s

avrdude.exe: verifying ...
avrdude.exe: verification error, first mismatch at byte 0x0000
             0xc9 != 0xe9
avrdude.exe: verification error; content mismatch

avrdude.exe done.  Thank you.
```
