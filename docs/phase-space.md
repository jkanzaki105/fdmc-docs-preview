---
layout: default
title: Phase space and mappings
---

# Phase space and mappings

## Shared catalogues

`phase_space/topologies/topology_2toN.yml` contains reviewed generation recipes
for N = 2, 3, 4 and 5. CMake runs `tools/generate_phase_space.py` to create
`phase_space_2toN.f90` under the binary directory. The elementary routines live
in `phase_space/src/phase_space.f90` and depend on the kinematics library.

Catalogue generation and syntactic validation do not prove completeness or
physical correctness for every scattering process.

## Process data

`emep_emepz/mapping.yml` associates each diagram with a shared topology ID,
external-particle permutation and internal-particle serials.
`internals.yml` supplies internal identities and momentum assignments. Its
CMake rule also uses `src/ngraphs.inc` and produces the process mapping module
under `build/processes/emep_emepz/generated/`.

The root-level `emep_emepz/topology.yml` and compatibility Fortran copies are
retained as reference material. The active CMake inputs are the shared
catalogue, `mapping.yml`, `internals.yml` and graph-count include.

Do not treat generic zero-filled mappings as physical input. `gg_ggg` currently
sets `mapping_ready = .false.` and refuses integration.

## Stable topology IDs

The catalogues use the classified numbering introduced on 6 October 2026.
`phase_space/topologies/NUMBERING.md` and the JSON/CSV migration tables record
that change. IDs are stable references, not merely a display order.

For legacy extraction, select the shared catalogue explicitly:

```sh
python3 tools/convert_legacy_phase_space.py /path/to/legacy.f90 \
  --catalogue-dir phase_space/topologies --output-dir /path/to/review_yaml
```

Review `conversion_report.json` before installing the output. Without a
catalogue option the converter retains legacy IDs. Recipe differences require
explicit review; do not silently accept changed sampling bounds or daughter
order. See `tools/convert_legacy_phase_space.md` for the converter contract.
