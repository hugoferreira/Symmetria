# Symmetria

A computer algebra system for the NumWorks N0100, as firmware. It takes the
place of the calculator's software: a command line in the manner of the TI-92,
exact and symbolic results typeset on the screen, graphs as answers, and help
for every function on the calculator itself.

![A session: an integral, an equation, a Taylor series, a factorisation](screenshots/calculus.png)

| | |
|---|---|
| Version | 0.4 alpha, 26 October 2026 ([what is new](#new-in-04)) |
| Calculator | NumWorks N0100 only (the model number is on the back) |
| Download | [Releases](https://github.com/hugoferreira/Symmetria/releases): `symmetria-n0100-0.4-alpha.bin`, 979,940 bytes |
| SHA-256 | `facc9aa0930bd6ecf9f21c9a0543c1bd87e9062b10a934907275910d04f51d89` |
| Source | not published yet: see [Source code](#source-code) |

Symmetria has no Python, no examination mode and none of Epsilon's
applications; a graph is the answer of a line, not an application. Read
[Before you install](#before-you-install).

## What it does

One expression per line; the answer is exact unless a decimal is asked for.

```
2/3 + 5/6                                3/2
expand((x + 1)^3)                        x^3+3x^2+3x+1
factor(x^4 - 1)                          (x-1)*(x+1)*(x^2+1)
factor(x^12 - 1)                         (x-1)*(x+1)*(x^2+1)*(x^2+x+1)*(x^2-x+1)*(x^4-x^2+1)
solve(x^2 - 5x + 6 = 0, x)               {2, 3}
solve([x + y = 10, x - y = 4], [x, y])   [[7, 3]]
partFrac(1/(x^2 - 1), x)                 1/(2*(x-1))-1/(2*(x+1))
d(sin(x)*exp(x), x)                      sin(x)*e^x+cos(x)*e^x
d(sqrt(x), x)                            1/(2sqrt(x))
integrate(x^2*exp(x), x)                 e^x*(x^2-2x+2)
integrate(1/(1+x^2), x, 0, inf)          pi/2
taylor(sin(x), x, 0, 5)                  x^5/120-x^3/6+x
limit(sin(x)/x, x, 0)                    1
sum(k^2, k, 1, n)                        n*(n+1)*(2n+1)/6
sum(1/k^2, k, 1, 20)                     17299975731542641/10838475198270720
deSolve(d(y, x) = y, y, x)               c*e^x
laplace(sin(t), t, s)                    1/(s^2+1)
inverse([[1,2],[3,4]])                   [[-2, 1], [3/2, -1/2]]
eigVl([[2,1],[1,2]])                     [1, 3]
charPoly([[1,2],[3,4]])                  lambda^2-5lambda-2
cSolve(x^2 + 1 = 0, x)                   {-i, i}
sin(pi/3)                                sqrt(3)/2
abs(x) | x > 0                           x
solve(x^2 - 4 = 0, x in N)               {2}
primeFactors(60)                         [[2, 2], [3, 1], [5, 1]]
nCr(33,11)*nCr(22,11)/6                  22754499243840
30!                                      265252859812191058636308480000000
[1..100000] /> (+)                       5000050000
[1..5] :> (x -> x^2)                     [1, 4, 9, 16, 25]
#{x | 0 < x < 30, x % 3 = 0}             9
nInt(exp(-x^2), x, 0, inf)               0.8862269255
normCdf(1.96)                            0.9750021049
0.1 + 0.2                                0.3
approx(2^2000/3^1200)                    3.269532543*10^29
plot(x^3 - 3x, x, -3, 3)                 the graph, in the window -20 to 20
```

The results are given here as text; the calculator typesets them, as in the
screens below.

About 400 functions, each with a help page on the calculator (406 pages, 895
worked examples, which a page evaluates when it is opened):

- **Algebra**: `expand`, `factor` (also in several variables), `solve` (also
  inequalities, congruences and polynomial systems), `cSolve`, `simult`, `partFrac`,
  `together`, `collect`, `polyGcd`, `resultant`, `groebner`, `eliminate`;
  numeric `proot`; the orthogonal polynomials of Legendre, Hermite, Laguerre
  and Chebyshev.
- **Calculus**: `d`, `integrate`, `limit`, `taylor`, `series`, `deSolve`,
  `rSolve`, `laplace`, `fourier`, `zTransform` and their inverses,
  `gradient`, `jacobian`, `hessian`; numeric `nInt`, `nSolve`, `nDeriv`.
- **Graphs of functions**: `plot(f, x)` or `plot(f, x, a, b)`; the answer is
  the graph, drawn in the history, and the call with its window.
- **Numbers**: integers to about 2,450 digits and fractions of them, exact;
  factorials, binomials, Fibonacci, Bell and Stirling numbers of that size.
- **Linear algebra**: `det`, `inverse`, `rref`, `lu`, `qr`, `cholesky`,
  `eigVl`, `eigVc`, `jordan`, `charPoly`, `nullSpace`, `matExp`, `matpow`,
  `matrix`; numeric `svd`.
- **Number theory**: primes, factorisation, `gcd`, modular inverse and powers,
  the Chinese remainder theorem, continued fractions, Stirling and Bell
  numbers.
- **Statistics**: descriptive statistics, regressions, twelve families of
  distributions, hypothesis tests and confidence intervals.
- **Assumptions**: `x^2 | x > 0`, `solve(..., x in Z)`, `given(x > 0)` for the
  session; a default domain for all variables in the settings.
- **Constraints over the integers**: `satisfy`, `allSolutions`, `nSolutions`,
  `minimize`, `maximize`, with `distinct`, `ordered`, `allowed`, `singleCycle`,
  `withinCapacity` and other predicates; set-builder notation `{x | ...}`.
- **Collections and functions**: lists, sets, ranges, lambdas
  (`x -> x^2`), map, filter, fold and scan as operators (`:>`, `?>`, `/>`, `\>`),
  definitions (`f(x) := ...`).
- **Graphs** as adjacency matrices: shortest paths, spanning trees, maximum
  flow, components.

| | |
|---|---|
| ![Matrices and a limit](screenshots/linear.png) | ![Constraints and a set-builder](screenshots/constraints.png) |
| ![A help page with its examples evaluated](screenshots/help.png) | ![The Toolbox](screenshots/toolbox.png) |
| ![Long integers and an exact sum](screenshots/numbers.png) | ![Factorisations and a characteristic polynomial](screenshots/factor.png) |

The screens are drawn by the firmware's own code, run on a computer, at twice
their size. They are not photographs of the panel.

## New in 0.4

**Read this before upgrading.** The collection operators are `/>` (fold),
`\>` (scan), `:>` (map) and `?>` (filter), and the functions that take a
collection take it first: `map(list, f)`. The constraint functions have new
names: `distinct`, `same`, `ordered`, `allowed`, `occurrences`,
`singleCycle`, `withinCapacity`. Lines written for 0.3 with the old ones no
longer work; the [release notes](https://github.com/hugoferreira/Symmetria/releases/tag/v0.4)
list every change.

**Solving.** Inequalities in one unknown by a sign chart, congruences as
residue classes, periodic equations as families, polynomial systems by
elimination, linear systems with symbols, and numerical roots where no exact
method applies (a Settings row turns them off).

![Inequalities, congruences, a periodic family](screenshots/solve4.png)

**Algebra and calculus.** Factoring in several variables; radicals written in
one form (`1/(sqrt(2)+sqrt(3)+sqrt(5))`); complex logarithms and powers;
`deSolve` for linear, Bernoulli, Riccati and homogeneous equations and
constant coefficients of any order; closed forms for sums, products and
recurrences (`sum(1/k^2, k, 1, inf)` is `pi^2/6`); `fMin`, `fMax`, `avgRC`,
`cubicReg`, `quartReg`, `powerReg`, `jacobi`, `order`, `primitiveRoot`,
`discreteLog`, `erf`, `harmonic`.

| | |
|---|---|
| ![A factorisation in three variables, a radical rationalised, a complex logarithm](screenshots/algebra4.png) | ![A differential equation of order 2, the Basel sum](screenshots/calculus4.png) |

**Constraint problems.** Predicates with one meaning everywhere (they also
evaluate on known values), optimisation of linear objectives of any length,
groups and circuits of 255 variables, exact counts past 2^31, values of mixed
kinds. Logic and sets are drawn as signs.

![A count, a fold, an intersection of residue classes](screenshots/constraints4.png)

**Answers that were wrong.** 94 kinds of wrong answer that 0.3 gives are
fixed, among them `normCdf(-10)` (0.3: 0), `gamma(17/2)` (negative),
`nSolutions([x in [0..200], (x - 1)*(x - 100) = 0], [x])` (100) and
`solve(x^2*(x+3) <= 0, x)` (the root 0 missing).

**Smaller footprint, more code.** Over the 1,169 lines of the measurement
corpus that give the same result in both versions, the median stack of an
evaluation is 1,172 bytes (0.3: 1,460) and evaluations take 3.3 s (3.5 s),
estimated for the calculator's processor; the calculator took 0.989 of the
estimate at the median. The firmware is 980 KB of the 1 MB flash, 111 KB more
than 0.3.

What was new in 0.3 (calculus in degrees, nine new functions, the fault
screen) and in 0.2 (graphs, colours, help pages that compute) is in their
release notes: [0.3](https://github.com/hugoferreira/Symmetria/releases/tag/v0.3),
[0.2](https://github.com/hugoferreira/Symmetria/releases/tag/v0.2).

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

1. Download `symmetria-n0100-0.4-alpha.bin` from
   [Releases](https://github.com/hugoferreira/Symmetria/releases) and compare
   its SHA-256 with the one above (`shasum -a 256` or `sha256sum`).
2. Connect the calculator by USB. Hold `6`, press Reset on the back (a pen
   tip or a paper clip), release. The screen stays dark: the calculator is in
   DFU mode and the computer sees an "STM32 BOOTLOADER" device.
3. Write the image to the internal flash, at address `0x08000000`:

   ```
   dfu-util -d 0483:df11 -a 0 -s 0x08000000:leave -D symmetria-n0100-0.4-alpha.bin
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
  back, `→` inserts its result, `OK` opens the entry's line on a page.
- A long line scrolls in seven rows; `↑` `↓` move through its rows.
- **Home**: the settings (angle unit, what a variable is when nothing is
  stated about it, exact or numerical solving, complex results, fractions or
  decimals, digits, the height of a graph, light or dark colours, the
  accent), the version, a diagnostics screen, a factory reset.
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
- An evaluation has 178 KB of working memory; one that needs more ends with
  `out of memory`, and one nested too deeply with `out of memory (stack)`.
  `expand` stops at 255 terms. A line holds 2,047 characters. An integer has
  about 2,450 digits at most.
- A graph is of one function of one variable. The history holds 14 graphs.
- Decimals are computed in double precision and shown with up to ten digits.
- No link to a computer: USB is for power and for installing.
- Not yet checked on a calculator: the colours as the panel shows them, the
  battery reading against a meter, and stopping an evaluation with Back
  (see [How it is tested](#how-it-is-tested)).

## How it is tested

The evaluator and the screens are the firmware's own code compiled for a
computer and run by a test suite. For this release 15,697 checks on the
results of expressions and 1,722 on screens and keys pass, among them every
example of every help page. About 178,000 generated lines (115,002 in 0.3)
are judged by values computed independently, in Python with exact integers
and 30-digit arithmetic, and 17,572 of them, in 136 families, are replayed by
the suite. The integer and rational
arithmetic is checked against 128-bit references, the suite is run once more
under sanitizers, and for 1,799 lines the time, memory and stack each takes
on the firmware's own code in an emulator of the processor are compared with
a recorded baseline before a change is accepted.

The serial build of this release's sources was run on an N0100 on 26
October 2026, driven and read over USB: 1,814 lines in one session from
power-on, every line of the performance corpus and every help example,
returned the computer's answers, in 0.989 of their estimated time at the
median. Preparing it found a fault that only a computer showed: a host build
placed the working memory at an address the calculator's build never uses,
and one constraint line wrote two bytes past its scratch block there. It is
fixed; the calculator's image did not change.

The serial build of 0.3's sources was run on an N0100 on 17 and 18
October 2026, driven and read over USB: 1,303 lines from power-on, every line
of the performance corpus and every help example, returned the computer's
text but for the order of two conjugate roots, in 0.994 of their estimated
time at the median. A longer session found a freeze that no computer can
show (a stack that ran out left a stale pointer behind); it is fixed, and a
fault of the processor now ends in a screen that says what happened, checked
on the calculator.

A build of 0.2's sources with a serial port added was run on an
N0100 on 14 October 2026, driven and read over USB. 776 lines in one session
returned what the computer returns, in the time estimated for them (0.99 of
the estimate at the median of the 108 that take a millisecond or more). 29
screens were the computer's pixel for pixel but for the charging mark: graphs,
the dark screen's frame buffer, a help page, a line of 2,045 characters, the
page of a line, a history that evicted its entries thirty times, the
settings. Lines nested too deeply ended in `out of memory (stack)` and the
calculator went on.

That run found what no test on a computer had: after enough lines for the
history to drop its oldest entry, the calculator could stand still until
Reset (a table of numbers moved to an odd address, which its processor
refuses and a computer does not). It is fixed since 0.2, and 0.1 has it.

Not checked, because it needs eyes and hands: the colours as the panel shows
them, stopping an evaluation with Back, the battery reading against a meter.

The tests cover what was thought of and what the generators draw; a computer
algebra system can be wrong in ways that neither did.

## How it was made

Symmetria is the work of one person. It is not a fork or a port of an
existing firmware or computer algebra system, and it contains no code of
NumWorks' Epsilon, of Omega or of Upsilon. The evaluator, the solvers, the
typesetting, the interface and the hardware drivers were written for it, for
the bare processor, without a vendor library.

What came from elsewhere is listed in
[THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md): the fonts (derived from
Terminus Font, with four glyphs of the misc-fixed fonts) and the mathematical
routines of the compiler's library.

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
version is on the About screen (Home, row 4).

## Licence

The image may be installed on N0100 calculators and passed on unmodified, free
of charge: [LICENSE.md](LICENSE.md). It is provided as it is, without warranty
of any kind; installing it is at your own risk.

NumWorks is a trademark of its owner. Symmetria is an independent project,
not affiliated with, sponsored by or endorsed by NumWorks.
