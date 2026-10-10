---
layout: default
title: FDMC Version 0
---

# FDMC — documentation preview

FDMC (**Feynman-Diagram Gauge Monte Carlo**) is a Fortran framework for
Monte Carlo calculations based on helicity amplitudes in the
**Feynman-diagram (FD) gauge**. By avoiding large artificial gauge cancellations
among diagrams, the FD gauge makes individual diagram amplitudes effective
guides for diagram-dependent phase-space channels and importance sampling,
used together with numerical integration backends.

For the introduction of this approach to electroweak helicity amplitudes, see
[Chen, Hagiwara, Kanzaki and Mawatari, “Helicity amplitudes without gauge
cancellation for electroweak processes”](references.html#fd-gauge-ew).
Physical interference is retained in the full amplitude.

This documentation is a review preview for collaborators. FDMC is a provisional
project name; the name, page structure and wording are open for discussion.
The software source remains in a separate private repository.

## Scientific basis

This project prepares the techniques used in the
[“Multichannel phase space (MCPS) with Feynman-diagram-gauge amplitudes”
by Hagiwara et al.](references.html#mcps),
*Phys. Rev. D* **114**, 036002 (2026), for software distribution.
The paper is the primary reference for the method and physics applications.

## Motivation and approach

Multi-particle processes at multi-TeV lepton colliders can be difficult to
integrate accurately because of complicated resonance structures, strongly
forward-peaked distributions and large gauge cancellations among individual
Feynman diagrams.

The starting point is **single-diagram-enhanced (SDE) multi-channel integration**,
introduced by Fabio Maltoni and Tim Stelzer in Section 2 of
[“MadEvent: Automatic event generation with MadGraph”](references.html#madevent).
It splits the full integrand into positive diagram-guided contributions that can
be integrated separately, with sampling effort adapted to each channel.
This idea underpins MadEvent and subsequent MadGraph event-generation frameworks.

The MCPS paper brings this strategy together with the **Feynman-diagram (FD) gauge**.
Large gauge cancellations can make individual diagram peaks poor sampling guides,
particularly at high energies. FD-gauge amplitudes avoid those cancellations,
allowing the original SDE idea to work effectively in these demanding regimes.
Physical interference is retained in the full integrand.
See [the method and its gauge dependence](physics.html#single-diagram-enhanced-integration-and-the-fd-gauge).

The broader research programme addresses associated top-quark pair and Higgs
production, including vector-boson-fusion and related channels, and the reliable
treatment of extremely forward charged leptons. See
[Method and physics applications](physics.html#method-and-physics-applications) for the
applications studied in the MCPS paper.

## Version 0 documentation

Start with [Getting started](getting-started.html), then use the
[examples](examples.html) to see the working process and amplitude-only checks.

Version 0 keeps a focused implementation: Fortran, an MG5 adapter, shared
amplitude/kinematics libraries, YAML-driven phase space, an external
Cuba/Vegas backend. Higher-multiplicity catalogue generation is available, while every
new process still needs reviewed physics mappings.

`emep_emepz` is the integration example. `gg_ggg` is an amplitude example with
an unfinished integration mapping. Neither compilation nor a smoke calculation
alone establishes a validated physical prediction.

The source layout and extension boundaries are described in
[Development](development.html). Preview scope and review points are described in
[About this preview](publication.html).
