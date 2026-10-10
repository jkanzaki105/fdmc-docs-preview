---
layout: default
title: Physics and validation
---

# Physics and validation

The method and physics applications described on this page are based on the
[MCPS paper by Hagiwara et al.](references.html#mcps),
*Phys. Rev. D* **114**, 036002 (2026). This project prepares the techniques
used in that paper for software distribution.

<span id="research-context-lcws2026-abstract"></span>

## Method and physics applications

### Single-diagram-enhanced integration and the FD gauge

Section 2 of [Maltoni and Stelzer's MadEvent paper](references.html#madevent)
introduces **single-diagram-enhanced (SDE) multi-channel integration**.
For diagram amplitudes Aᵢ and total amplitude A = Σᵢ Aᵢ, define

```text
wᵢ = |Aᵢ|² / Σⱼ |Aⱼ|²
fᵢ = wᵢ |A|² = |Aᵢ|² R
R  = |Σⱼ Aⱼ|² / Σⱼ |Aⱼ|²
Σᵢ fᵢ = |A|²
```

These positive contributions are integrated separately with diagram-specific
mappings. Each channel can receive sampling effort appropriate to its convergence.
The decomposition preserves interference; it is not an incoherent approximation.
This avoids the coupled sampling-density weights of conventional multi-channel
optimization. Statistical independence of sampled integrations does not mean
absence of physical interference.

The [MCPS paper](references.html#mcps) explains why gauge choice matters:
large cancellations in conventional gauges can spoil diagram-based sampling,
especially in enhanced high-energy regions. The MadGraph conventions discussed
there use Feynman gauge for photons/gluons and unitary gauge for weak bosons.
The [electroweak FD-gauge construction of Chen et al.](references.html#fd-gauge-ew)
provides amplitudes without artificial gauge cancellation.
FD gauge avoids the large gauge cancellations and keeps R of order unity in
the configurations studied. Together with suitable singularity mappings, this
lets the original SDE method realize its intended efficiency. Physical
interference remains included.

Fabio Maltoni, coauthor of the original MadEvent paper, is also a coauthor of
the MCPS paper. The approach builds directly on his and Tim Stelzer's integration
method.

### Top–Higgs production and a complex top-Yukawa coupling

The applications studied in the paper involve top-quark pair production
in association with a Higgs boson at lepton colliders, including
vector-boson-fusion and related channels, within the Standard Model effective
field theory (SMEFT) with a complex top-Yukawa coupling.

Some of these processes contain lepton-mass singularities arising from
*t*-channel photon exchange. Accurate integration therefore requires a dedicated
phase-space parametrization for extremely forward charged leptons.

### Reliable amplitudes in the extreme forward region

The paper describes modifications to HELAS (HELicity Amplitude Subroutines)
that allow helicity amplitudes to be evaluated reliably even when the magnitude
of the squared momentum transfer is as small as the squared electron mass at
multi-TeV collision energies. These amplitude improvements and the forward-region
parametrization are complementary parts of the integration method.

The paper reports stable and efficient integration
for these challenging processes, providing a step toward reliable event
generation for future high-energy lepton-collider studies.

### Relationship to this documentation preview

The Version 0 examples presented here are `emep_emepz` (an integration example)
and `gg_ggg` (amplitude checks with an unfinished integration mapping).
The top–Higgs/SMEFT applications and extreme-forward HELAS results described
above are research context, not additional runnable examples or validation
benchmarks supplied by this preview. The choice of distributed process examples will be reviewed with collaborators;
the present examples are unchanged pending that discussion.

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

See [References](references.html) for the primary paper and its BibTeX record.
