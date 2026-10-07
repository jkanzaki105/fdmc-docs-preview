---
layout: default
title: Physics and validation
---

# Physics and validation

Version 0 distinguishes implementation checks from physical validation.

| Evidence | Establishes | Does not establish |
| --- | --- | --- |
| Clean CMake build | Compiler/link dependency consistency | Physical correctness |
| YAML generator/converter tests | Data checks and expected conversion behaviour | Completeness of the topology catalogue |
| MG5 amplitude consistency | Reconstructed helicity/colour contraction at sampled points | Full phase-space integration |
| Fixed-seed source/candidate replay | Numerical preservation across repository preparation | Agreement with an independent physical calculation |
| Short integration | Executable and backend operation | Convergence, precision or final physics results |

For publication, record process definition, MG5/model provenance, parameter
card, energy, cuts, amplitude configuration, mapping/catalogue versions,
integrator settings, RNG/seed, compiler and result with statistical uncertainty.
Compare the total with an independent or established reference and check
convergence over increasing statistics. Preserve the actual cards used.

The matrix already carries out the fixed-helicity colour contraction. Do not
add another colour-flow sum. Process diagrams and configuration tables control
coherent/incoherent reconstruction and channel weights; changes require
physics-level review.

No paper citation, release DOI or bibliography is invented by this preparation.
The project owner should supply the intended FD-gauge/FDMC references and
citation metadata before tagging the release.
