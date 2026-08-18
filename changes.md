### Change made

`sky130_ef_io.lef`, `MACRO sky130_ef_io__gpiov2_pad_wrapped`:

1. Added two met5 `PORT` blocks to `PIN VSSIO`, geometrically identical to the
   existing met4 ports (the met4 and met5 plates have exactly the same extent
   here, unlike the power rails lower down where met5 is inset by 0.100 um in
   y, so no inset is applied):

   ```
       PORT
         LAYER met5 ;
           RECT 0.000 186.750 1.270 210.965 ;
       END
       PORT
         LAYER met5 ;
           RECT 78.730 186.750 80.000 210.965 ;
       END
   ```

2. Carved the new ports out of the met5 obstruction, so that the obstruction no
   longer covers the macro's own pin (a pin fully inside a same-layer `OBS` is
   treated as unusable by most routers):

   ```
   -      LAYER met5 ;
   -        RECT 0.000 179.575 80.000 210.965 ;
   +      LAYER met5 ;
   +        RECT 0.000 179.575 80.000 186.750 ;
   +        RECT 1.270 186.750 78.730 210.965 ;
   ```

   The carve is *tight* — the obstruction abuts the pin edges at `x` 1.270 and
   78.730 — so no met5 in the strap is left both exposed and unblocked. (The
   met4 obstruction here keeps a 0.400 um back-off from its pin, and the met5
   obstructions over the power rails keep 1.600 um; matching either of those
   would have left a strip of bare VSSIO met5 that a router could legally
   short to.)

No other `MACRO` in the file was touched. `PIN VSSIO`'s `DIRECTION INOUT` /
`USE GROUND` and every other pin, port and obstruction are unchanged.
