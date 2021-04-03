# Simple Serial Card

## Overview

The Simple Serial Card is a low-cost Super Serial Card work-a-like. I designed
this because I needed a way to use ADTPro on my Apple IIe, and the price of
super serial cards on eBay is getting a bit high for my taste.

This card is more or less 100% compatible with the original Apple card. It
uses the same 6551 ACIA mapped to the same I/O locations, and in fact can
use the same firmware ROM.  It does however have some minor differences in the
interest of cost:

1. There are no DIP switches, so you cannot set the default configuration. It
   does, however, include a jumper that can be removed to disable IRQs.
2. Instead of a DB-25 it is wired to use a DB-9 cable like those used by PC
   motherboards.
3. There is no terminal/modem block to change the serial port wiring. Just use
   the appropriate cable instead, or a null modem adapter.

The KiCAD schematics and PCB layout are included. It is a four layer board,
because my fab house of choice charges the same for 2 and 4 layers, so I went
with what made the layout easier.

## Parts List

* A 74HCT245 octal bus transceiver (U1)
* A 28C256 EEPROM (U2)
* A 1.8432 MHz 5V oscillator (U3)
* A 22V10 GAL (any speed or variety should work) (U4)
* A W65C51N ACIA (U5)
* Two MAX3232 RS-232 line transceivers (U6, U7)
* Seven 0.1uf ceramic bypass caps (C1-C7)
* Eight 1uf 16V electrolytic caps (C9-C16)
* A 2x5 pin header (J1)
* A 1x2 pin header (J2)

## Assembly

Before assembly you will need to burn firmware ROM into the EEPROM, and burn
the appropriat equations into the GAL. See "Burning the ROM" and "Burning the
GAL" below for more details.

I would suggest socketing all of the chips, but at the very least socket the
ROM and the GAL in case you need to update them.

As mentioned above the header on this card is wired the same as a PC COM
header. There are two varieties of these cables available; this card uses the
newer style which is wired 1:1; that is, pin 1 on the header goes to pin 1
on the DB-9, pin 2 to pin 2, etc. These are sometimes referred to as
"AT/EVEREX" style cables.

There is an asterisk on the card's silkscreen indicating pin one on the RS-232
header; make sure this lines up with pin one on your ribbon cable. Usually this
is the red wire on the ribbon cable.

## Installation

Turn off the computer, put the card in an empty slot, and mount the DB-9 on one
of the small cutouts on the back of the computer (or just run the ribbon cable
through an open cutout if you don't care).

## Design Considerations

Many parts choices were influenced by what I have already in my parts bin, even
if that makes the design non-optimal. For example, I used MAX3232 line drivers
because I have a ton of them. With 12V available on the slots I could've used
different parts and avoided all the charge pump caps, but I didn't feel like
buying more chips.

With the exception of the 22V10 GAL used for glue logic all parts in this design
are still actively available. The GALs are out of production but still readily
available on eBay or Aliexpress (though YMMV as far as quality).

## Burning the ROM

A standard super serial card image should work fine with this card, though
since there are no configuration switches the default settings after power on
or reset will be somewhat random.  Once I have working cards built I will be
creating a customized firmware ROM that sets the defaults to 9600 bps for the
ADT Pro bootstrap.

Since 2K EPROMs are nearly impossible to find I subbed in an AT28C256 EEPROM
(again, I had one already). Only the lower 2K is used by the card, so when
burning you can either duplicate the image 16 times, or fill the upper 30K
with zeroes or some other random data. In theory a AT28C64 EEPROM should also
work (and only waste 6K), but I don't have one to test, and a brief search
suggests they aren't any cheaper than the AT28C256 (presumably because they
are out of production).

I use the TL866 II+ USB programmer along with the minipro Linux client for my
ROM burns.

## Burning the GAL

The equations for the GAL are included as a .pld file for the GALasm program.
If you run Linux or another OS that can run GNU make then you should be able
to just type "make" in the "gal" subfolder to compile the equations into a
.jed file.  Burning this file requires a suitable programmer; I personally
use the same TL866 II+ that I use for burning ROMs, though at least at the time
of this writing the minipro client doesn't know how to burn GALs properly, so
you'll need to use the official client software, either on a Windows PC or
under Wine.
