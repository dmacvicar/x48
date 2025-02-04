x48 0.6.4, an HP48 emulator, with a tweak to compile in 2018 on my linux box.

Requires `xfonts-base` (in debian) to be installed for all of the default
fonts to be present. The package name may differ by platform, but you
are looking for the bitmap fonts.

To compile it:

```
./autogen.sh
./configure
make
```

Optional:
```
make install
```

Getting this thing to actually run was an endeavour for me.

First, you need to make the directory `~/.hp48`.
Put your ROM dump (HP48SX or HP48GX) in `~/.hp48/`, and name it `rom.dump`.
I had to (on the first run) run x48 as:

    x48 -initialize -reset -rom ~/.hp48/rom.dump

Then I hit the on button a few times, got no reply, and closed the emulator.

I then re-opened it using just the command `x48` with no additional arguments,
tried hitting 'ON' again, and it suddenly worked. Just getting it initialized
was the hard part.

If you hit segfaults, delete all the files except `rom.dump` in `~/.hp48` and
redo the initialization step.

I have tested this program to work (with the above caveats relating to the first
run) on Amd64 (x86_64) and PowerPC (32 bit) architectures.

The systems I used for testing were a Lenovo ThinkPad X201 and an Apple
PowerBook G4 aluminum 1.33Ghz - both running Debian Sid (unstable).

Original README:

```
x48 -- HP48 CPU emulator
========================

This is x48, an HP48 CPU emulator. This is Version 0.6.1

if the 'LCD' is scrambled, you might try running x48 in one of the 3 ways below
to see if that will solve the problem.

1. run 'x48 +shm'
2. put 'x48*useXShm: False' in you .Xdefaults file (man xrdb)
3. run './configure --disable-shm' to build x48 without the Shared memory extention.

If it does please leave a bug report at https://developer.berlios.de/bugs/?group_id=3335

======

for version 0.4.0

What's new:

	- Implemented CONFIG/UNCNFG more exactly.

	- Added support for different RAM or ROM cards.

	- Added G/GX support (lost of changes)
	  new colors, new labels, ...

	- Rewrote ROMDump for both S/SX and G/GX. Lots faster now.
	  Thanks go to Robert Tiismus <robert@Sneezy.net.ut.ee>

	- Corrected handling of Display and Menu images, fixed number
	  of lines in init_display(), to avoid XShmError on virgin
	  startup

	- Catch SIGPIPE and save state, so everything is saved, when
	  x48 gets terminated by xkill or the window manager


NOTE (from EM48 README by Paul Fox)
===================================

This emulator is capable of providing a faithful replication of the
HP48. In order to do so, it requires a copy of the ROM software
from YOUR calculator. In order to avoid breaking copyright laws,
and upsetting HP, you MUST BE THE PROUD OWNER OF AN HP48 before
running this program. Of course you can run this program without a
copy of the ROM software in order to write trivial machine code
programs but you will not be able to access any of the calculator
functionality.

Instructions on how to download a copy of the ROM are provided
later in this document.


CREDITS
=======


I would like to thank Robert Tiismus <robert@Sneezy.net.ut.ee>
for his help with ROMDump for the G/GX. The ROMDump utility gained
a lot of speed and works for both S/SX and G/GX now.


I used SADHP to disassemble parts of the 48's ROM and some other
programs to test x48, also I very closely looked at SADHP while
writing code for RPL debugging. So here go sincere thanks:

-- Jan Brittenson <bson@ai.mit.edu>
-- Mika Heiskanen <mheiskan@vipunen.hut.fi>

Latest version of SADHP is available from
        wuarchive.wust.edu:/systems/hp/hp48Uploads/
Get it to look deeper into your HP48.


I would like to thank Lutz Vieweg <lkv@mania.robin.de> for his
outstanding bug reports. Must be the crazyness of porting gcc to
the 48, and of writing code in assembler, that digs out all the
bugs in x48. Go on like this Lutz.


I must thank Joe Ervin for his article on keyboard handling.


I also must thank Paul Fox, Author of EM48, for his work on his
emulator, the source code was a great help for solving some problems,
specially the memory bank switching. Thanks Paul.


I would also like to thank Alonzo Gariepy for his contribution to
the world on how the HP28/HP48 works.


I would sincerely like to ask HP about giving me a 48 GX, so the world
could receive more complete x48, also I could use some ROM cards, like
the EQ-LIB to implement an easy use of these. Well, I credited HP in
the last releases, but now x48 really gets running and I still haven't
heard a word from them... 
Just remember, x48 comes under the terms and conditions of the GPL.


HACKERS PLEA
============

I will continue to work on this product and support it where I have
the time, and would therefore appreciate any patches or bug fixes,
etc which any of you make so that I can keep control of the software.

Information in the form of loose english, context diffs, or
suggestions are welcome so that we can all make this product a
little better.


INSTALLATION
============

I improved handling of differences in Solaris, SunOS, HP/UX, and Linux.
Things are not perfect, anyway.

To configure some things for the Makefiles, edit 'config.h' in the 
directory 'x48-0.4.0'. There you can set the compiler, flags to
pass to the compiler, and some other stuff.

To build the emulator do the following in the directory 'x48-0.4.0':

  1. xmkmf
  2. make

This should give you the programs 'x48', 'dump2rom', 'checkrom', and
'mkcard' in the directory 'x48-0.4.0/bin'.

To get the serial line working, you should set up your .Xdefaults for
x48. The resources concerning the serial line are:

x48*useSerial:		True
x48*serialLine:		/dev/ttyS0	; your serial device here, this
					; looks good for Linux.

Run 'x48 -help' for a list of command line options.

Look at 'x48-0.4.0/src/X48.ad' for the whole set of Xresources the
program uses.


HOW TO DOWNLOAD A COPY OF THE ROM
=================================

The emulator works by executing an image of the HP48s ROM. In order
to run the emulator you must have a version of your ROM on the system.

**********************************************************************
* This includes the HIDDEN ROM. Please don't use DUMP programs, that *
* don't dump the HIDDEN ROM. The emulator won't run.                 *
**********************************************************************


To get a memory dump you need to do the following:


- Download the file 'romdump/ROMDump' to your HP.
 
- To capture a complete ROM, start kermit on your computer, set the
  line so it fits your HP, set the speed to 9600 baud and type
  'log session', then 'connect'.
 
- On a HP48 S/SX type '#0h #7FFFFh ROMDump',
  on a HP48 G/GX type '#0h #FFFFFh ROMDump'.
  This will take about 15 minutes on the S/SX, 30 minutes on the G/GX.
 
- When done, type the kermit-Escape (usually CTRL-\) followed
  by 'C' on your Computer. Say 'quit' to the kermit.
 
Your ROM should now be in the file 'session.log'.



Now you have a file containing lines like

#00000:2369B108DADF1008
...

This has to be converted to a binary ROM for x48.

Run the command: `dump2rom session.log`
This will convert your dump into a rom file readable by the emulator
called 'rom.dump'.

CHECK the file with the program 'checkrom'.
Type: `checkrom rom.dump`. It should say:

  ROM Version is HP48-A
  ROM CRC reads 0xcb76 (for Rev. A, will be different for other ROMs)
  IROM OK: ROM CRC test passed.

or

  ROM Version is HP48-R
  ROM CRC 1 reads 0xdfed (for Rev. R, will be different for other ROMs)
  ROM CRC 2 reads 0xf0b1 (                --- " ---                   )
  IROM OK: ROM CRC test passed.

If the test failed, something went wrong transfering the ROM. Don't
start thinking about the size or the nibbles in 'rom.dump'. That's
all correct. Do the Transfer again.

If you know how to do it, you could of course only transfer the
'broken' part of the dump, using e.g. '#60000h #60080h ROMDump'
and fix these lines in 'session.log'.


USING THE EMULATOR
==================

Go into the directory, where your ROM dump resides and type 'x48', or
type 'x48 -rom <filename>', where <filename> is the file containing your
ROM dump.

On the first start the emulator will read the ROM dump.
It will start running at the addr 00000. This will result in the
question 'Try to recover memory?' Answer 'NO'.

To use the emulator, click your mouse at the buttons
on the emulator. You have to press the button down long enough, so
the HP-48 will considder it a keypress. Well, this should work.

To do something like ON-E, or ON-A-F, one would use the middle mouse
button to press ON, then the middle mouse button to press A. These
keys should stay depressed. Now use the left mouse button to press F.
This should release all three keys and yield the desired results.

You can check your ROM dump by pressing ON-E. This enters the selftest
of the HP-48. If it says IROM OK after some time, your ROM CRC has
been calculated correctly.

The SX emulator has 32K RAM and two 128K RAM cards, both read/write.
To access all memory you have to do: 1 MERGE 2 MERGE
The GX emulator has 128K RAM and no RAM cards, yet.
To access all memory you have to do: 1 MERGE 2 MERGE

When you hit the 'quit' button of the window frame (under ol(v)wm),
the emulator saves its state, RAM, ROM, and the ports to files in
the directory '$HOME/.hp48/', so the next time it is started, all
your stuff should be still there.

If the emulator gets messed up, or crashes, enter the debugger
by typing CTRL-C in the shell you started x48. At the prompt type
`reset`, confirm with `y`. Then type `cont`.

If this does not help, enter the debugger again, and type
`exit`, confirm with `y`.  This will NOT save the stuff, so the next
time you use x48, it should be in the same state as on the last run.

Another way to achive this, is to send SIGTERM or SIGKILL to x48.


USING RAM CARDS
===============

To get a RAM card, use the program 'mkcard' to build the card, and 
copy the resulting file to $HOME/.hp48/port1 or $HOME/.hp48/port2.
On the next start of the emulator, the card should be detected and
usable. (Maybe you have to turn it off, and on again...)


KEYBOARD SUPPORT
================

I have added simple keyboard support:

The keys 'A' - 'Z', '0' - '9', '+', '-', '*', '/', 'RETURN', '.', 'SPACE',
'DELETE', and 'BACKSPACE' are mapped to the according keys on the HP-48's
keyboard. The SHIFT keys give you the Orange and Blue shifts. 'ALT' gives
you the `alpha' key, and 'ESC' the `ON' key.
The keys 'F1' - 'F2' also access the menu labels on the HP. The cursor keys
give you the HP-48 keys 'K', 'P', 'Q', and 'R'.

Note that the key 'A' doesn't give you the letter "A" on the HP, but the
button in Row 1, Column 1 on the HP keyboard. The function executed depends
on the Shift State of the HP.

To do something like 'ON-A-F', press and hold `ESC', then press and hold
`A', then press `F' and release all buttons.


TALKING TO THE OUTSIDE WORLD
============================

The emulator shows one line similar to:

  wire:/dev/ttyp2      IR:/dev/ttyS0

right below it's display.

If you have a backup off your real HP-48 on your computer, you
could restore it using kermit:

On the emulator enter the IO menu, hit SETUP, configure to
  wire, binary, 9600, none, 1, 3.

'wire' is the local computer connection, 'IR' talks to the serial
line of your computer. This is strange, but the other way around
it does not work, yet.

Go to the IO menu again. Hit SERVER. The emulator says:
  Awaiting Server Command

On your computer take an unused xterm, go to the directory where
you keep your backups, and start kermit. Configure the line:

type 'set line /dev/ttyp2'    <---- use the number from the line
                                    below the display !!!

This should be the pseudo terminal connected to your emulator.

At the kermit prompt type: 'send backup18.08.1994', or however your
backup was called. There should be communication going.

After the transfer you should have a VAR 'backup18.08.1994'. Hit
the menu key for it. It says BackupHOMEDIR. Type RESTORE. That's
all.


Talking to the serial line, to e.g. another real HP-48 works the
same, just configure the IO to IR.

!!! Note that the communcication speed is 2400 Baud here,
    because the emulator, or better the HP48, thinks he is
    doing IR transfer.


KNOWN BUGS
==========

Very slow, especially the GX compared to a real GX.

Lots of guesses in the devices section of the emulator. I could realy
use the 'SATURN HARDWARE SPECIFICATION' from HP. If anyone has access
to it, send a copy, or tell me where to get it.

Documentation is missing. There should be a file 'x48.texinfo' to
describe the various options and flags of x48.


ACCESS TO THE AUTHOR
====================

Please send any bug reports, context-diffs or suggestions to

ecd@dressler.de

Eddie C. Dost
61, Rue de la Chapelle
4850 Moresnet-Chapelle
Belgium

Have Fun.
```
