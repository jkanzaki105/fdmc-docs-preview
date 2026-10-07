---
layout: default
title: FDMC Version 0
---

# FDMC — documentation preview

FDMC provides Fortran Monte Carlo calculations built around MG5-generated
helicity amplitudes, diagram-dependent phase-space channels and integration
backends. This documentation is a review preview for collaborators. FDMC is a provisional
project name; the name, page structure and wording are open for discussion.
The software source remains in a separate private repository.

## Motivation and approach

Multi-particle processes at multi-TeV lepton colliders can be difficult to
integrate accurately because of complicated resonance structures, strongly
forward-peaked distributions and large gauge cancellations among individual
Feynman diagrams.

Our approach uses multi-channel phase-space integration guided by individual
Feynman diagrams evaluated in the Feynman-diagram (FD) gauge. In this gauge,
individual amplitudes retain a close correspondence with the associated physical
subprocesses. Their squared amplitudes guide importance sampling in the relevant
regions of phase space, while the physical prediction includes the required
interference between diagrams.

The broader research programme addresses associated top-quark pair and Higgs
production, including vector-boson-fusion and related channels, and the reliable
treatment of extremely forward charged leptons. See
[Research context](physics.html#research-context-lcws2026-abstract) for the
scope described in the LCWS2026 abstract.

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
