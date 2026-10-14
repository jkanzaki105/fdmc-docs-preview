---
layout: default
title: Integration and job management
---

# Integration and job management

`fdmc-run` turns one energy/configuration/parameter choice into independent graph
jobs. `fdmc-scan` expands many such choices and manages them together. Cards,
seeds, task logs and results stay attached to their run, so you can distribute
sampling effort across cores, see what remains, and recover failures without
rerunning successful graphs. These are local process managers, not a cluster
scheduler; `--jobs` caps concurrent graph processes.

## Build and prepare

Install Cuba separately and build from the software root:

```sh
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release   -DFDMC_FORTRAN_BUILD_BASES=OFF -DFDMC_FORTRAN_ENABLE_CUBA=ON   -DCUBA_ROOT=/path/to/cuba/prefix
cmake --build build -j 4
source ./setup.sh
cd processes/emep_emepz
```

Cuba defaults to OFF. Set `cores=0` in `cards/run_card.dat` for externally
parallel graph tasks; `cores=1` would request a Cuba worker. Public source omits
BASES. Backend-unavailable requests fail explicitly.

## One run: create, execute, inspect, collect

```sh
fdmc-run create runs/example   --run-card cards/run_card.dat --param-card cards/param_card.dat   --amp-config cards/amplitude_config_list.dat --ident-card cards/ident_card.dat   --sqrts 1000 --config 0
fdmc-run execute runs/example   --executable "$FDMC_ROOT/build/processes/emep_emepz/emep_emepz_fortran_driver" --jobs 4
fdmc-run status runs/example
fdmc-run finalize runs/example
```

| Command | What it does | Why it helps with many jobs |
| --- | --- | --- |
| `create` | Snapshots inputs and assigns graph tasks with reproducible seeds | Keeps every job tied to the same calculation definition |
| `execute --jobs 4` | Runs at most four graph processes concurrently; skips completed tasks on re-execution | Uses available cores and resumes pending work |
| `status` | Counts complete, failed, running and pending tasks; lists failed/pending graphs | Reveals what needs attention without opening every log |
| `retry --executable ... --jobs 4` | Selects failed tasks for another attempt | Avoids recomputing successful graphs |
| `finalize` | Verifies required graph coverage, results, input fingerprints and a consistent executable, then aggregates | Prevents an incomplete set of jobs from becoming a formal total |

A typical status line is `READY_TO_FINALIZE: 16/16 complete, 0 failed, 0 running,
0 pending`. This state means graph jobs are done; `finalize` still needs to
collect them. After successful collection the state becomes `COMPLETE` and
`fdmc_result.json` contains the integral/error in fb and per-graph evidence.
When graphs are missing or failed, finalization returns failure and writes
`retry_graphs.txt` instead of accepting a complete total. Use `execute` for
pending work and `retry` for failed work:

```sh
fdmc-run retry runs/example   --executable "$FDMC_ROOT/build/processes/emep_emepz/emep_emepz_fortran_driver" --jobs 4
fdmc-run status runs/example
fdmc-run finalize runs/example
```

Inspect failed-task logs before retrying: an unchanged retry does not cure
insufficient statistics or a physics error. Inputs are immutable snapshots;
use a new run directory when changing cards, accuracy settings or the executable.
Run/scan management verifies provenance and completeness, not physical correctness.

For reusable settings, `fdmc-run create runs/example --spec cards/single_run.json`
accepts the same `inputs` object as a scan, plus scalar `sqrts`, `configuration`,
`seed` and `integrator`. CLI overrides take priority over JSON and then the run
card. Paths inside JSON are relative to that JSON file; direct card arguments
are relative to the shell's current directory.

## A scan: many energies or parameter choices

Save this as `cards/energy_scan.json`:

```json
{
  "schema_version": 1,
  "inputs": {
    "run_card": "run_card.dat",
    "param_card": "param_card.dat",
    "amplitude_config": "amplitude_config_list.dat",
    "ident_card": "ident_card.dat"
  },
  "integrator": "cuba-vegas",
  "seed": 12345,
  "energies": [250.0, 500.0, 1000.0, 3000.0],
  "configurations": [0]
}
```

```sh
fdmc-scan create cards/energy_scan.json scans/energy_001
fdmc-scan execute scans/energy_001   --executable "$FDMC_ROOT/build/processes/emep_emepz/emep_emepz_fortran_driver" --jobs 4
fdmc-scan status scans/energy_001
fdmc-scan finalize scans/energy_001
```

With 16 graph tasks per energy this manages 64 tasks across four runs, while
`--jobs 4` limits total concurrency across the scan. `status` reports run/task
progress; `retry` targets failed tasks across runs, and repeated `execute`
resumes pending tasks. You do not need a separate shell command per graph or
energy. Model-parameter points and axes use the same machinery; start from
`examples/parameter_scan.json` and adapt cards and allowed values for your model.

Scan finalization calls the run checks and writes `fdmc_scan_result.json` with
one result per energy/configuration/parameter point. If any runs are incomplete,
it records `status: incomplete` and lists them; this file must not be interpreted
as a complete scan. Distinct energies, parameter points and alternative amplitude
configurations are separate results and are not summed into one cross section.

Within a completed run, independent graph estimates are summed and statistical
errors combined in quadrature. Cuba status 0 means the requested accuracy was
met. Check nonzero status, failed graphs and convergence before interpreting a
physical prediction. The earlier [integration validation record](validation-20261013.html)
describes operation tests and remaining high-statistics comparisons.

See [Tools](tools.html) and the software `tools/README.md` for full options,
input path rules and process-local examples. Source-only distribution does not
bundle Cuba; linked binaries need the relevant LGPL compliance materials.
