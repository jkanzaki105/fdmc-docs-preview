---
layout: default
title: Tools and workflows
---

# Tools and workflows

Choose a tool by the result you need: import a process, prepare mappings, check
amplitudes at reproducible physical points, or manage many integration jobs.
Commands below run from the Fortran software checkout, not this documentation
repository. Use `source ./setup.sh` at the software root to set `FDMC_ROOT` and
add executable tools to `PATH`; in a process directory use `source ../../setup.sh`.
Python tools can be called as `python3 "$FDMC_ROOT/tools/<name>.py" --help`.

| Tool | What it makes easier | Typical use |
| --- | --- | --- |
| `fdmc-points` | Generate momenta once and replay exactly the same points in multiple amplitude evaluators | `fdmc-points --process emep_emepz --build-dir build generate ...`, then `evaluate ...` |
| `compare_point_values.py` | Find the point and helicity responsible for an amplitude mismatch | `python3 tools/compare_point_values.py fd.values.dat mg5.values.dat --report comparison.json` |
| `build_mg5_point_reference.py` | Build an independent evaluator from an unmodified MG5 standalone export | `python3 tools/build_mg5_point_reference.py /path/to/standalone --pdgs processes/emep_emepz/cards/external_particles.dat --build-dir /tmp/mg5-reference` |
| `fdmc-run` | Split one calculation into graph tasks, run a bounded number concurrently, resume and collect results | `fdmc-run create runs/example --spec cards/single_run.json`; `execute`, `status`, `retry`, `finalize` |
| `fdmc-scan` | Expand energies, configurations and model-parameter points into managed runs | `fdmc-scan create cards/energy_scan.json scans/example`; the same four management commands |
| `mg5_fdmc_adapter.py` | Import MG5 files and generate amplitude adapters, dimensions and process scaffolding | `python3 tools/mg5_fdmc_adapter.py /path/to/standalone processes/new_process --external-pdgs ...` |
| `generate_phase_space.py` | Validate YAML and generate shared or process phase-space Fortran | `--topology ... --phase-space-output ...`, or `--mapping ... --internals ... --mapping-output ...` |
| `convert_legacy_phase_space.py` | Recover YAML and a review report from supported legacy Fortran | `python3 tools/convert_legacy_phase_space.py legacy.f90 --catalogue-dir phase_space/topologies --output-dir generated/legacy_yaml` |
| `audit_update.py` | Check whether an earlier update ZIP is applied without changing the checkout | `python3 tools/audit_update.py update.zip /path/to/fortran` |
| `publication/export_public.py` | Prepare a fresh BASES-free source checkout containing reviewed public samples | `python3 tools/publication/export_public.py /path/to/new-public-checkout` |
| `publication/guard.py` | Check all reachable Git history before publishing source | `python3 tools/publication/guard.py`; also installed as the public pre-push hook |

`fdmc_manager.py` implements run/scan management and `generate_point_adapter.py`
provides the process ABI used by CMake and the reference builder. Normally use
the entry points above rather than invoking these internal helpers.

See [Fixed points and amplitude checks](fixed-points.html),
[Integration and job management](integration.html), [MG5 interface](mg5-interface.html)
and [Phase space](phase-space.html) for complete commands and review requirements.
The software repository's `tools/README.md` contains the detailed source-tree guide.
