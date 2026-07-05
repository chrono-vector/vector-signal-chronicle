# vector-signal-chronicle

This repository stores external symbolic signals, observations, and research notes collected as part of the VECTOR signal integration effort.

## Purpose

Each entry is a chronicle: a structured record of what was observed, where it came from, and how it was interpreted at the time. The repository exists to organize and preserve these observations for future reference and analysis.

## Important principles

- **Observations are descriptive only.** Entries document what was seen, heard, or noted — not conclusions about what is true.
- **No factual claims are made.** Nothing in this repository should be read as asserting the accuracy or reality of external narratives.
- **Human judgment is always authoritative.** Automated tools, templates, and scoring systems may assist organization, but a person always makes the final call on meaning and significance.

## Structure

- `signals/` — Individual signal chronicles, one Markdown file per observation.
- `templates/` — Reusable templates for new chronicle entries.
- `notes/04 VECTOR/` — Architecture and phase documentation for the VECTOR ecosystem.

## Documentation / Architecture

- [UNIFIED_VECTOR_ARCHITECTURE.md](UNIFIED_VECTOR_ARCHITECTURE.md) — Ecosystem architecture map.
- [GOVERNANCE_CONTRACT.md](GOVERNANCE_CONTRACT.md) — Governance principles for this repository.
- [notes/04 VECTOR/STAGE5B_EXTERNAL_ACTIVATION_SIGNAL_SPEC.md](notes/04%20VECTOR/STAGE5B_EXTERNAL_ACTIVATION_SIGNAL_SPEC.md) — Stage5-B External Activation Signal spec (documentation only).
- [notes/04 VECTOR/PHASE_H_OBSERVATION_LAYER.md](notes/04%20VECTOR/PHASE_H_OBSERVATION_LAYER.md) — Phase H Observation layer boundary (documentation only).
- [notes/04 VECTOR/CAD_VORTEX_CHRONICLE_HANDOFF_BOUNDARY.md](notes/04%20VECTOR/CAD_VORTEX_CHRONICLE_HANDOFF_BOUNDARY.md) — CAD-Vortex → Chronicle handoff boundary (documentation only).

**Phase H is documentation only.** No runtime code, intake implementation, or validation fixtures are created by Phase H.

## Usage

Copy `templates/signal_template.md` when creating a new signal chronicle, fill in the fields, and save the file under `signals/` using a descriptive filename (e.g. `YYYY-MM-DD_source_keyword.md`).
