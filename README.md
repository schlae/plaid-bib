# The Plaid Bib - an Ad Lib MCA Clone

**This card uses the P82C611 bus interface chip and will not work with faster
MCA systems. For a more widely-compatible version, use the [Plaid Bib CPLD
version](https://github.com/schlae/plaid-bib-cpld).**

The Plaid Bib is a 100% compatible clone of the Ad Lib MCA sound card. This is
an Ad Lib card designed for Micro Channel Architecture machines, such as the
IBM PS/2 family and other compatibles from NCR, Tandy, Dell, and others.

![Plaid Bib photo](https://github.com/schlae/plaid-bib/blob/master/images/PlaidBib.jpg)

Here are the design files. The BOM includes Mouser Electronics part numbers for
everything except for the 82C611, YM3812, and YM3014B, all of which are
available from brokers in China such as UTSource.

[Schematic](https://github.com/schlae/plaid-bib/blob/master/PlaidBib.pdf)

[Bill of Materials](https://github.com/schlae/plaid-bib/blob/master/PlaidBib.csv)

[Fab Files](https://github.com/schlae/plaid-bib/blob/master/fab/PlaidBib-fab.zip)

Use caution when obtaining the P82C611 MCA bus interface part. There is another
chip with the same part number from OPTi, except it is a VLB IDE controller and
totally different.

## Fabrication Notes
This is a two-layer board, outer dimensions 6.452in x 3.475in (164mm x 88mm).
Thickness of the board is 0.063in (1.6mm). When ordering the board, specify
a card edge bevel of 20 degrees. The Dwgs.User layer includes a detail with
the dimensions of the bevel.

Ideally you should use selective gold plating for the edge fingers, but this
can get expensive for small orders. ENIG will work but the gold will rub off
pretty quickly.

For the soldermask color, pick whatever you want! The original was green.

## Assembly Notes
You may wish to socket U7, U8, and U4. These sockets are not included in the
BOM. U7 and U8 are the Yamaha synthesizer chips, and it is useful to be able
to swap them out in case you get a bad one. U4 is the PAL; consider using a
socket in case of programming difficulties.

## PAL Notes
There is a programmable logic device that implements the address decoder and an
address bit latch. The original card uses a 16L8, but the Plaid Bib uses a
readily-available Atmel ATF16V8B. Here is the
[WinCUPL source](https://github.com/schlae/plaid-bib/blob/master/pld/plaidbib.pld)
and the resulting 
[JEDEC file](https://github.com/schlae/plaid-bib/blob/master/pld/plaidbib.jed).
You will need a device programmer such as the low-cost TL866A/CS. 

## Bracket
MCA brackets are no longer available. You have a few options:
- Remove the bracket from an existing card, like a token ring card or something
- 3D print the bracket
- Use the card without a bracket at all

If you want to print your own, here are STEP models of the bracket and the
plastic clip.

[Bracket](https://github.com/schlae/plaid-bib/blob/master/mech/MCABracket.STEP)

[Clip](https://github.com/schlae/plaid-bib/blob/master/mech/MCAClip.STEP)

Please note that neither file is designed specifically for 3D printing; they're
meant to match the real hardware exactly. The clip might print just fine, but
the bracket is probably too thin to print well.

## Compatibility

Micro Channel machines operate at a variety of different bus speeds. Only the
slower ones are compatible with the Plaid Bib.

|PS/2 Model|Clock t(ns)|Default transfer(ns)|Extended transfer(ns)| Compatible?|
|-----|-----------|--------------------|---------------------|----------------|
| 50  | 100       | 300/1WS (I/O)      | 300/1WS             | Yes |
| 55  | 62.5      | 250/2WS            | 375/4WS             | Untested |
| 60  | 100       | 300/1WS (I/O)      | 300/1WS             | Yes/untested |
| 65  | 62.5      | 250/2WS            | 375/4WS             | Untested |
| 70  | 50        | 200/2WS            | 300/4WS             | No |
| 80  | 62.5, 50, 40 | 200             | 300                 | No |
| 90  | 40        | 200                | 300                 | No |
| 95  | 40, 30    | 200, 240           | 300, 320            | No |

The YM3812 FM synthesis chip has minimum timings that are somewhat too slow.
For example, the read pulse width minimum spec is 200ns. The 82C611 generates
the read pulse based on the MCA CMD signal, which asserts for only part of the
default transfer cycle. A fast machine like the model 90 has a default transfer
cycle time of 200ns, so CMD is low for only about 90ns. This is too short to
properly read from the FM chip, meaning that Ad Lib auto detect functions will
fail to see the card.

## Installation
You will need the ADF (adapter description file) in order to set up the Plaid
Bib on your MCA computer.

[Plaid Bib ADF](https://github.com/schlae/plaid-bib/blob/master/@70D7.ADF)

Place the file on a 3 1/2" floppy disk. After you install the card and boot
the computer, it will detect that a new card has been installed and prompt
you to insert the reference disk. Do this and follow the prompts. When the
setup utility asks you to insert the option disk, use the one that contains
the ADF file. After the setup utility configures the card, it will prompt
you to reboot the machine.

Once that's finished, try out the Plaid Bib with your favorite game!

## License
This work is licensed under a Creative Commons Attribution-ShareAlike 4.0
International License. See [https://creativecommons.org/licenses/by-sa/4.0/](https://creativecommons.org/licenses/by-sa/4.0/).
