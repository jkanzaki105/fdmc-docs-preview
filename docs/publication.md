---
layout: default
title: Publication status and preview scope
---

# Publication status and preview scope

**Updated 13 October 2026.** This repository contains documentation and site
assets only. The Fortran integration work has been applied to the local software
checkout. A separate public source snapshot has been prepared and checked locally,
but has not yet been committed or pushed as a software release.

FDMC is a provisional project name. This preview supports review of the
explanations, examples and presentation, and documents techniques used in the
published [MCPS paper](references.html#mcps).

## Integrated software status

- PCG now lives in `common/random`, independently of BASES, with C and Fortran
  interfaces. The C header supports C++ callers; a CUDA implementation remains
  future work. This refactoring does not change Cuba's own random-number generator.
- The previously tested MG5 3.8.1 import
  `mummup_vmvmxttxh` (μ− μ+ → νμ anti-νμ t tbar h) is integrated locally,
  alongside `emep_emepz` and the amplitude example `gg_ggg`.
- YAML phase-space generation, MG5 split amplitude routines, shared drivers and
  run management are included in the integrated checkout.
- Builds and numerical replay checks passed in the configurations described in
  [the integration validation record](validation-20261013.html). High-statistics
  cross-section comparison remains pending; six high-energy channel precision
  failures are explicitly retained in that record.

## Public and private source management

The private local checkout retains BASES. The public source export excludes
BASES and starts a new Git history, so the private development history is not
transferred. Public builds can use separately installed Cuba; private builds can
use BASES, Cuba, or both.

The local publication tools provide two safeguards:

1. A private-checkout pre-push hook rejects pushes from that checkout.
2. A public-export pre-push hook checks every commit reachable from the pushed
   references for prohibited paths, known private-source fingerprints, BASES
   declarations, archives and other excluded material.

The export uses an explicit source selection and refuses an existing destination.
These hooks are local safeguards and can be bypassed or removed; they do not
replace review of the exported files and history. Publish only from the reviewed
public checkout, with its hook installed. This documentation update publishes
neither BASES nor software source.

## Review points

- Project name and introductory description.
- Navigation and clarity of MG5 and phase-space explanations.
- Separation of implementation checks from independent physics validation.
- Process selection for a future software release.
- Related references alongside the primary MCPS paper.

## Hosting and license

GitHub Pages uses **main** and **/docs**, with project path
`/fdmc-docs-preview`. Documentation and site assets use BSD-3-Clause; see
the repository LICENSE. MG5, PCG and Cuba retain their own software licenses
and notices in the software distribution. Descriptions here do not bundle them.
