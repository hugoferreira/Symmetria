# Symmetria

A computer algebra system for the NumWorks N0100, as firmware. It takes the
place of the calculator's software: a command line in the manner of the TI-92,
exact and symbolic results typeset on the screen, and help for every function
on the calculator itself.

![A session: an integral, an equation, a Taylor series, a factorisation](screenshots/calculus.png)

| | |
|---|---|
| Version | 0.1 alpha, 12 October 2026 |
| Calculator | NumWorks N0100 only (the model number is on the back) |
| Download | [Releases](https://github.com/hugoferreira/Symmetria/releases): `symmetria-n0100-0.1-alpha.bin`, 836,528 bytes |
| SHA-256 | `aa61fff9e7a4a9dd95449b25dfc019fa966307ef5e8c0f1764b00d0d048959ac` |
| Source | not published yet: see [Source code](#source-code) |

Symmetria has no graphs, no Python, no examination mode and none of Epsilon's
applications. Read [Before you install](#before-you-install).

## What it does

One expression per line; the answer is exact unless a decimal is asked for.

```
2/3 + 5/6                                3/2
expand((x + 1)^3)                        x^3+3x^2+3x+1
factor(x^4 - 1)                          (x-1)*(x+1)*(x^2+1)
solve(x^2 - 5x + 6 = 0, x)               {2, 3}
solve([x + y = 10, x - y = 4], [x, y])   [[7, 3]]
partFrac(1/(x^2 - 1), x)                 1/(2*(x-1))-1/(2*(x+1))
d(sin(x)*exp(x), x)                      sin(x)*e^x+cos(x)*e^x
integrate(x^2*exp(x), x)                 e^x*(x^2-2x+2)
integrate(1/(1+x^2), x, 0, inf)          pi/2
taylor(sin(x), x, 0, 5)                  x^5/120-x^3/6+x
limit(sin(x)/x, x, 0)                    1
sum(k^2, k, 1, n)                        n*(n+1)*(2n+1)/6
deSolve(d(y, x) = y, y, x)               c*e^x
laplace(sin(t), t, s)                    1/(s^2+1)
inverse([[1,2],[3,4]])                   [[-2, 1], [3/2, -1/2]]
eigVl([[2,1],[1,2]])                     [1, 3]
cSolve(x^2 + 1 = 0, x)                   {-i, i}
sin(pi/3)                                sqrt(3)/2
abs(x) | x > 0                           x
solve(x^2 - 4 = 0, x in N)               {2}
primeFactors(60)                         [[2, 2], [3, 1], [5, 1]]
nCr(33,11)*nCr(22,11)/6                  22754499243840
[1..5] <~> (x -> x^2)                    [1, 4, 9, 16, 25]
#{x | 0 < x < 30, x % 3 = 0}             9
nInt(exp(-x^2), x, 0, inf)               0.8862269255
normCdf(1.96)                            0.9750021049
0.1 + 0.2                                0.3
```

The results are given here as text; the calculator typesets them, as in the
screens below.

About 390 functions, each with a help page on the calculator (391 pages, 645
worked examples):

- **Algebra**: `expand`, `factor`, `solve`, `cSolve`, `simult`, `partFrac`,
  `together`, `collect`, `polyGcd`, `resultant`, `groebner`, `eliminate`.
- **Calculus**: `d`, `integrate`, `limit`, `taylor`, `series`, `deSolve`,
  `rSolve`, `laplace`, `fourier`, `zTransform` and their inverses,
  `gradient`, `jacobian`, `hessian`; numeric `nInt`, `nSolve`, `nDeriv`.
- **Linear algebra**: `det`, `inverse`, `rref`, `lu`, `qr`, `cholesky`,
  `eigVl`, `eigVc`, `jordan`, `charPoly`, `nullSpace`, `matExp`.
- **Number theory**: primes, factorisation, `gcd`, modular inverse and powers,
  the Chinese remainder theorem, continued fractions, Stirling and Bell
  numbers.
- **Statistics**: descriptive statistics, regressions, twelve families of
  distributions, hypothesis tests and confidence intervals.
- **Assumptions**: `x^2 | x > 0`, `solve(..., x in Z)`, `given(x > 0)` for the
  session; a default domain for all variables in the settings.
- **Constraints over the integers**: `satisfy`, `allSolutions`, `nSolutions`,
  `minimize`, `maximize`, with `allDifferent`, `circuit`, `table` and other
  global constraints; set-builder notation `{x | ...}`.
- **Collections and functions**: lists, sets, ranges, lambdas
  (`x -> x^2`), map, filter and fold as operators (`<~>`, `<?>`, `<>`),
  definitions (`f(x) := ...`).
- **Graphs** as adjacency matrices: shortest paths, spanning trees, maximum
  flow, components.

| | |
|---|---|
| ![Matrices and a limit](screenshots/linear.png) | ![Constraints and a set-builder](screenshots/constraints.png) |
| ![A help page](screenshots/help.png) | ![The Toolbox](screenshots/toolbox.png) |

The screens are drawn by the firmware's own code, run on a computer, at twice
their size. They are not photographs of the panel.

## Before you install

- **N0100 only.** The image is built for the N0100's processor and board. Do
  not write it to an N0110, N0115 or N0120.
- **It replaces Epsilon.** Nothing of NumWorks' software stays, and what the
  calculator holds (scripts, settings) is lost.
- **No examination mode.** A calculator running Symmetria must not be taken to
  an examination that requires one.
- **It is an alpha.** Expect wrong answers, missing methods and rough edges.
  Do not rely on it where a mistake costs something.
- **It can be undone.** The N0100 enters its update mode by hardware (key `6`
  with Reset), whatever is installed, so other firmware can always be written
  over it: see [Going back](#going-back).

## Install

1. Download `symmetria-n0100-0.1-alpha.bin` from
   [Releases](https://github.com/hugoferreira/Symmetria/releases) and compare
   its SHA-256 with the one above (`shasum -a 256` or `sha256sum`).
2. Connect the calculator by USB. Hold `6`, press Reset on the back (a pen
   tip or a paper clip), release. The screen stays dark: the calculator is in
   DFU mode and the computer sees an "STM32 BOOTLOADER" device.
3. Write the image to the internal flash, at address `0x08000000`:

   ```
   dfu-util -d 0483:df11 -a 0 -s 0x08000000:leave -D symmetria-n0100-0.1-alpha.bin
   ```

   Without `dfu-util`: a WebUSB page such as
   [TI-Planet's for the N0100](https://ti-planet.github.io/webdfu_numworks/n0100/)
   (Chromium browsers) writes a `.bin` to the same address.
4. If the calculator does not start by itself, press Reset.

If the transfer stops with an error, put the calculator in DFU mode again and
repeat; another USB port has helped.

## Going back

Enter DFU mode as above and install other firmware. NumWorks'
[recovery procedure](https://www.numworks.com/support/handheld/rescue/)
reinstalls Epsilon; it asks for a NumWorks account.

## Using it

- Type a line, `EXE` evaluates it. Shift+`EXE` gives the decimal value.
- The printed legends keep their meaning: letters with Alpha, brackets and
  the inverse functions with Shift. While a name is typed a list of
  completions opens: `↑` `↓` choose, `EXE` takes one.
- **Toolbox**: the functions by category. **var**: your definitions, the
  assumptions in force, the last answers.
- **Help**: type `help`, or `help solve`; Shift+Back on a name opens its page.
  On a page, `↑` `↓` mark an example and `EXE` puts it into the line.
- `↑` from the line goes through the history: `EXE` brings an entry's input
  back, `→` inserts its result.
- **Home**: the settings (angle unit, what a variable is when nothing is
  stated about it, fractions or decimals, digits), the version, a diagnostics
  screen.
- Holding Back stops a long evaluation. The power key suspends the
  calculator and wakes it.
- `help syntax` lists the notation: `a % b`, `[1..5]`, `{x | ...}`, `#set`,
  `x -> body`, `f(x) := body`, `expr | condition`.

![The settings](screenshots/settings.png)

## What to expect of an alpha

- Settings, definitions, assumptions and the history are kept in memory only.
  They survive the power key; they are lost when the Reset key is pressed or
  the battery runs out.
- A line the evaluator has no method for comes back as entered, sometimes
  with a note in the status bar (`no method applies; try nSolve`).
- An evaluation has 160 KB of working memory; one that needs more ends with
  `out of memory`. `expand` stops at 255 terms. A line holds 255 characters.
- Decimals are computed in double precision and shown with up to ten digits.
- No link to a computer: USB is for power and for installing.
- Not yet checked on a calculator: the battery reading against a meter, and
  stopping an evaluation with Back (it is tested on a computer only).

## How it is tested

The evaluator and the screens are the firmware's own code compiled for a
computer and run by a test suite: 6,132 checks pass for this release, among
them 4,174 on the results of expressions, 1,117 on screens and keys, and every
example of every help page. A build of that day's sources with a serial port
added was run on an N0100 on 12 October 2026, and its results and screens
compared with the computer's. The tests cover what was thought of; a computer
algebra system can be wrong in ways that were not.

## How it was made

Symmetria is the work of one person. It is not a fork or a port of an
existing firmware or computer algebra system, and it contains no code of
NumWorks' Epsilon, of Omega or of Upsilon. The evaluator, the solvers, the
typesetting, the interface and the hardware drivers were written for it, for
the bare processor, without a vendor library.

What came from elsewhere is listed in
[THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md): the fonts (derived from
Terminus Font), the decompressor of the help text (adapted from
zlib-deflate-nostdlib) and the mathematical routines of the compiler's
library.

What the N0100's board needs was learned from published material: the
documentation of the STM32F412 and of the display controller, and, for the
board itself, the N0100 drivers of the Omega firmware. Pin assignments, the
battery's voltage thresholds, the timing of the key matrix, the sequence that
puts the processor to sleep and the behaviour of Shift and Alpha follow what
those drivers do. They were read as a description of the hardware; the
drivers in Symmetria are separate code.

### Use of AI

Most of Symmetria's code, tests and help text were written with AI coding
assistants (Anthropic's Claude models, through Claude Code), to the author's
design and decisions. This document was drafted the same way. What stands
behind a result is what the tests and the measurements cover, as described
above, and no more than that.

## Source code

This release is the binary only. The source code is being prepared for
publication and will appear in this repository; its licence will be stated
then. Until it is there, the image cannot be rebuilt or modified by others,
and questions about how something is computed are welcome as issues.

## Reporting a problem

Open an [issue](https://github.com/hugoferreira/Symmetria/issues) with the
line as typed, what the calculator answered and what was expected. The
version is on the About screen (Home, row 5).

## Licence

The image may be installed on N0100 calculators and passed on unmodified, free
of charge: [LICENSE.md](LICENSE.md). It is provided as it is, without warranty
of any kind; installing it is at your own risk.

NumWorks is a trademark of its owner. Symmetria is an independent project,
not affiliated with, sponsored by or endorsed by NumWorks.
