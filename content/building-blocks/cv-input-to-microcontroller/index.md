---
title: "CV Input to Microcontroller"
tags: ["microcontroller", "op-amp", "analogue"]
--- 
Many digital modules will need at least one CV input, however you have likely noticed that while Eurorack can have ±12V travelling through its patch cables, most microcontrollers will be very unhappy to receive anything outside of the range 0-3.3V.
To allow you to 'read' the whole 12V CV range, you need to perform something called 'signal conditioning'.
This means that you want to 'condition' the potential range of ±12V into 0-3.3V, cutting out any parts you don't need to read, and scaling the parts that you do to the readable range.

One excellent circuit for doing just this, has been designed by Émilie Gillet of Mutable Instruments.

### Mutable Instruments' CV Input Stage (used in Braids)
![Mutable Instruments CV Input Stage](mutable-instruments-cv-input.png)

All credit goes to Émilie Gillet of Mutable Instruments.

This circuit has two op-amps, which both perform very important functions.
The first op-amp (a TL07X) is set up as an inverting amplifier, so it is able to have a gain of less than 1 (Non-inverting amplifiers can only have a gain of greater than or equal to 1).

Its gain is -0.25, which will turn +12V into -3V, and -12V into +3V.
We still have a problem that the voltage enters negative potential, and that the voltages have been flipped.

Now op-amps always clip the incoming signal depending on the power supply they are given, but 'rail-to-rail' op-amps clip very precisely, incredibly close to the power rails they are provided with.

Thus, by providing the second op-amp with a power supply of Ground and +3.3V, the outgoing signal has always been clipped to an absolute minimum and maximum of 0-3.3V, so cannot harm the microcontroller's input.

The op-amp used for this is an MCP600X, as it is rail-to-rail, and it is also set up in the inverting configuration, so that it will flip the voltages back to 
positive = positive.

This circuit in its entirety will thus take a signal that potentially is anywhere in the range ±12V, and allow you to 'read' 0-12V, and will safely clip anything below 0V to keep it at 0V. This is of course a limitation of this circuit; that it cannot 'read' negative input CV.

This is an [interactive version of the schematic](https://www.falstad.com/circuit/circuitjs.html?cct=$+1+0.000005+10.20027730826997+50+5+43%0Aa+128+256+240+256+8+12+-12+1000000+0.000023399707503656204+0+100000%0Aa+336+272+448+272+8+3.3+0+1000000+-0.000023399239518865825+0+100000%0Ar+128+176+240+176+0+25000%0Ar+128+176+16+176+0+100000%0Ar+240+256+336+256+0+25000%0Aw+240+176+240+256+0%0Aw+128+240+128+176+0%0Ar+336+208+448+208+0+25000%0Aw+336+208+336+256+0%0Aw+448+208+448+272+0%0Ag+128+272+128+288+0%0Ag+336+288+336+304+0%0AO+448+272+496+272+0%0A172+16+176+-64+176+0+7+9.36+12+-12+0+0.5+Voltage%0Ao+12+64+0+4098+5+0.1+0+1%0A) created by [Thea Flowers (Stargirl)](https://twitter.com/theavalkyrie) of [Winterbloom](https://winterbloom.com/)
