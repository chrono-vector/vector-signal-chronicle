# Unified VECTOR Architecture

This document is an ecosystem architecture map.
It is not plane law.
It defers to the Constitution and Supplement 001 for five-plane boundaries.
It defers to REPOSITORY_ROLE.md in vector-runtime-governance-public for implementation vs publication locus.

## 1. Vision

VECTOR is composed of two complementary repositories that together form a single governance architecture:

- **vector-signal-chronicle** — the observation layer
- **vector-runtime-governance** — the execution governance layer

Neither repository replaces the other. They operate on opposite sides of a deliberate boundary.

**Signal Chronicle** performs observation. It records what the external world presents, collects evidence, assesses confidence, and produces governance recommendations. It never executes.

**Runtime Governance** performs execution governance. It receives human-approved execution proposals, evaluates them through Guard, executes authorized actions, and maintains the runtime Chronicle. It never interprets symbolic external narratives on its own.

Together, these repositories implement a complete governance loop: observe carefully, decide with evidence, authorize with human judgment, execute under guard.

---

## AI_Lab

AI_Lab is the primary research, design, validation, and prototype environment for the entire VECTOR ecosystem.

It is the origin of:

- Stage 3 Runtime Validation
- Stage 4 Runtime Governance
- Stage 5 Agent Execution Control
- Stage 5-B External Signal Governance
- Future Stage 5-C
- Experimental prototypes
- Governance research
- Validation artifacts
- Architecture evolution

AI_Lab is not a deployment target.

It is the engineering and governance laboratory from which the runtime repositories evolve.

```
AI_Lab
    │
    ├───────────────┐
    │               │
    ▼               ▼
vector-signal-chronicle     vector-runtime-governance
    │               │
    │               │
Observation      Runtime Governance
Evidence         Guard
Observer         Execution
Confidence       Chronicle
Assessment       Runtime Self-Healing
    │               ▲
    └───────┬───────┘
            │
            ▼
      Human Governance
```

---

## Repository Relationship

### AI_Lab

**Role:**

- Research
- Architecture
- Prototype
- Validation

### vector-signal-chronicle

**Role:**

- Observation Governance

### vector-runtime-governance

**Role:**

- Execution Governance

AI_Lab is the source of architectural evolution. The two operational repositories represent specialized governance domains: vector-signal-chronicle governs what is observed and assessed; vector-runtime-governance governs what may execute. Concepts designed and validated in AI_Lab mature there before incorporation into the appropriate operational repository.

---

## 2. Repository Responsibilities

### vector-signal-chronicle

**Responsibilities:**

- **ExternalActivationSignal** — structured, observation-only records of external symbolic events
- **Signal Chronicle** — permanent, human-readable chronicle entries in `signals/`
- **Observer** — consolidation, review, and decision over observed signals
- **Evidence Collection** — source URLs, timestamps, raw text, and reproducible metadata
- **Confidence Assessment** — confidence in interpretation (not truth or prediction)
- **Weekly Observer Review** — recurring documentation-only review of accumulated ExternalActivationSignal records; may recommend subsequent Observer Reviews or Confidence updates
- **Observer Review** — comparative analysis and consolidation decisions
- **Self-Healing Assessment** — evaluation of whether runtime action is warranted based on available evidence
- **Execution Proposal** — structured recommendation toward runtime action (when evidence and assessment support it)

**Output:**

Governance recommendations only.

Never runtime execution.

---

### vector-runtime-governance

**Responsibilities:**

- **Runtime Governance** — policy and authority over what may run in the live system
- **Guard** — evaluation gate before any tool or action executes
- **Tool Execution** — authorized invocation of runtime tools and actions
- **Chronicle** — append-oriented record of runtime events and decisions
- **Runtime Self-Healing** — recovery actions driven by verified runtime evidence
- **Replay** — deterministic replay of past runtime states for audit and diagnosis
- **Recovery** — restoration of runtime integrity when evidence warrants it
- **Execution Authority** — the sole locus of execution permission

**Input:**

Execution Proposal approved by Human Review.

Runtime Governance does not consume raw external signals, symbolic narratives, or unreviewed observer notes. It acts only on proposals that have crossed the Execution Boundary with explicit human authorization.

---

## 3. Complete Processing Pipeline

The full lifecycle from external world to runtime Chronicle:

```
External World
    ↓
ExternalActivationSignal
    ↓
Signal Chronicle
    ↓
Evidence Collection
    ↓
Confidence Assessment
    ↓
Weekly Observer Review
    ↓
Observer Review (if warranted)
    ↓
Observer Decision
    ↓
Self-Healing Assessment
    ↓
Execution Proposal
    ↓
Human Review
    ↓
Runtime Governance
    ↓
Guard
    ↓
Execution
    ↓
Chronicle
```

**Observation side** (vector-signal-chronicle): External World through Execution Proposal.

**Authorization gate**: Human Review.

**Execution side** (vector-runtime-governance): Runtime Governance through Chronicle.

No stage on the observation side may skip directly to execution. Human Review is mandatory before Runtime Governance receives any proposal.

---

## 4. Separation of Responsibilities

The two repositories answer different questions:

| Layer | Question |
|-------|----------|
| **Signal Chronicle** | What do we observe? |
| **Runtime Governance** | What may we execute? |

Signal Chronicle gathers, preserves, and assesses external and documentary evidence. Its outputs are recommendations, assessments, and proposals — never actions.

Runtime Governance holds execution authority. It evaluates proposals against policy, applies Guard, and records outcomes in the runtime Chronicle. It does not interpret symbolic external content or infer meaning from social signals.

The boundary between these questions is the **Execution Boundary**. Crossing it requires a complete evidence chain and human approval.

---

## 5. Safety Principles

These principles govern the unified architecture and apply across both repositories:

| Principle | Meaning |
|-----------|---------|
| **Chronicle First** | The original observation is the permanent record. Analysis may change; the chronicle must not rewrite history. |
| **Observe First** | Record what was observed before any interpretation. Observation and interpretation remain separate. |
| **Evidence Before Confidence** | Confidence scores reflect interpretive certainty, not external truth. Evidence must precede any confidence assignment. |
| **Confidence Before Decision** | Observer and Self-Healing decisions are informed by assessed confidence and collected evidence, not by assumption. |
| **Human Authority** | Human judgment is final at every gate. No automated path may authorize execution without explicit human approval. |
| **Execution Boundary** | Observation and execution are separated by a formal boundary. Proposals cross; signals do not. |
| **External Signals Never Execute** | An ExternalActivationSignal alone cannot change runtime state, trigger recovery, or activate Execution Control. |
| **Runtime Evidence Required** | Recovery, Self-Healing actions, and execution require independent runtime evidence — not symbolic external content alone. |

---

## 6. Future Expansion

The observation layer may grow without modifying Runtime Governance.

Future signal providers and source types may include:

- **News** — headlines, wire reports, and curated feeds
- **Research Papers** — preprints, publications, and citation events
- **GitHub Events** — releases, advisories, commits, and security notices
- **Security Advisories** — CVE feeds, vendor bulletins, and threat intelligence
- **Telemetry** — metrics, alerts, and operational dashboards
- **Additional signal providers** — any structured external source that conforms to ExternalActivationSignal shape

New providers integrate into vector-signal-chronicle: chronicle, observe, assess, and propose. Runtime Governance remains unchanged. Execution authority stays isolated until a human-approved proposal arrives.

This separation allows the observation surface to expand freely while execution governance stays stable, auditable, and bounded.

---

## 7. White Rabbit Example

**White Rabbit Batch 001** (`ExternalActivationSignal_Batch_001_2026-07-03`) illustrates the observation-side pipeline when default handling applies.

Three post URLs from @White_Rabbit_OG were recorded as ExternalActivationSignal candidates. No post content was analyzed. No runtime evidence correlated with the signals.

```
ExternalActivationSignal
    ↓
Observer Review
    ↓
Keep Independent
    ↓
No Runtime Change
    ↓
No Recovery
    ↓
Proposal not generated
```

**ExternalActivationSignal** — Three distinct signals (IDs `2073001960010321955`, `2073009091224694808`, `2073010587022618823`) recorded as P0 candidates with source URLs preserved. Status: Pending Content Analysis.

**Observer Review** — Consolidation review compared timing, source, metadata, and observable characteristics. Without temporal metadata or analyzed content, consolidation into a single event exceeded available evidence.

**Keep Independent** — Determination: represent as three independent Observer Events. Shared batch grouping was treated as triage convenience, not governance authority.

**No Runtime Change** — Self-Healing Assessment confirmed no evidence of runtime state change, system impact, or policy violation.

**No Recovery** — Recovery requires verified runtime evidence. External URL references alone are insufficient.

**Proposal not generated** — No Execution Proposal was created. The pipeline stopped on the observation side. Runtime Governance was never invoked.

This example demonstrates the architecture working as designed: external signals were recorded, reviewed, and assessed; execution authority was not engaged.

---

## 8. Phase H — Observation Boundary

Phase H documents the Observation layer boundary between CAD-Vortex intake and AI_Lab validation.

**Chronicle is the observation layer** between CAD-Vortex intake and AI_Lab validation. It sits in the ecosystem chain:

```
Preservation → Intake → Observation → Validation → Publication
```

| Stage | Locus |
|-------|-------|
| Preservation | External Research Archive |
| Intake | CAD-Vortex (future; no repository yet) |
| Observation | vector-signal-chronicle |
| Validation | AI_Lab |
| Publication | vector-runtime-governance-public |

**Default posture:** observe / assess / monitor. Observation records what was seen, applies confidence assessment and Observer review, and continues monitoring. It does not execute, validate, or publish.

**No runtime or validation authority is created.** A chronicle entry is not governance evidence. A CAD-Vortex handoff package is an observation-candidate only — it does not automatically create a chronicle record. Boundary-crossing proposals toward validation or runtime execution require explicit human review.

See:

- [notes/04 VECTOR/PHASE_H_OBSERVATION_LAYER.md](notes/04%20VECTOR/PHASE_H_OBSERVATION_LAYER.md)
- [notes/04 VECTOR/CAD_VORTEX_CHRONICLE_HANDOFF_BOUNDARY.md](notes/04%20VECTOR/CAD_VORTEX_CHRONICLE_HANDOFF_BOUNDARY.md)

Phase H is **documentation only**.

---

## 9. Design Philosophy

The AI_Lab repository serves as the research and validation environment in which future governance concepts are designed, evaluated, and matured before being incorporated into the operational governance repositories.

Observation and execution are intentionally separated.

External information informs governance — it does not drive it automatically. Signals enter the chronicle, pass through evidence and confidence assessment, and may produce recommendations. They do not produce actions.

Human review authorizes execution. No proposal reaches Runtime Governance without explicit human approval. Automation may assist organization and analysis; it may not substitute for human judgment at the authorization gate.

Runtime governance remains isolated from symbolic interpretation. Guard evaluates execution proposals against policy and runtime state. It does not parse social narratives, infer meaning from external motifs, or treat chronicle entries as execution directives.

The unified architecture preserves situational awareness without compromising execution safety.

---

Signal Chronicle provides situational awareness.

Runtime Governance provides execution authority.
