---
layout: default
title: Physics and validation
---

# Physics and validation

## Research context: LCWS2026 abstract

The following research context is adapted from the project team's LCWS2026
abstract. It describes the broader method and applications; the software examples
currently documented in Version 0 have a narrower scope.

### Diagram-guided multi-channel integration

Accurate phase-space integration at multi-TeV lepton colliders requires sampling
that resolves resonance structures and strongly forward-peaked distributions.
Large gauge cancellations can obscure the physical interpretation of individual
Feynman diagrams in conventional gauges. In the FD gauge, individual amplitudes
retain a close correspondence with their associated physical subprocesses.
The method uses their squared amplitudes as importance-sampling guides for
individual integration channels. The complete physical amplitude, including
interference, remains the basis of the observable being integrated.

### Top–Higgs production and a complex top-Yukawa coupling

The applications described in the abstract involve top-quark pair production
in association with a Higgs boson at lepton colliders, including
vector-boson-fusion and related channels, within the Standard Model effective
field theory (SMEFT) with a complex top-Yukawa coupling.

Some of these processes contain lepton-mass singularities arising from
*t*-channel photon exchange. Accurate integration therefore requires a dedicated
phase-space parametrization for extremely forward charged leptons.

### Reliable amplitudes in the extreme forward region

The abstract describes modifications to HELAS (HELicity Amplitude Subroutines)
that allow helicity amplitudes to be evaluated reliably even when the magnitude
of the squared momentum transfer is as small as the squared electron mass at
multi-TeV collision energies. These amplitude improvements and the forward-region
parametrization are complementary parts of the integration method.

The research described in the abstract reports stable and efficient integration
for these challenging processes, providing a step toward reliable event
generation for future high-energy lepton-collider studies.

### Relationship to this documentation preview

The Version 0 examples presented here are `emep_emepz` (an integration example)
and `gg_ggg` (amplitude checks with an unfinished integration mapping).
The top–Higgs/SMEFT applications and extreme-forward HELAS results described
above are research context, not additional runnable examples or validation
benchmarks supplied by this preview. Process-specific results, benchmarks and
literature references should accompany their eventual software documentation.

## Validation of the Version 0 examples

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
