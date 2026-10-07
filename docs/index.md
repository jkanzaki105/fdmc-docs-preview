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
[Publication preparation](publication.html).
