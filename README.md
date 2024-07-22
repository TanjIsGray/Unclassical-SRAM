# Unclassical-SRAM
LTSpice worksheet demonstrating operation of the SRAM circuit from https://tanjb.substack.com/p/doing-more-with-the-minimum on the Poratbo blog.

Pass transistors and transmission gates are discussed here: https://my.eng.utah.edu/~cs6710/slides/Harris-circuitsx2.pdf

The classic SRAM circuit uses 2 weak-pass transistors per read-write port, one on each side of the internal feedback pair.  When a new signal is applied to both sides they can overpower the internal feedback and set a new value.  This approach was fine in planar logic designs but is wasteful for FinFET or ribbon-FET (aka Gate All Around) on layout because each port adds a pair of the same kind, while CMOS is optimized for complementary pairs.  With the traditional approach a single-port SRAM uses 6 transistors but in fins or ribbons it leaves 2 unused spaces, for a 75% efficiency on use of space.  A more useful dual-port SRAM uses 8 transistors in a 12-transistor space, for a 67% layout efficiency.

I searched for ways to eliminate the wasted space and discovered a simple approach - use a transmission gate (a CMOS pair of pass gates, with complementary enable signals).  This does not work for a single port design - you still lose 2 transistor spaces to get isolation from a neighbor - but it does allow 8 transistors in an 8 transistor space to deliver a dual-port SRAM with 100% layout efficiency.  In this case one transmission gate is on one side of the feeback pair, which the other transmission gate is on the other side.  When a transmission gate is enabled it has 2 active transistors which allow it to force a new value.  When not enabled the transmission gate provides isolation from the neighbor.

As an additional bonus, this design is laid out with the same Vdd/Vss rail system as logic cells use, so there is in principle no extra processing step needed or boundary zone for rules to adjust.  It may be desirable for the SRAM to use fewer ribbons than the logic cells bit it should work with the same transistors as used for logic.

This project is a simple analog model to demonstrate that the transmission gates do in fact allow a cell to be written.  The TSMC018.lib is referenced to obtain some reasonably recent transistors.  That library is the best process library for LTspice that you can find on the web.  It was written at TheMOSISservice to support shuttle projects.  I have adjusted the width settings on the PMOS and NMOS models to make the PMOS twice as wide in order to get roughly equal P- and N- drive strength which is the norm with FinFET and RibbonFET, so that even with the different sizes the principles of the circuit are reasonably similar.  If you have access to a more modern library you may swap the devices and run it with newer simulations.

A very simple signal sequence is set up with some voltage generators.  If you run the simulation and monitor the output labels you can see the inner value does indeed switch (in about 1ns) when the transmission gate is opened to a new value.

Other advantages and notes on physical construction can be found on the article linked above, on the Poratbo blog.

If you do run alternative devices through simulation, please leave a comment on what you observed.  I will be open to uploading alternative versions, with your authorship added, if you agree to the Apache 2 license on this project.
