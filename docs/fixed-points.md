---
layout: default
title: Fixed points and amplitude checks
---

# Fixed points and amplitude checks

Generate physical momenta once, save them, and evaluate the same points with
FDMC and an independently exported MG5 calculation. This isolates amplitude
checks from numerical integration and phase-space sampling differences. No Cuba
or BASES backend is needed. The public samples are `emep_emepz` and
`mummup_vmvmxttxh`; unfinished gluon integrations are not distributed.

## Generate and evaluate 100 reusable points

Run from the software root:

```sh
cmake -S . -B build-points -DCMAKE_BUILD_TYPE=Release   -DFDMC_FORTRAN_BUILD_BASES=OFF -DFDMC_FORTRAN_ENABLE_CUBA=OFF
cmake --build build-points -j 4
mkdir -p runs/points
python3 tools/fdmc-points --process emep_emepz --build-dir build-points generate   --param-card processes/emep_emepz/cards/param_card.dat   --generator rambo --sqrts 1000 --points 100 --seed 20261015 --stream 1   --output runs/points/emep.dat
python3 tools/fdmc-points --process emep_emepz --build-dir build-points evaluate   --param-card processes/emep_emepz/cards/param_card.dat   --input runs/points/emep.dat --output runs/points/emep.fd.values.dat   --label FD --details
```

RAMBO produces check points independently of diagram mappings; its recorded
weight is zero, indicating that no integration weight was computed. For a
completed FDMC mapping, `--generator channel --graph 5` instead exercises that
channel. A seed and stream reproduce the sequence; the generation sidecar saves
successful random vectors. Use `--random-file FILE` for explicit vectors, one
per line. Values must be strictly between 0 and 1. An invalid explicit vector
fails rather than being replaced. The emep graph-5 channel accepts a five-entry
vector `0.5 0.5 0.5 0.5 0.5`; all-0.5 RAMBO input is degenerate and rejected.
Output files are protected against overwriting.

The common `FDMC_POINTS_V1` file records PDG order and `E, px, py, pz` in GeV
at high precision. Reading checks momentum conservation, mass shells, finite
values and external ordering. It never repairs an invalid point. The launcher
records parameter/point fingerprints and isolates MG5's `param.log` in a
temporary working directory.

## What the amplitude output means

| Record | Meaning |
| --- | --- |
| `HEL` | One explicit helicity tuple's colour-summed squared amplitude before division by MG5 `IDEN` |
| `TOTAL` | Exhaustive helicity sum divided by the exported MG5 `IDEN` |
| `SMATRIX` | The MG5 standalone entry-point result, corresponding to the value printed by `check_sa.f` |
| `AMP`, `JAMP` with `--details` | Complex diagram and colour-basis amplitudes for diagnosis |
| `P`, `RESIDUAL` | Actual momenta and conservation/mass-shell residuals |

All helicities are evaluated by default. `--helicity INDEX` selects one MG5
helicity-table entry and omits `TOTAL` and `SMATRIX`. Do not apply spin, colour
or identical-particle factors again: use the exported `IDEN`. Squared matrix
elements have units `GeV^(8-2*NEXTERNAL)`; no flux, SDE/channel weight or fb
conversion is included. Individual `AMP`/`JAMP` values depend on gauge, colour
basis and wavefunction phases and are not the physical cross-gauge comparator.

## Compare with an independent MG5 export

Generate a standalone reference with the same physical model, restrictions,
external states, masses, widths and couplings, explicitly choosing the intended
gauge (for example `set gauge unitary`). Verify its proc card; a directory name
is not evidence of the gauge. Then:

```sh
python3 tools/build_mg5_point_reference.py /path/to/unitary_standalone   --pdgs processes/emep_emepz/cards/external_particles.dat   --build-dir /tmp/emep-unitary-reference
python3 tools/fdmc-points --process emep_emepz   --executable /tmp/emep-unitary-reference/build/mg5_points evaluate   --param-card processes/emep_emepz/cards/param_card.dat   --input runs/points/emep.dat --output runs/points/emep.unitary.values.dat   --label unitary
python3 tools/compare_point_values.py   runs/points/emep.fd.values.dat runs/points/emep.unitary.values.dat   --rtol 1e-8 --atol 1e-20 --report runs/points/comparison.json
```

The builder compiles the unmodified reference matrix, model and HELAS separately;
it does not link FDMC's transformed matrix. Select `--subprocess P1_...` when
necessary. The comparator checks momenta, helicity tuples, normalization,
completion and available fingerprints, then compares `HEL`, `TOTAL`, `SMATRIX`.
Failures identify the point/helicity and return a nonzero exit status. Absolute
tolerances have the corresponding matrix-element units; do not silently loosen
tolerances to hide failures.

## Using MG5's check_sa.f

The original MG5 standalone `check_sa.f` is a useful single-point sanity check.
In the inspected export it initializes `param_card.dat`, reads `PS.input` when
present (one `E px py pz` row per external particle in MG5 order), otherwise
generates a RAMBO point, and prints `SMATRIX`. Use the original standalone
export's build/run instructions and colocated model cards. The copied
`src/check_sa.f` in FDMC is provenance material, not a normal CMake executable.

`FDMC_POINTS_V1` is not a `PS.input` file: omit its header, PDG/point records and
particle indices when transferring one point into MG5's four-column input.
Retain full precision and the same parameter card. For repeated comparisons,
the reference builder above avoids manual conversion and runs both evaluators
on the same common file. The original random `check_sa.f` run alone does not
establish same-point agreement.

## Known validation limits

The 15 October local fixed-point study found finite-electron-mass `emep_emepz`
helicity mismatches despite close summed results. A separate massless-electron
fixture passed; it does not validate the default finite-mass case. The cause is
unresolved and the default cards/tolerances remain unchanged. The mummup point
checks establish generation and internal consistency, but a new independent
unitary-gauge comparison was not performed for that process. Fixed-point checks
do not establish integration convergence or cross-section accuracy.
