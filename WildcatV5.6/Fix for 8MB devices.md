Since a while my DS203 v2.72 8MB was not able to reliably save files, only CFG were working, also filesystem get corrupted very often.
I tried all combinations of:

SYS_B152 

SYS_B160 

SYS_B162 

SYS_B164

JPA Alterbios by patching all the above

formatting the flash with any kind of old dos and windows devices, virtual machines

v113 firmware, B251 firmware and any older release of Wildcat firmware.

Nothing worked, I was close to throw away the scope thinking the flash got broken after 7 years.

Then I fixed a bug in the Alterbios that used to write/read using 512B as block size regardless the flash disk size.
Now it uses 512B on 2M and 4096B on 8M.

Every issue disappeared, I realized it was partially working with CFG because those files were 512B (now 4096B), with other filesizes the filesystem got messed.

Here https://github.com/PetteriAimonen/AlterBIOS/tree/master/Compiled You can find the Compiled folder with all binaries ready to flash, I already patched all 4 SYS versions I found (1.52 , 1.60 , 1.62, 1.64), anyway the 1.64 will work on any hardware release.

Morevover since SYS+Alterbios < 32KB I did a little change to fit both SYS and Alterbios within SYS slot (ALT_F1XX.HEX) so flashing is easier and we won't cause any interference with other projects that use the memory region originally used by alterbios (0x08044000 - 0x08045FFF).  

Installation of original Alterbios is easy:

load Altbios.HEX and Alt_B1xx.SYS files on DFU or

load ADR+BIN of both on DFU or

load BIN with stm32flash (following debrick instructions) 

I use these commands on my linux desktop:

stm32flash /dev/ttyUSB0  -S 0x8044000:0x1fff -w ALTBIOS.BIN

stm32flash /dev/ttyUSB0  -S 0x8004000:0x7fff -w ALT_B164.BIN

I suggest the flash of ALT_F164.HEX which includes alterbios.

As a confirmation of the installation at boot it appears "Alterbios 0.version: OK"

Uninstallation:

Flash stock SYS file.
