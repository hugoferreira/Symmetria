# Third-party notices

The Symmetria firmware image contains the following work of others. Each
remains under its own licence, which the terms of [LICENSE.md](LICENSE.md) do
not restrict.

## Fonts: derived from Terminus Font

The two bitmap fonts in the image (8x14 and 6x10 pixels) are modified versions
of Terminus Font 4.49: the glyphs the firmware uses of the 8x14 face
(`ter-u14n`), and those of the 6x12 face (`ter-u12n`) cut to ten rows, with
the marks that reached above them moved down. The integral sign and the
multiplication dot are drawn for Symmetria and do not come from that font. The
modified fonts are not named Terminus Font.

- Copyright (C) 2020 Dimitar Toshkov Zhekov, with Reserved Font Name
  "Terminus Font"
- SIL Open Font License 1.1: [licenses/OFL-1.1.txt](licenses/OFL-1.1.txt)
- https://terminus-font.sourceforge.net/

The font data in the image stays under the SIL Open Font License.

## Decompressor of the help text: adapted from zlib-deflate-nostdlib

The routine that unpacks the built-in help is adapted from
zlib-deflate-nostdlib.

- Copyright 2021 Birte Kristina Friesel
- BSD 2-Clause License:
  [licenses/BSD-2-Clause-zlib-deflate-nostdlib.txt](licenses/BSD-2-Clause-zlib-deflate-nostdlib.txt)
- https://github.com/derf/zlib-deflate-nostdlib

## Mathematical functions: newlib

The image is linked with the mathematical library (`libm`) of newlib 4.6.0, as
shipped in the Arm GNU Toolchain 15.3.Rel1: the double-precision elementary
functions (`sin`, `cos`, `tan`, `asin`, `acos`, `atan`, `sinh`, `cosh`, `tanh`,
`exp`, `expm1`, `log`, `pow`, `sqrt`, `cbrt`, `floor`, `ceil` and their
helpers). No other part of newlib is in the image.

- The functions come from fdlibm:

  > Copyright (C) 1993 by Sun Microsystems, Inc. All rights reserved.
  >
  > Developed at SunPro, a Sun Microsystems, Inc. business.
  > Permission to use, copy, modify, and distribute this
  > software is freely granted, provided that this notice is preserved.

- Their error handling: Copyright (c) 2009-2022 Arm Ltd, BSD 3-Clause License.
- Parts added by Red Hat: Copyright (c) 1994-2009 Red Hat, Inc., BSD License.
- The complete notices of newlib:
  [licenses/COPYING.NEWLIB](licenses/COPYING.NEWLIB), sections (1), (7) and
  (35).
- https://sourceware.org/newlib/

## Compiler support routines: libgcc

The image is linked with routines of GCC's runtime library (64-bit division,
double-precision arithmetic in software, bit counts), from the Arm GNU
Toolchain 15.3.Rel1 (GCC 15.3.1).

- Copyright (C) Free Software Foundation, Inc.
- GNU General Public License version 3 or later, with the GCC Runtime Library
  Exception 3.1, which permits a program compiled with GCC and linked with
  these routines to be distributed under terms of its author's choice.
- https://www.gnu.org/licenses/gcc-exception-3.1.html

## Not in the image

The released image contains no USB stack, no code of NumWorks' Epsilon and no
code of the Omega or Upsilon firmwares.
