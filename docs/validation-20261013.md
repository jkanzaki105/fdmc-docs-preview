---
layout: default
title: Integration validation — 13 October 2026
---

# Integration validation — 13 October 2026

This record describes checks performed on the integrated local Fortran checkout
and its separately exported public source candidate. The documentation repository
does not contain the test executables or private source. These are implementation
and regression results, not an independent physics validation or software release.

## Recovery and integration

The earlier MG5 update archive contained 168 files. Before integration, 154 were
missing from the local checkout, 13 matched the original baseline, and the README
had diverged. After integration, 161 files matched the update exactly and seven
had intentional integration changes (documentation, shared-driver build wiring
and test improvements); none remained missing or at the original baseline.

The previous working tree was backed up locally before changes. The local
integration report, source hashes, archive manifest, comparison results and test
logs provide a reproducible record. Existing BASES and PCG implementation files
were checked against that backup and preserved.

## Build and regression checks

Release builds used GNU Fortran 16.2 and AppleClang 17.0. Cuba runtime checks used
a separate Cuba installation configured without shared-memory workers; the system
installation was not changed.

| Software configuration | CTest result |
| --- | --- |
| Private, BASES and Cuba | 25 / 25 passed |
| Private, BASES only | 19 / 19 passed |
| Public export, Cuba only | 24 / 24 passed |
| Public export, neither backend | 18 / 18 passed |

The public export rejects enabling BASES. Installation and license-file inclusion
were checked. Publication-guard tests included rejection of prohibited material
in earlier commits reachable from a proposed push, rather than only the current
working tree.

## μ− μ+ → νμ anti-νμ t tbar h

The MG5 3.8.1 import has 89 diagrams, one colour amplitude, 64 helicity
combinations and 11 integration dimensions.

- 4,272 physical phase-space points were checked.
- 3,600 legacy phase-space comparisons and 17,088 MG5 helicity comparisons
  were performed.
- The sampled MATRIX relative difference was zero; maximum momentum residual
  divided by collision energy was 3.87 × 10⁻¹⁶, and maximum weight relative
  difference was 1.04 × 10⁻¹⁵.
- The continuum phase-space volume check passed.
- 113 integration tasks with 71,500 evaluations returned finite, nonnegative
  estimates. Six channels at 100 TeV (graphs 14, 35, 39, 54, 55 and 56) failed
  their requested precision, as in the earlier test.

Public Cuba driver execution, a four-graph managed run and a two-energy scan
(eight tasks, two external workers) completed and finalized. These used deliberately
loose accuracy settings and short runs to verify operation. BASES-only driver
execution also completed. Such smoke runs do not establish convergence.

## Remaining physics checks

High-statistics cross-section comparison against the paper or another established
calculation remains pending. Record actual model and parameter cards, energies,
cuts, configurations, mappings, seeds, compiler and backend settings when making
that comparison. The new runnable example does not supply all SMEFT applications
or extreme-forward HELAS benchmarks discussed in the research papers.

See [Publication status](publication.html), [Examples](examples.html) and
[Physics and validation](physics.html) for the surrounding scope.
